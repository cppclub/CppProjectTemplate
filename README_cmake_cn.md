# CMake 教程

## 生成项目
使用以下命令生成项目：
```
cmake [<options>] -S <源代码路径> -B <构建路径>
```
假设根目录中有 `CMakeLists.txt` 文件，可以像下面这样生成项目。

```bash
mkdir build
cd build
cmake -S .. -B . # 选项 1
cmake .. # 选项 2
```
如果您已经构建过 CMake 项目，可以更新生成的项目。

```bash
cd build
cmake .
```

## GCC 和 Clang 的生成器
```bash
cd build
cmake -S .. -B . -G "Unix Makefiles" # 选项 1
cmake .. -G "Unix Makefiles" # 选项 2
```

## MSVC 的生成器
```bash
cd build
cmake -S .. -B . -G "Visual Studio 16 2019" # 选项 1
cmake .. -G "Visual Studio 16 2019" # 选项 2
```

## 获取生成器列表
```bash
cmake --help
```

## 指定构建类型
默认情况下，标准类型通常是调试类型。如果要生成发布模式的项目，需要设置构建类型。

```bash
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
```

## 传递选项
如果您在 `CMakeLists.txt` 中设置了一些选项，可以在命令行中传递值。

```bash
cd build
cmake -DMY_OPTION=[ON|OFF] ..
```

## 指定构建目标（选项 1）
标准构建命令会构建 `CMakeLists.txt` 中创建的所有目标。如果要构建特定目标，可以这样做。

```bash
cd build
cmake --build . --target ExternalLibraries_Executable
```
`ExternalLibraries_Executable` 是可能的目标名称的示例。注意：所有依赖的目标会在此之前构建。

## 指定构建目标（选项 2）
除了在 `cmake build` 命令中设置目标外，您还可以运行之前生成的 Makefile。如果要构建 `ExternalLibraries_Executable`，可以执行以下命令。

```bash
cd build
make ExternalLibraries_Executable
```

## 运行可执行文件
在生成项目并构建特定目标后，您可能想要运行可执行文件。在默认情况下，可执行文件存储在 `build/5_ExternalLibraries/app/ExternalLibraries_Executable` 中，假设您正在构建项目 `5_ExternalLibraries`，可执行文件的主文件位于 `app` 目录中。

```bash
cd build
./bin/ExternalLibraries_Executable
```

## 不同的链接类型
```cmake
add_library(A ...)
add_library(B ...)
add_library(C ...)
```
### PUBLIC
```cmake
target_link_libraries(A PUBLIC B)
target_link_libraries(C PUBLIC A)
```
当 A 将 B 作为 PUBLIC 链接时，它表示 A 在实现中使用 B，且 B 也在 A 的公共 API 中使用。因此，C 可以使用 B，因为它是 A 的公共 API 的一部分。

### PRIVATE
```cmake
target_link_libraries(A PRIVATE B)
target_link_libraries(C PRIVATE A)
```
当 A 将 B 作为 PRIVATE 链接时，表示 A 在实现中使用 B，但 B 没有在 A 的公共 API 中使用。任何调用 A 的代码都不需要直接引用 B 中的任何内容。

### INTERFACE
```cmake
add_library(D INTERFACE)
target_include_directories(D INTERFACE {CMAKE_CURRENT_SOURCE_DIR}/include)
```
通常用于仅包含头文件的库。

## 不同的库类型
- **库**：包含有关代码的信息的二进制文件。库不能独立执行，应用程序利用库。
- **共享库**：
  - Linux: `*.so`
  - MacOS: `*.dylib`
  - Windows: `*.dll`
  
  共享库减少了每个使用库的程序中重复的代码量，使二进制文件保持较小。然而，共享库在执行时会有少量额外的成本。通常共享库与可执行文件在同一目录中。

- **静态库**：
  - Linux/MacOS: `*.a`
  - Windows: `*.lib`
  
  静态库增加了二进制文件的整体大小，但意味着您不需要携带使用的库的副本。由于代码在编译时连接，因此没有额外的运行时加载成本。

