可以用`${<NAME>}`获得变量代表的路径

#### `CMAKE_SOURCE_DIR`：
- 项目源代码的根目录路径，即包含顶级 `CMakeLists.txt` 文件的目录路径。

#### `CMAKE_CURRENT_SOURCE_DIR`：
- 表示当前`CMakeLists.txt`文件的目录

#### `BASE_DIR`：
- 资源文件中需要的文件会在`${BASE_DIR}`下寻找
```
qt_add_resources(GameResources "resources.qrc"
    BASE_DIR "${CMAKE_SOURCE_DIR}/TankVerseClient"
    PREFIX "/"
)
```

#### `PROJECT_NAME`：
- 在`project()`中定义的项目名