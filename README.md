这些库（-lasan 和 -lubsan）并不是所有 GCC 发行版都默认自带的。AddressSanitizer (ASan) 和 UndefinedBehaviorSanitizer (UBSan) 是 GCC 提供的两种动态分析工具，用于检测程序中的内存错误和未定义行为。虽然 GCC 支持这些工具，但在某些发行版或编译器包中，它们可能作为可选组件提供，需要单独安装或通过特定的编译选项启用。

具体来说：

AddressSanitizer (-lasan)：用于检测诸如缓冲区溢出、使用未初始化内存、释放后重用、双重释放等内存错误。要在 GCC 中启用 ASan，通常需要使用 -fsanitize=address 编译选项，并确保链接到 ASan 库（即 -lasan）。某些 GCC 发行版可能默认不包含 ASan 库，需要通过额外的软件包管理命令或从源码编译 GCC 时明确选择包含 ASan 组件来安装。

UndefinedBehaviorSanitizer (-lubsan)：用于检测可能导致未定义行为的代码片段，如整数溢出、无效类型转换、未初始化的变量使用、无效的函数指针解引用等。启用 UBSan 需要使用 -fsanitize=undefined 编译选项，并链接到 UBSan 库（即 -lubsan）。同 ASan 一样，UBSan 也可能不是所有 GCC 发行版的默认组成部分，需要根据具体情况安装。

综上所述，如果您在编译过程中遇到找不到 -lasan 或 -lubsan 的错误，可能的原因包括：

使用的 GCC 版本未包含 ASan 和 UBSan 组件。
虽然 GCC 版本支持 ASan 和 UBSan，但这些组件在安装时未被选择或未正确安装。
构建系统（如 CMake 或 Makefile）未能正确配置链接路径，导致链接器无法找到已安装的 ASan 和 UBSan 库。
为解决这些问题，您可以采取以下措施：

确认 GCC 版本：检查所使用的 GCC 是否支持 ASan 和 UBSan。现代 GCC 版本通常支持这两种工具，但较旧版本可能不包含。如果需要，考虑升级到支持 ASan 和 UBSan 的 GCC 版本。

检查 GCC 安装：确认 GCC 是否已正确安装 ASan 和 UBSan 组件。如果是通过包管理器安装的，查阅相关文档了解如何安装包含 ASan 和 UBSan 的 GCC 软件包。如果是从源码编译安装，确保在配置阶段启用了 ASan 和 UBSan 支持。

配置构建系统：如果 ASan 和 UBSan 已经安装，但链接器仍无法找到，检查项目构建系统的配置（如 CMakeLists.txt 或 Makefile），确保为链接器指定了正确的库搜索路径（如 -L/path/to/libsanitizer）以及链接选项（即 -lasan 和 -lubsan）。

完成上述检查和调整后，重新构建项目应该能够成功链接 ASan 和 UBSan 库，从而避免出现“cannot find -lasan”和“cannot find -lubsan”的错误。


# Simulator project for LVGL embedded GUI Library

The [LVGL](https://github.com/lvgl/lvgl) is written mainly for microcontrollers and embedded systems however you can run the library **on your PC** as well without any embedded hardware. The code written on PC can be simply copied when your are using an embedded system.

## Requirements
This project is configured for [VSCode](https://code.visualstudio.com) and only tested on Linux, although this may work on OSx or WSL. It requires a working version of GCC, GDB and make in your path.

To allow debugging inside VSCode you will also require a GDB [extension](https://marketplace.visualstudio.com/items?itemName=webfreak.debug) or other suitable debugger. All the requirements, build and debug settings have been pre-configured in the [.workspace](simulator.code-workspace) file (simply open the project by doubleclick on this file).

The project can use **SDL** but it can be easily relaced by any other built-in LVGL dirvers.

## Usage

### Get the PC project

Clone the PC project and the related sub modules:

```bash
git clone --recursive https://github.com/lvgl/lv_port_pc_vscode
```

### Install graphics driver

Please make sure **SDL**  is installed in the system:

#### Install SDL
You can download SDL from https://www.libsdl.org/

On on Linux you can install it via terminal:
```bash
sudo apt-get update && sudo apt-get install -y build-essential libsdl2-dev
```

### Optional library
There are also FreeType and FFmpeg support. You can install FreeType support with:
```bash
# FreeType support
wget https://kumisystems.dl.sourceforge.net/project/freetype/freetype2/2.13.2/freetype-2.13.2.tar.xz
tar -xf freetype-2.13.2.tar.xz
cd freetype-2.13.2
make
make install
```

The FFmpeg support can be installed with:
```bash
# FFmpeg support
git clone https://git.ffmpeg.org/ffmpeg.git ffmpeg
cd ffmpeg
git checkout release/6.0
./configure --disable-all --disable-autodetect --disable-podpages --disable-asm --enable-avcodec --enable-avformat --enable-decoders --enable-encoders --enable-demuxers --enable-parsers --enable-protocol='file' --enable-swscale --enable-zlib
make
sudo make install
```