## CMake 路径的重要变量
- **CMAKE_SOURCE_DIR**：包含 `CMakeLists.txt` 文件的最上层文件夹（源代码目录）。
- **PROJECT_SOURCE_DIR**：包含项目源代码目录根路径的完整路径。
- **CMAKE_CURRENT_SOURCE_DIR**：当前处理的 `CMakeLists.txt` 所在的目录。
- **CMAKE_CURRENT_LIST_DIR**：当前正在处理的列表文件的目录（例如，一个 `*.cmake` 模块）。
- **CMAKE_MODULE_PATH**：告诉 CMake 在使用 `FIND_PACKAGE()` 或 `INCLUDE()` 时首先在 `CMAKE_MODULE_PATH` 列出的目录中搜索。
- **CMAKE_BINARY_DIR**：构建目录的文件路径。

## 可以在目标上设置的内容
- `target_link_libraries`：其他目标；也可以直接传递库名称。
- `target_include_directories`：包含目录。
- `target_compile_features`：需要激活的编译器特性，例如 `cxx_std_11`。
- `target_compile_definitions`：定义。
- `target_compile_options`：更一般的编译标志。
- `target_link_directories`：不使用，最好给出完整路径（CMake 3.13+）。
- `target_link_options`：一般链接标志（CMake 3.13+）。
- `target_sources`：添加源文件。

## 自定义目标和命令
### 何时需要使用 `add_custom_target`？
每次需要运行一个命令来执行构建系统中不同于构建库或可执行文件的操作时。

### 何时使用 `add_custom_command`？
每当我们希望在需要时运行命令时，例如需要生成一个文件（或多个文件）或在源文件夹中某些内容更改时重新生成它。

### 何时使用 `execute_process`？
在配置时运行命令。

## 宏与函数
宏和函数之间唯一的区别是作用域；宏没有作用域。因此，如果您在函数中设置了一个变量并希望它在外部可见，需要使用 `PARENT_SCOPE`。

## 生成器表达式
使用生成器表达式，可以为多配置生成器中的不同构建类型配置项目。对于这种生成器，项目在运行 `cmake` 时配置一次，但可以为多个构建类型构建。例如，Visual Studio 就是这种生成器。

对于多配置生成器，`CMAKE_BUILD_TYPE` 在配置阶段不可用。因此，使用 if-else 切换不会起作用：

```cmake
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_compile_options("/W4 /Wx")
elseif(CMAKE_BUILD_TYPE STREQUAL "Release")
    add_compile_options("/W4")
endif()
```
但使用条件生成器表达式有效：

```cmake
add_compile_options(
    $<$<CONFIG:Debug>:/W4 /Wx>
    $<$<CONFIG:Release>:/W4>
)
```

Visual Studio、XCode 和 Ninja 多配置生成器允许在同一构建目录中拥有多个配置，因此不会使用 `CMAKE_BUILD_TYPE` 缓存变量。相反，使用 `CMAKE_CONFIGURATION_TYPES` 缓存变量，该变量包含该构建目录要使用的配置列表。

## 使用工具链文件进行交叉编译
### ARM 32 交叉编译
```bash
cmake -B build_arm32 -DCMAKE_TOOLCHAIN_FILE=cmake/toolchains/arm32-cross-toolchain.cmake
cmake --build build_arm32 -j8
```

### ARM 32 本地编译
```bash
cmake -B build -DCMAKE_TOOLCHAIN_FILE=cmake/toolchains/arm32-native-toolchain.cmake
cmake --build build -j8
```

### x86 64 MingW
```bash
cmake -B build_mingw -DCMAKE_TOOLCHAIN_FILE=cmake/toolchains/x86-64-mingw-toolchain.cmake
cmake --build build_mingw -j8
```

### x86 64 本地编译
```bash
cmake -B build -DCMAKE_TOOLCHAIN_FILE=cmake/toolchains/x86-64-native-toolchain.cmake
cmake --build build -j8
```
