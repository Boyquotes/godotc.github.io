# 如何让clangd理解你的项目，并以你的想法进行静态检查与格式化

## 前言

- clangd 是一个提供了format, static diagnose, completion 的语言服务器
- 在使用时是可以项目具体要求来编写配置文件来配置的
- 配置的方式有很多，系统的`config.yaml`或者项目下的`.clangd`等等...
- 上期有很多朋友提到clangd无法正常地补全...

## 1. 如何处理找不到标准库头文件
- 安装编译器gcc/clang/msvc/mingw, 或者安装标准库libc++
- 添加clangd 启动参数 : `--quert-driver`
- 在配置文件中添加头文件包含目录, compile_flags.txt
- https://clangd.llvm.org/troubleshooting#lots-of-error-diagnostics

## 2. 如何让clangd认识到你的项目结构

- 简单一些，使用CMake来一键配置
- 在配置文件中来写:
	- `<project>/.clangd`
	- `~/.config/clangd/config.yaml`
	- `<project>/compile_commands.json` 
>https://clang.llvm.org/docs/JSONCompilationDatabase.html

## 3. 代码风格/静态检查

- Diagnose: 
	- https://clangd.llvm.org/config#diagnostics
- Format:
	- https://clang.llvm.org/docs/ClangFormat.html
	- https://www.cnblogs.com/oloroso/p/14699855.html