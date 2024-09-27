# Tools

To sum up all the tools we use:

- Compiler warnings: fast checks while compiling the code, for the all target.
- Clang-tidy, CppCheck: linters, can be manually run at any time after their specific targets are built.
- Sanitizers: shows memory leaks in runtime. Built with the all target.
- LTO: applies linking optimization in release mode. Automatically works at compile/linking time for all target
- Doxygen: generates HTML documentation. It can be run apart after build its specific target.
- Clang-format and Cmake-format: allows automatically format the code and CMake files. They can be run apart after build their specific targets.

# 工具

工具总结如下：

- **编译器警告**：在编译代码时进行快速检查，适用于所有目标。
- **Clang-tidy、CppCheck**：代码检查工具，可以在特定目标构建后手动运行。
- **Sanitizers**：在运行时显示内存泄漏。与所有目标一起构建。
- **LTO**（链接时间优化）：在发布模式下应用链接优化。自动在编译/链接时为所有目标工作。
- **Doxygen**：生成HTML文档。可以在构建特定目标后单独运行。
- **Clang-format 和 Cmake-format**：自动格式化代码和CMake文件。可以在构建特定目标后单独运行。
