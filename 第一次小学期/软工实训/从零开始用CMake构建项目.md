### 从零开始：在 VS Code 中创建 CMake C++ 项目

#### **第 0 步：环境准备（一次性工作）**

在开始之前，确保你的电脑上安装了以下四样东西：

1.  **C++ 编译器**:
    *   **Windows 推荐**: 安装 **Visual Studio Community** 并确保在安装时勾选了“**使用 C++ 的桌面开发**”工作负载。这将为你安装 MSVC 编译器、Windows SDK 等所有必需品。
    *   **或者 (跨平台选择)**: 安装 **MinGW-w64**。可以从 [MSYS2](https://www.msys2.org/) 安装，或者从其他发行版网站下载。安装后，务必将其 `bin` 目录（例如 `C:\mingw64\bin`）添加到系统的 `PATH` 环境变量中。

2.  **CMake**:
    *   从 [CMake 官网](https://cmake.org/download/) 下载并安装最新版本。
    *   在安装过程中，确保勾选“**Add CMake to the system PATH for all users**”或“for current user”。

3.  **Visual Studio Code (VS Code)**:
    *   从 [VS Code 官网](https://code.visualstudio.com/) 下载并安装。

4.  **VS Code 扩展**:
    *   打开 VS Code，进入扩展视图（`Ctrl+Shift+X`）。
    *   搜索并安装以下两个**必装**扩展：
        *   **C/C++**: 由 Microsoft 提供，提供语言支持（代码补全、语法高亮）。
        *   **CMake Tools**: 由 Microsoft 提供，提供 CMake 项目的完整集成（配置、构建、调试）。

---

#### **第 1 步：创建项目文件夹和源文件**

1.  在你的电脑上找一个**纯英文路径**的位置，创建一个新的文件夹作为你的项目根目录。例如：`D:\dev\projects\MyFirstCMakeProject`。
2.  用 VS Code 打开这个文件夹 (`File` -> `Open Folder...`)。
3.  在 VS Code 的文件浏览器中，新建一个 C++ 源文件，命名为 `main.cpp`。
4.  在 `main.cpp` 中写入一个简单的 "Hello World" 程序：

    ```cpp
    #include <iostream>

    int main() {
        std::cout << "Hello, CMake World!" << std::endl;
        return 0;
    }
    ```

---

#### **第 2 步：编写 `CMakeLists.txt` 文件**

这是 CMake 项目的“心脏”，它告诉 CMake 如何构建你的项目。

1.  在项目的根目录下（与 `main.cpp` 同级），新建一个文件，**严格命名**为 `CMakeLists.txt`。
2.  在 `CMakeLists.txt` 文件中写入以下基本内容：

    ```cmake
    # 指定 CMake 的最低版本要求
    cmake_minimum_required(VERSION 3.10)

    # 定义项目名称和语言
    project(MyFirstCMakeProject LANGUAGES CXX)

    # 添加一个可执行文件目标
    # 语法: add_executable(<目标名> <源文件1> <源文件2> ...)
    add_executable(my_app main.cpp)
    ```
    *   `project(...)`: 定义了项目名。
    *   `add_executable(...)`: `my_app` 是你想要生成的可执行文件的名字，`main.cpp` 是它的源文件。

---

#### **第 3 步：配置 CMake (选择编译器)**

当你保存 `CMakeLists.txt` 文件后，CMake Tools 扩展通常会自动激活。

1.  VS Code 可能会在右下角弹窗询问“Do you want to configure project...? (是否配置项目?)”，点击“**Yes**”。
2.  接着，它会让你“**Select a Kit** (选择一个工具包)”。Kit 就是编译器工具链。
    *   如果你安装了 Visual Studio，你会看到类似 `Visual Studio Community 2022 Release - amd64` 的选项。
    *   如果你安装了 MinGW-w64，你会看到类似 `GCC 11.2.0 ...` 的选项。
    *   **选择一个你想要使用的编译器**。对于初学者，选择 Visual Studio 的 Kit 通常最省心。

3.  选择 Kit 后，CMake Tools 会在后台自动运行 `cmake` 命令进行配置。你可以在底部的“**输出 (Output)**”面板中，切换到 “**CMake**” 频道查看详细日志。如果最后看到 `[proc] configure done` 或类似信息，就说明配置成功了。
    *   **如果错过了自动弹窗怎么办？** 按 `Ctrl+Shift+P` 打开命令面板，输入 `CMake: Select a Kit` 来手动选择，然后输入 `CMake: Configure` 来手动配置。

---

#### **第 4 步：构建项目 (编译)**

配置成功后，你的构建目录（通常是 `build`）已经创建好了。现在可以编译了。

*   **最简单的方式**: 点击 VS Code 底部状态栏上的 **`Build`** 按钮（通常是一个方块或者写着 "Build"）。
*   **快捷键**: 按 **`F7`**。
*   **命令面板**: 按 `Ctrl+Shift+P`，输入并选择 `CMake: Build`。

构建成功后，你会在“输出”面板中看到 `[build] build finished with exit code 0`。

---

#### **第 5 步：配置运行与调试 (`launch.json`)**

现在程序已经编译好了，我们需要告诉 VS Code 如何去运行和调试它。

1.  切换到 VS Code 左侧边栏的“**运行和调试**”视图。
2.  你可能会看到一个提示“to customize Run and Debug, create a launch.json file (要自定义运行和调试，请创建 launch.json 文件)”。点击蓝色的“**create a launch.json file**”链接。
3.  在弹出的选择环境菜单中，选择 **`C++ (Windows)`** (如果你用的 MSVC) 或 **`C++ (GDB/LLDB)`** (如果你用的 MinGW/GCC)。
4.  VS Code 会自动生成一个 `.vscode/launch.json` 文件。**现在需要检查并修改它**。

    **关键是修改 `"program"` 字段！**
    *   默认生成的路径可能是错的。你需要把它改成你**真实**的可执行文件的路径。
    *   根据你在第 3 步选择的 Kit 和第 4 步的构建，可执行文件通常在：
        *   `build/Debug/my_app.exe` (使用 Visual Studio Kit)
        *   `build/my_app.exe` (使用 MinGW Kit)
    *   **请亲自去 `build` 目录里确认一下！**

    **修改后的 `launch.json` 示例 (用于 Visual Studio/MSVC):**
    ```json
    {
        "version": "0.2.0",
        "configurations": [
            {
                "name": "(Windows) Launch",
                "type": "cppvsdbg",
                "request": "launch",
                "program": "${workspaceFolder}/build/Debug/my_app.exe", // <-- 修改这里！
                "args": [],
                "stopAtEntry": false,
                "cwd": "${workspaceFolder}",
                "environment": [],
                "console": "externalTerminal" // 弹出独立的黑窗口终端
            }
        ]
    }
    ```

---

#### **第 6 步：运行和调试！**

一切准备就绪！

1.  **切换回你的 `main.cpp` 文件** (确保活动窗口是代码文件)。
2.  在 `main.cpp` 的 `std::cout` 那一行左边点击，可以设置一个**红点断点**。
3.  **按 `F5` 启动调试**。
    *   程序会开始运行，并在你设置的断点处暂停。
    *   你可以查看变量、单步执行代码 (`F10`, `F11`)。
4.  **按 `Ctrl + F5` 直接运行** (不调试)。
    *   程序会直接运行到底，你会在弹出的终端窗口里看到输出 "Hello, CMake World!"。

恭喜你！你已经成功地在 VS Code 中从零开始创建、配置、构建并运行了一个 CMake C++ 项目。后续添加新的源文件，只需要在 `CMakeLists.txt` 的 `add_executable()` 中把新文件名加上去，然后重新构建即可。