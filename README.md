# Main-stream Platform ABIs and function-calling conventions on most of Processor Architectures and Operating Systems
各主流处理器与系统平台的ABI和函数调用约定，外加系统平台预定义的宏

<br />

## 汇编语言相关

- [Using as——The GNU Assembler](http://ftp.gnu.org/old-gnu/Manuals/gas-2.9.1/html_chapter/as_toc.html)
- [AT&T Syntax versus Intel Syntax](http://ftp.gnu.org/old-gnu/Manuals/gas-2.9.1/html_chapter/as_16.html#SEC198)
- [Oracle Solaris x86 Assembly Language Reference Manual](https://docs.oracle.com/cd/E36784_01/html/E36859/enmzx.html#scrolltoc)
- In GNU Assembly Language, use Intel syntax for x86 ISA: `.intel_syntax noprefix`
- In GNU Assembly Language, use AT&T syntax for x86 ISA: `.att_syntax`
- GNU Assembler for ARMv7, use Thumb2 instruction set: `.syntax unified` 
- [Understanding this part arm assembly code](https://stackoverflow.com/questions/22396214/understanding-this-part-arm-assembly-code)
- [Intel x87 FPU的使用基础](https://blog.csdn.net/zenny_chen/article/details/6186820)

<br />

## 各大平台的ABI与函数调用约定

- [System V ABI](https://wiki.osdev.org/System_V_ABI)
- [x86 calling conventions](https://en.wikipedia.org/wiki/X86_calling_conventions)
- [System V Application Binary Interface: AMD64 Architecture Processor Supplement (With LP64 and ILP32 Programming Models) Version 1.0](https://github.com/hjl-tools/x86-psABI/wiki/x86-64-psABI-1.0.pdf)
- [MSVC上的函数调用约定](https://docs.microsoft.com/en-us/search/index?search=calling%20convention)
- [Procedure Call Standard for the Arm® Architecture](https://developer.arm.com/docs/ihi0042/i/procedure-call-standard-for-the-arm-architecture-abi-2019q4-documentation)
- [Procedure Call Standard for the Arm® 64-bit Architecture](https://developer.arm.com/docs/ihi0055/d/procedure-call-standard-for-the-arm-64-bit-architecture)
- [Procedure Call Standard for the ARM® 64-bit Architecture (AArch64) with SVE support](https://developer.arm.com/docs/100986/latest/procedure-call-standard-for-the-arm-64-bit-architecture-aarch64-with-sve-support)
- [Cx51函数声明与调用约定](http://www.keil.com/support/man/docs/c51/c51_le_funcdecls.htm)（在Keil Cx51中，针对中断例程使用`using`指定寄存器段是无效的，`using`只作用于一般C函数。）

<br />

## 各大平台的预定义宏

- [各处理器架构的预定义宏](https://sourceforge.net/p/predef/wiki/Architectures/)
- [各操作系统的预定义宏](https://sourceforge.net/p/predef/wiki/OperatingSystems/)
- [各大编译器预定义宏](https://sourceforge.net/p/predef/wiki/Compilers/)（此外，微软的Clang加MS CodeGen后端编译器也同样支持 **_MSC_VER** 预定义宏。）
- [Unix平台标准预定义宏](https://sourceforge.net/p/predef/wiki/Standards/)
- [ARMv8 - ARM](https://en.wikichip.org/wiki/arm/armv8)
- 在与GNU C兼容的编译工具链中，`__cplusplus`宏表示当前的编译器为C++；`__OBJC__`宏表示当前的编译器为Objective-C；`__ASSEMBLER__`宏表示当前使用的是汇编器。

<br />

#### 在Apple环境下针对Apple A处理器架构的一些额外的预定义宏

首先，在所有Apple系统环境下一定有预定义宏——**`__APPLE__`**。

1. 列出指定架构的所有预定义宏：`clang -arch <架构名> -E -dM - < /dev/null`。其中 *<架构名>* 目前支持：`armv5`、`armv6`、`armv7`、`armv7s`、`arm64`。除此之外，当前macOS平台上Xcode自带的Apple LLVM编译器还支持这些架构：`i386`、`x86_64`、`ppc`、`ppc64`。
1. Apple平台下，除了支持`__ARM_ARCH_'V'__`这个标准的架构预定义宏之外，还支持`__ARM_ARCH`这个宏，这个宏后面跟着一个整数，指明当前ARM架构的大版本。比如，5表示ARMv5，8则表示ARM64。
1. Apple平台下，如果当前Apple A处理器是arm64架构的话，那么它不仅支持`__aarch64__`这个标准的宏，而且还支持`__arm64__`这个宏。
1. armv7s的版本预定义宏为：`__ARM_ARCH_7S__`。
1. arm64e架构的预定义宏为：`__arm64e__`。arm64e默认是不被构建的，当前Xcode 10的默认标准架构只有`armv7`和`arm64`这两种。如果要额外添加的话需要在编译构建选项列表中的**Architectures**一栏中**追加**`arm64e`才行。

<br />

#### 在Android系统环境下指定处理器架构

首先，在Android环境下一定有预定义宏——**`__ANDROID__`**。

由于ARMv8-A架构有多个变种，比如ARMv8、ARMv8.1、ARMv8.2、ARMv8.3、ARMv8.4等等。为了能在NDK编译环境中指定架构名，我们可以用 `-march=`命令行选项。
比如：`-march=armv8.2a`指定使用ARMv8.2-A架构。

<br />

#### MSVC下对各个处理器平台的宏定义

- x86: `_M_IX86`
- x86_64: `_M_X64`
- ARM: `_M_ARM`
- ARM64: `_M_ARM64`

<br />

## 各编译器预定义的项目工程相关的预定义宏

- Visual Studio：`$(LocalDebuggerWorkingDirectory)`表示当前工程中用于存放源文件的主目录。
- Xcode：`$(SRCROOT)`表示当前项目工程的根目录。`$(PROJECT_DIR)`所表示的工程路径与`$(SRCROOT)`一样。`$(PROJECT_NAME)`表示当前项目名称。
- [cmake-variables(7)](https://cmake.org/cmake/help/latest/manual/cmake-variables.7.html)
- CMake：`${PROJECT_SOURCE_DIR}`表示当前工程项目中用于存放源文件的主目录。在Android Studio中则是当前项目工程根目录下的**`app/src/main/cpp/`**目录。在Android Studio的cmake中，`${ANDROID_ABI}`表示当前正在编译的ABI架构名称。

