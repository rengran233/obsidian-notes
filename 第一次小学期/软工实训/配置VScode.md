### VS Code + CMake + Qt/C++ 项目配置与调试问题排查笔记

#### **核心原则**
*   **分清阶段**：C++ 项目开发分为**配置 (Configure)**、**构建 (Build)** 和 **运行/调试 (Launch/Debug)** 三个独立阶段。一个阶段的问题会影响下一个阶段。
*   **路径纯净**：项目文件路径应避免使用中文、空格或特殊字符，以防止工具（特别是 GDB）解析错误。
*   **环境一致**：编译器 (Compiler)、库 (Library, 如Qt) 和调试器 (Debugger, 如GDB) 的版本和架构（如 64位 MinGW）必须相互匹配。
*   **清理缓存**：当修改了核心配置（如 `CMakeLists.txt`、工具链、Kit）后，务必**删除构建目录 (`build`)** 或使用 `CMake: Delete Cache and Reconfigure` 来确保新配置生效。

---

#### **第一阶段：CMake 配置与构建问题**

1.  **错误: `Unable to get the build directory.`**
    *   **原因**: `cmake --build` 命令不知道构建目录在哪里。
    *   **解决方案**: 遵循“外部构建 (Out-of-Source Build)”标准流程：
        1.  `mkdir build`
        2.  `cd build`
        3.  `cmake ..`  (配置)
        4.  `cmake --build .` (在 `build` 目录内构建)

2.  **如何指定编译器？**
    *   **VS Code (推荐)**: 使用 `CMake: Select a Kit` (选择工具包) 命令，从列表中选择你想要的编译器（如 Visual Studio 的 MSVC 或 MinGW 的 GCC）。
    *   **命令行**: 使用 `-G` 指定生成器 (`cmake -G "MinGW Makefiles" ..`) 或用 `-D` 指定编译器路径 (`-D CMAKE_CXX_COMPILER=...`)。
    *   **专业方法**: 使用工具链文件 (`-D CMAKE_TOOLCHAIN_FILE=...`)，特别适用于交叉编译或标准化团队环境。

3.  **如何绕过 vcpkg 使用自己的 Qt？**
    *   **原因**: vcpkg 的工具链文件会“劫持”`find_package`。
    *   **解决方案**: 使用 `CMAKE_PREFIX_PATH` 变量，它的优先级高于 vcpkg。在配置时，明确告诉 CMake 你的 Qt 在哪里。
        *   **VS Code**: 在 `.vscode/settings.json` 的 `cmake.configureArgs` 中添加 `"-D", "CMAKE_PREFIX_PATH=C:/path/to/your/Qt"`。
        *   **命令行**: `cmake .. -DCMAKE_PREFIX_PATH=C:/path/to/your/Qt`。

---

#### **第二阶段：运行与调试 (`launch.json`) 问题**

1.  **错误: `launch: program '...a.exe' does not exist`**
    *   **原因**: `launch.json` 中的 `"program"` 字段指向了一个不存在的、默认的或错误的程序名（如 `a.exe`）。
    *   **解决方案**: 修改 `launch.json`，将 `"program"` 的值改为**真实**生成的可执行文件的**完整路径**。使用 `${workspaceFolder}` 变量是最佳实践。
        *   **示例**: `"program": "${workspaceFolder}/build/Debug/my_app.exe"`。
        *   **注意**: 务必亲自去 `build` 目录确认可执行文件的真实位置和名字。

2.  **错误: `You don't have an extension for debugging 'JSON with Comments'.`**
    *   **原因**: 按下 `F5` 时，当前活动窗口是 `.json` 文件（如 `launch.json`），导致 VS Code 试图调试 JSON 文件本身。
    *   **解决方案**: 在按 `F5` 之前，**先用鼠标点击一下你的任意 C++ 源代码文件 (`.cpp`)**，把焦点切换回来。

3.  **选择正确的调试配置**
    *   `launch.json` 可以包含多个配置（如用于 MSVC 的 `(Windows) Launch` 和用于 GDB 的 `(gdb) Launch`）。
    *   **如何选择**: 在“运行和调试”侧边栏顶部的下拉菜单中，**明确选择**你想要使用的那一个。

4.  **GDB 调试配置 (`(gdb) Launch`) 关键点**
    *   **编译器 vs 调试器**: `g++` 是**编译器**，`gdb` 是**调试器**。
    *   `"type"`: 必须是 `"cppdbg"`。
    *   `"miDebuggerPath"`: **必须**设置为你电脑上 `gdb.exe` 的**完整、正确路径**。例如: `"miDebuggerPath": "C:/mingw64/bin/gdb.exe"`。
    *   **错误: `The value of miDebuggerPath is invalid`**: 就是因为这个路径没填对。

---

#### **第三阶段：运行时环境问题 (DLL 找不到)**

1.  **错误: `During startup program exited with code 0xc0000135.`**
    *   **原因**: 这是 Windows 的 `STATUS_DLL_NOT_FOUND` 错误。程序启动时找不到依赖的动态链接库 (`.dll` 文件)。对于 MinGW+Qt 项目，通常是**找不到 Qt 的 DLL 和 MinGW 的运行时 DLL**。
    *   **连环错误**: 这个底层错误会导致 GDB 报告 `program exited with code 0`，并最终导致 VS Code 报告 `GDB exited unexpectedly`。**`0xc0000135` 是根本原因**。

2.  **根源: `PATH` 环境变量问题**
    *   Windows 只在特定位置（包括系统 `PATH` 环境变量里的所有目录）查找 DLL。
    *   **验证方法**: 在 VS Code 的终端里运行 `echo $env:PATH` (PowerShell) 或 `where Qt6Widgets.dll` 来检查当前环境中的 `PATH` 是否正确。

3.  **最终诊断: 【环境冲突】**
    *   **现象**: 系统 `PATH` 中存在多个不同版本的 MinGW/GCC 工具链。
    *   **案发过程**: 程序由 **A 版本**的 `g++` 编译，但在运行时，系统从 `PATH` 里先找到了 **B 版本**的 DLL。版本不匹配导致程序启动时直接崩溃。

4.  **解决方案**:
    *   **方法一 (推荐，立刻见效)**: 在 `launch.json` 的 `"environment"` 字段中，**强制指定正确的 PATH 顺序**，将与项目匹配的 Qt 和 MinGW 的 `bin` 目录放在**最前面**。
        ```json
        "environment": [
            {
                "name": "PATH",
                "value": "D:/path/to/correct/Qt/bin;D:/path/to/correct/MinGW/bin;${env:PATH}"
            }
        ]
        ```
    *   **方法二 (长久之计)**: 整理系统的环境变量，删除多余、冲突的 MinGW/GCC 路径，只保留一个主版本。

---

希望这份笔记能帮助你未来在遇到类似问题时，能快速、系统地定位并解决。祝你编码愉快！