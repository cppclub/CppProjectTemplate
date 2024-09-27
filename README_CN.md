# C++ 项目模板

![C++](https://img.shields.io/badge/C%2B%2B-11%2F14%2F17%2F20%2F23-blue)
![License](https://img.shields.io/github/license/franneck94/CppProjectTemplate)
![Linux Build](https://github.com/franneck94/CppProjectTemplate/workflows/Ubuntu%20CI%20Test/badge.svg)

这是一个现代 C++ 项目的模板。您将获得：

- 库、可执行文件和测试代码分别存放在不同的文件夹中
- 使用现代 CMake 进行构建和编译
- 外部库的安装和管理：
  - [CPM](https://github.com/cpm-cmake/CPM.cmake) 包管理器 **或**
  - [Conan](https://conan.io/) 包管理器 **或**
  - [VCPKG](https://github.com/microsoft/vcpkg) 包管理器
- 使用 [Catch2](https://github.com/catchorg/Catch2) v2 进行单元测试
- 通用库：[JSON](https://github.com/nlohmann/json)、[spdlog](https://github.com/gabime/spdlog)、[cxxopts](https://github.com/jarro2783/cxxopts) 和 [fmt](https://github.com/fmtlib/fmt)
- 使用 Github Actions 和 [pre-commit](https://pre-commit.com/) 进行持续集成测试
- 使用 [Doxygen](https://doxygen.nl/) 和 [Github Pages](https://franneck94.github.io/CppProjectTemplate/) 进行代码文档生成
- 工具：Clang-Format、Cmake-Format、Clang-tidy、Sanitizers

## 结构

``` text
├── CMakeLists.txt
├── app
│   ├── CMakesLists.txt
│   └── main.cc
├── cmake
│   └── cmake 模块
├── docs
│   ├── Doxyfile
│   └── html/
├── external
│   ├── CMakesLists.txt
│   ├── ...
├── src
│   ├── CMakesLists.txt
│   ├── foo/...
│   └── bar/...
└── tests
    ├── CMakeLists.txt
    └── test_*.cc
```

库代码放在 [src/](src/)，主程序代码放在 [app/](app)，测试代码放在 [tests/](tests/)。

## 软件要求

- CMake 3.21+
- GNU Makefile
- Doxygen
- Conan 或 VCPKG
- MSVC 2017（或更高版本）、G++9（或更高版本）、Clang++9（或更高版本）
- 可选：代码覆盖率（仅在 GNU|Clang 上）：gcovr
- 可选：Makefile、Doxygen、Conan、VCPKG

## 构建

首先，克隆此仓库并进行初步工作：

```shell
git clone --recursive https://github.com/franneck94/CppProjectTemplate
mkdir build
```

- 应用可执行文件

```shell
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build . --config Release --target main
cd app
./main
```

- 单元测试

```shell
cmake -H. -Bbuild -DCMAKE_BUILD_TYPE="Debug"
cmake --build build --config Debug
cd build
ctest .
```

- 文档生成

```shell
cd build
cmake -DCMAKE_BUILD_TYPE=Debug ..
cmake --build . --config Debug --target docs
```

- 代码覆盖率（仅限 Unix）

```shell
cmake -H. -Bbuild -DCMAKE_BUILD_TYPE=Debug -DENABLE_COVERAGE=On
cmake --build build --config Debug --target coverage -j4
cd build
ctest .
```

有关 CMake 的更多信息，请参见 [这里](./README_cmake.md)。
