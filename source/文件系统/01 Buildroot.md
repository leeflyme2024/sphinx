[Buildroot 开发 — Firefly Wiki](https://wiki.t-firefly.com/zh_CN/Core-PX30-JD4/buildroot_develop.html)

# 版本号
```bash
mkdir -p /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/tisdk-buildroot-2024.05/output/target/etc
( \
        echo "NAME=Buildroot"; \
        echo "VERSION=2024.05.3"; \
        echo "ID=buildroot"; \
        echo "VERSION_ID=2024.05.3"; \
        echo "PRETTY_NAME=\"Buildroot 2024.05.3\"" \
) >  /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/tisdk-buildroot-2024.05/output/target/usr/lib/os-release
```

```bash
root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/tisdk-buildroot-2024.05/output# cat target/usr/lib/os-release
NAME=Buildroot
VERSION=2024.05.3
ID=buildroot
VERSION_ID=2024.05.3
PRETTY_NAME="Buildroot 2024.05.3"


root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/targetNFS# cat usr/lib/os-release
NAME=Buildroot
VERSION=2024.05.3
ID=buildroot
VERSION_ID=2024.05.3
PRETTY_NAME="Buildroot 2024.05.3"
```

# 编译过程
```bash
-rw-r--r--   1 root root       0 Feb 19 08:17 .stamp_downloaded
-rw-r--r--   1 root root       0 Feb 19 08:17 .stamp_extracted
-rw-r--r--   1 root root       0 Feb 19 08:17 .stamp_patched
-rw-r--r--   1 root root       0 Feb 19 08:18 .stamp_configured
-rw-r--r--   1 root root       0 Feb 19 08:18 .stamp_built
-rw-r--r--   1 root root       0 Feb 19 08:18 .stamp_staging_installed
-rw-r--r--   1 root root       0 Feb 19 08:18 .stamp_installed
-rw-r--r--   1 root root       0 Feb 19 08:18 .stamp_target_installed
```
```bash
1739953071.813039045:start:download            : cairo
1739953072.093483631:end  :download            : cairo
1739953072.107733338:start:extract             : cairo
1739953073.978457215:end  :extract             : cairo
1739953073.991570346:start:patch               : cairo
1739953074.086461100:end  :patch               : cairo
1739953074.103145823:start:configure           : cairo
1739953084.960794398:end  :configure           : cairo
1739953084.975159738:start:build               : cairo
1739953101.836503070:end  :build               : cairo
1739953101.873043056:start:install-staging     : cairo
1739953103.400684483:end  :install-staging     : cairo
1739953103.416647493:start:install-target      : cairo
1739953104.144126645:end  :install-target      : cairo
```

这些日志记录显示了 Buildroot 在构建 cairo 包时各个阶段的开始与结束时间（时间戳通常是自某个固定时间点的秒数）。下面对每个阶段做详细说明：

1. **download 阶段**
    
    - **开始与结束时间**：
        - 开始：1739953071.813039045
        - 结束：1739953072.093483631
    - **说明**：在这一阶段，Buildroot 会从指定的源（比如官方网站、Git 仓库或镜像服务器）下载 cairo 的源代码压缩包。下载完成后，源代码文件就被保存在本地下载目录中。
2. **extract 阶段**
    
    - **开始与结束时间**：
        - 开始：1739953072.107733338
        - 结束：1739953073.978457215
    - **说明**：下载完成后，构建系统会解压缩（提取）下载的压缩包，把源代码和相关文件恢复到一个工作目录中，供后续编译使用。
3. **patch 阶段**
    
    - **开始与结束时间**：
        - 开始：1739953073.991570346
        - 结束：1739953074.086461100
    - **说明**：在这个阶段，Buildroot 会对提取出来的源码应用预定义的补丁（patch）。这些补丁可能是为了修复 bug、添加特定功能或调整代码以适应目标平台环境。
4. **configure 阶段**
    
    - **开始与结束时间**：
        - 开始：1739953074.103145823
        - 结束：1739953084.960794398
    - **说明**：这一阶段运行 cairo 的配置脚本（通常是 `./configure`），根据当前的编译环境、目标平台和用户配置选项生成适合交叉编译的 Makefile 和其他配置文件。配置阶段会检测系统中所需的库、头文件以及编译器特性，确保后续编译能够顺利进行。
5. **build 阶段**
    
    - **开始与结束时间**：
        - 开始：1739953084.975159738
        - 结束：1739953101.836503070
    - **说明**：配置完成后，Buildroot 调用 make 命令开始编译源码。这个阶段会把源代码编译成目标文件、库文件以及可执行文件。日志显示这个阶段耗时较长，因为编译工作量通常比较大。
6. **install-staging 阶段**
    
    - **开始与结束时间**：
        - 开始：1739953101.873043056
        - 结束：1739953103.400684483
    - **说明**：安装阶段的“staging”部分是指将编译好的文件安装到一个中间目录（通常称为 sysroot 或 staging area），这个目录用于集成后续构建其他依赖该包的软件。这一步确保库和头文件能够被后续的包正确引用。
7. **install-target 阶段**
    
    - **开始与结束时间**：
        - 开始：1739953103.416647493
        - 结束：1739953104.144126645
    - **说明**：最后，Buildroot 将已经安装到 staging 区域的文件部署到最终目标文件系统中。这个阶段会把最终的二进制文件、库、配置文件等复制到目标根文件系统，供最终运行时使用。

总的来说，上述过程展示了从下载源码到最终将编译好的 cairo 软件包安装到目标系统的整个自动化构建流程。每个阶段都有明确的开始与结束时间，帮助开发者了解整个构建过程的耗时情况，同时也便于调试和性能分析。

# 配置文件
```bash
-rw-r--r--   1 root root     164 Feb 19 08:18 cairo-egl-uninstalled.pc
-rw-r--r--   1 root root     190 Feb 19 08:18 cairo-fc-uninstalled.pc
-rw-r--r--   1 root root     187 Feb 19 08:18 cairo-ft-uninstalled.pc
-rw-r--r--   1 root root     183 Feb 19 08:18 cairo-glesv2-uninstalled.pc
-rw-r--r--   1 root root     224 Feb 19 08:18 cairo-gobject-uninstalled.pc
-rw-r--r--   1 root root     167 Feb 19 08:18 cairo-png-uninstalled.pc
-rw-r--r--   1 root root     345 Feb 19 08:18 cairo-uninstalled.pc
```
这些 `.pc` 文件是 pkg‑config 的配置文件，它们包含了编译和链接 Cairo 各个模块所需的路径、库和编译选项信息。具体说明如下：

- **用途**
    - 当其他程序或库依赖 Cairo 时，编译系统可以通过 pkg‑config 工具读取这些文件，从而获得正确的头文件路径（includedir）、库文件路径（libdir）以及编译和链接选项。
    - 例如，执行命令 `pkg-config --cflags cairo` 或 `pkg-config --libs cairo` 会返回相应的编译标志和链接参数。
- **uninstalled 后缀的意义**
    - “uninstalled” 表示这些 `.pc` 文件用于指向当前构建（但尚未安装到系统目录）的库文件和头文件。这样在构建依赖该库的其他软件时，可以直接使用构建目录中的内容，而不必等待安装到最终的系统位置。
    - 每个文件对应 Cairo 的不同组件，比如：
        - `cairo-egl-uninstalled.pc`：用于 EGL 后端相关信息。
        - `cairo-fc-uninstalled.pc`：用于 FreeType/Fontconfig 相关信息。
        - `cairo-ft-uninstalled.pc`：用于 FreeType 字体支持。
        - `cairo-glesv2-uninstalled.pc`：用于 OpenGLESv2 后端。
        - `cairo-gobject-uninstalled.pc`：用于 GObject 集成支持。
        - `cairo-png-uninstalled.pc`：用于 PNG 图片支持。
        - `cairo-uninstalled.pc`：总体的 Cairo 配置文件，可能包含公共的编译和链接参数。

总的来说，这些 `.pc` 文件为开发者和构建系统提供了一种统一的方式来查询 Cairo 库的构建信息，从而确保在编译和链接时使用正确的选项。

## Autotools（配置信息）
```bash
-rw-r--r--   1 root root  306392 Feb 19 08:18 config.log
-rwxr-xr-x   1 root root  288623 Feb 19 08:18 config.status*
-rwxrwxr-x   1 root root 1300369 Nov 28  2020 configure*
-rw-rw-r--   1 root root   30680 Nov 26  2020 configure.ac
```

这些文件都是由 GNU Autotools 系统生成或使用的，用于配置和记录构建过程的细节。具体说明如下：
- **configure.ac**  
    这是 Autoconf 的输入文件，包含了项目的配置信息、测试宏以及如何检测目标系统特性。开发者在这里定义需要检测的库、函数、编译器选项等，之后由 autoconf 生成最终的 configure 脚本。
- **configure**  
    由 configure.ac 经过 autoconf 处理生成的 shell 脚本。它会在项目构建前运行，自动检测当前系统环境、库依赖、头文件路径等，并生成 Makefile 和其他必要的配置文件，使得项目能在该环境下正确编译。
- **config.status**  
    这是 configure 脚本运行后生成的脚本，用于记录配置过程中做出的决策和生成的文件状态。它可以在需要重新生成某些配置文件时被再次调用，确保配置结果的一致性。
- **config.log**  
    记录了 configure 脚本执行时的详细输出、检测过程和错误信息。当配置过程中遇到问题时，可以查看这个日志文件来排查问题，了解哪些检测失败或出现了警告。

总的来说，这些文件协同工作，使得源代码可以根据不同的系统环境自动调整编译选项和依赖，从而实现跨平台的构建。

# 交叉编译工具链
```bash
root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/tisdk-buildroot-2024.05/output# ll host/bin/aarch64-none-linux-gnu*
lrwxrwxrwx 1 root root 232 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-addr2line -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-addr2line*
lrwxrwxrwx 1 root root 225 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-ar -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-ar*
lrwxrwxrwx 1 root root 225 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-as -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-as*
lrwxrwxrwx 1 root root  17 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-c++ -> toolchain-wrapper*
lrwxrwxrwx 1 root root 230 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-c++filt -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-c++filt*
lrwxrwxrwx 1 root root  17 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-cpp -> toolchain-wrapper*
lrwxrwxrwx 1 root root 226 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-dwp -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-dwp*
lrwxrwxrwx 1 root root 230 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-elfedit -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-elfedit*
lrwxrwxrwx 1 root root  17 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-g++ -> toolchain-wrapper*
lrwxrwxrwx 1 root root  17 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gcc -> toolchain-wrapper*
lrwxrwxrwx 1 root root  17 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gcc-13.3.1 -> toolchain-wrapper*
lrwxrwxrwx 1 root root 229 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gcc-ar -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gcc-ar*
lrwxrwxrwx 1 root root 229 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gcc-nm -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gcc-nm*
lrwxrwxrwx 1 root root 233 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gcc-ranlib -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gcc-ranlib*
lrwxrwxrwx 1 root root 227 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gcov -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gcov*
lrwxrwxrwx 1 root root 232 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gcov-dump -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gcov-dump*
lrwxrwxrwx 1 root root 232 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gcov-tool -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gcov-tool*
lrwxrwxrwx 1 root root 226 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gdb -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gdb*
lrwxrwxrwx 1 root root 236 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gdb-add-index -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gdb-add-index*
lrwxrwxrwx 1 root root 239 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gdb-add-index-py -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gdb-add-index-py*
lrwxrwxrwx 1 root root 229 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gdb-py -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gdb-py*
lrwxrwxrwx 1 root root  17 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gfortran -> toolchain-wrapper*
lrwxrwxrwx 1 root root 228 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-gprof -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gprof*
lrwxrwxrwx 1 root root 225 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-ld -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-ld*
lrwxrwxrwx 1 root root 229 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-ld.bfd -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-ld.bfd*
lrwxrwxrwx 1 root root 230 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-ld.gold -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-ld.gold*
lrwxrwxrwx 1 root root 231 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-lto-dump -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-lto-dump*
lrwxrwxrwx 1 root root 225 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-nm -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-nm*
lrwxrwxrwx 1 root root 230 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-objcopy -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-objcopy*
lrwxrwxrwx 1 root root 230 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-objdump -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-objdump*
lrwxrwxrwx 1 root root 229 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-ranlib -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-ranlib*
lrwxrwxrwx 1 root root 230 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-readelf -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-readelf*
lrwxrwxrwx 1 root root 227 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-size -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-size*
lrwxrwxrwx 1 root root 230 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-strings -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-strings*
lrwxrwxrwx 1 root root 228 Feb 19 07:50 host/bin/aarch64-none-linux-gnu-strip -> /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/crosstool/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-strip*

```

# 外部补丁
`rootfs-overlay` 是 Buildroot 提供的一项非常实用的功能，允许开发者在目标文件系统编译完成后，将指定的文件或目录覆盖到根文件系统的特定位置。通过这种方式，可以轻松地向根文件系统中添加或修改文件，而无需直接修改源码或打补丁 [[1]]。

以下是如何在根文件系统的 `/etc/` 目录下添加文件 `overlay-test` 的具体操作步骤：

---

### 1. **设置 rootfs-overlay 根目录**
- 打开 Buildroot 的配置菜单：
  ```bash
  make menuconfig
  ```
- 在配置界面中，找到并设置 `BR2_ROOTFS_OVERLAY` 选项，指定用于覆盖的根目录。对于 `px30` 平台，默认已添加了目录：
  ```
  board/rockchip/px30/fs-overlay-64
  ```
  如果需要自定义路径，可以在这里修改为其他目录 [[5]]。

---

### 2. **创建 overlay 文件结构**
- 在指定的 `rootfs-overlay` 根目录下，创建与目标文件系统相同的目录结构。例如，要将文件添加到 `/etc/` 目录下，可以在 `fs-overlay-64` 中创建对应的目录：
  ```bash
  mkdir -p board/rockchip/px30/fs-overlay-64/etc
  ```

---

### 3. **添加目标文件**
- 将需要添加的文件（如 `overlay-test`）放入对应的目录中。例如：
  ```bash
  echo "This is a test file" > board/rockchip/px30/fs-overlay-64/etc/overlay-test
  ```
  这会在目标文件系统的 `/etc/` 目录下生成一个名为 `overlay-test` 的文件，内容为 `"This is a test file"` [[1]]。

---

### 4. **重新编译根文件系统**
- 完成上述配置后，重新编译 Buildroot：
  ```bash
  make
  ```
  编译完成后，Buildroot 会自动将 `fs-overlay-64` 中的内容覆盖到目标文件系统中 [[4]]。

---

### 5. **验证结果**
- 编译完成后，检查生成的目标文件系统（通常位于 `output/target/` 或打包后的镜像文件），确认 `/etc/overlay-test` 文件是否正确生成：
  ```bash
  ls output/target/etc/
  cat output/target/etc/overlay-test
  ```
  输出应显示：
  ```
  This is a test file
  ```

---

### 总结
通过 `rootfs-overlay` 功能，可以方便地向根文件系统中添加或修改文件，而无需修改源码或重新配置软件包。这种方法特别适合需要快速定制文件系统的场景 [[1]]。
# 软件包类型
在 Buildroot 中，支持多种格式的软件包，每种格式适用于不同类型的项目和构建需求。以下是 Buildroot 支持的主要软件包格式及其区别：

---

### 1. **`generic-package`**
- **适用场景**：通用格式，适合没有特定构建系统的软件包。
- **特点**：
  - 需要手动定义编译、安装等步骤。
  - 灵活性高，适合自定义逻辑。
  - 示例：简单的命令行工具或脚本 [[1]]。

---

### 2. **`autotools-package`**
- **适用场景**：使用 GNU Autotools（如 `configure` 和 `Makefile`）的软件包。
- **特点**：
  - 自动处理 `./configure`、`make` 和 `make install`。
  - 适合大多数开源项目。
  - 示例：许多传统的 Linux 工具和库 [[8]]。

---

### 3. **`cmake-package`**
- **适用场景**：使用 CMake 构建系统的软件包。
- **特点**：
  - 自动调用 `cmake` 进行配置和编译。
  - 适合现代 C/C++ 项目。
  - 示例：基于 CMake 的应用程序或库 [[4]]。

---

### 4. **`meson-package`**
- **适用场景**：使用 Meson 构建系统的软件包。
- **特点**：
  - 类似于 CMake，但更轻量级。
  - 适合需要快速构建的项目。
  - 示例：一些新兴的开源项目 [[10]]。

---

### 5. **`python-package`**
- **适用场景**：Python 软件包。
- **特点**：
  - 使用 Python 的 `setup.py` 或其他打包工具。
  - 适合纯 Python 应用程序。
  - 示例：Python 模块或脚本 [[9]]。

---

### 6. **`host-package`**
- **适用场景**：为主机端生成工具或依赖的软件包。
- **特点**：
  - 不为目标系统生成文件，而是为开发环境提供工具。
  - 示例：交叉编译工具链中的辅助工具 [[5]]。

---

### 7. **`kernel-module`**
- **适用场景**：Linux 内核模块。
- **特点**：
  - 专门用于编译内核模块。
  - 示例：设备驱动程序 [[2]]。

---

### 8. **`custom-package`**
- **适用场景**：完全自定义的软件包。
- **特点**：
  - 提供最大的灵活性。
  - 适合无法使用其他格式的特殊项目。
  - 示例：需要复杂构建逻辑的项目 [[1]]。

---

### 列表总结

| 格式名称                | 适用场景                  | 特点                                         | 示例             |
| ------------------- | --------------------- | ------------------------------------------ | -------------- |
| `generic-package`   | 通用格式                  | 手动定义编译和安装步骤，灵活性高                           | 自定义工具或脚本       |
| `autotools-package` | 使用 GNU Autotools 的软件包 | 自动处理 `./configure`、`make` 和 `make install` | 传统 Linux 工具和库  |
| `cmake-package`     | 使用 CMake 构建系统的软件包     | 自动调用 `cmake` 进行配置和编译                       | 基于 CMake 的应用程序 |
| `meson-package`     | 使用 Meson 构建系统的软件包     | 类似于 CMake，但更轻量级                            | 新兴开源项目         |
| `python-package`    | Python 软件包            | 使用 Python 的 `setup.py` 或其他打包工具             | Python 模块或脚本   |
| `host-package`      | 主机端工具                 | 为主机生成工具或依赖，不为目标系统生成文件                      | 交叉编译工具链辅助工具    |
| `kernel-module`     | Linux 内核模块            | 专门用于编译内核模块                                 | 设备驱动程序         |
| `custom-package`    | 完全自定义的软件包             | 提供最大灵活性，适合特殊项目                             | 复杂构建逻辑的项目      |

---

### 总结
Buildroot 提供了多种软件包格式，以满足不同项目的构建需求。选择合适的格式可以显著简化开发流程，并提高构建效率 [[1]]。

# 自定义软件包
在 Buildroot 中新增本地源码包是一个常见的需求，尤其是在 Buildroot 自带的软件包无法满足项目需求时。以下是如何添加一个自定义的本地源码包，并以 `generic-package` 格式为例进行说明 [[1]]。

---

### 1. **创建软件包目录**
在 Buildroot 的 `package/` 目录下为你的软件包创建一个新的子目录。例如：
```bash
mkdir package/myapp
```
这个目录将存放你的软件包配置文件和补丁（如果有）[[6]]。

---

### 2. **编写 `.mk` 文件**
在 `package/myapp/` 目录中创建一个名为 `myapp.mk` 的文件，用于定义软件包的构建规则。以下是一个基于 `generic-package` 的示例：

```makefile
MYAPP_VERSION = 1.0
MYAPP_SITE = /path/to/local/source
MYAPP_SITE_METHOD = local
MYAPP_LICENSE = GPL-2.0+
MYAPP_LICENSE_FILES = COPYING

define MYAPP_BUILD_CMDS
    $(MAKE) $(TARGET_CONFIGURE_OPTS) -C $(@D)
endef

define MYAPP_INSTALL_TARGET_CMDS
    $(INSTALL) -D -m 0755 $(@D)/myapp $(TARGET_DIR)/usr/bin/myapp
endef

$(eval $(generic-package))
```

#### 配置说明：
- **`MYAPP_VERSION`**：软件包版本号。
- **`MYAPP_SITE`**：本地源码路径。如果源码是远程下载的，则可以指定 URL。
- **`MYAPP_SITE_METHOD`**：指定源码获取方式。对于本地源码，使用 `local`。
- **`MYAPP_LICENSE` 和 `MYAPP_LICENSE_FILES`**：指定许可证类型和相关文件。
- **`MYAPP_BUILD_CMDS`**：定义编译命令。
- **`MYAPP_INSTALL_TARGET_CMDS`**：定义安装命令，将生成的二进制文件复制到目标文件系统 [[4]]。

---

### 3. **编写 `Config.in` 文件**
在同一目录下创建 `Config.in` 文件，用于在 Buildroot 的配置界面中显示该软件包。内容如下：

```kconfig
config BR2_PACKAGE_MYAPP
    bool "myapp"
    help
      This is a custom application called myapp.
```

#### 配置说明：
- **`BR2_PACKAGE_MYAPP`**：软件包的配置选项名称。
- **`bool`**：表示这是一个布尔选项（启用或禁用）。
- **`help`**：提供简要描述，帮助用户了解该软件包的作用 [[9]]。

---

### 4. **更新全局配置**
为了让 Buildroot 能够识别新添加的软件包，需要将其注册到全局配置中。编辑 `package/Config.in` 文件，添加以下内容：

```kconfig
source "package/myapp/Config.in"
```

这会将 `myapp` 的配置选项添加到 Buildroot 的菜单中 [[8]]。

---

### 5. **配置并编译**
完成上述步骤后，可以通过以下命令配置并编译 Buildroot：
```bash
make menuconfig
```
在配置界面中找到 `myapp` 并启用它。然后运行以下命令进行编译：
```bash
make
```
Buildroot 会根据 `myapp.mk` 文件中的定义自动处理源码、编译和安装过程 [[3]]。

---

### 6. **验证结果**
编译完成后，检查以下目录以确认软件包是否正确安装：
- **`output/target/usr/bin/`**：目标文件系统中的二进制文件。
- **`output/build/myapp-1.0/`**：解压后的源码和编译中间文件 [[2]]。

---

### 总结
通过以上步骤，你可以成功地将本地源码包添加到 Buildroot 中，并以 `generic-package` 格式进行管理。这种方式非常适合开发自定义应用或移植第三方软件包 [[1]]。

# 编译命令

```bash
root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/filesystem/am62xx-evm/zy/tisdk-buildroot-2024.05/src/tisdk-buildroot-2024.05/package/cairo# ll
total 152
drwxr-xr-x    2 root root   4096 Feb 18 13:37 ./
drwxr-xr-x 2864 root root 118784 Feb 18 13:37 ../
-rw-r--r--    1 root root   1213 Feb 18 13:37 0001-_arc_max_angle_for_tolerance_normalized-fix-infinite.patch
-rw-r--r--    1 root root   1758 Feb 18 13:37 0002-Fix-mask-usage-in-image-compositor.patch
-rw-r--r--    1 root root    971 Feb 18 13:37 0003-cairo-ft-private.h-fix-missing-FT_Color-error.patch
-rw-r--r--    1 root root   1170 Feb 18 13:37 Config.in
-rw-r--r--    1 root root    561 Feb 18 13:37 cairo.hash
-rw-r--r--    1 root root   4383 Feb 18 13:37 cairo.mk
```

在 Buildroot 中，常见的编译命令可以帮助用户完成从配置到生成目标文件系统的整个流程。以下是一些常用的 Buildroot 编译命令及其用途：

---

### 1. **配置相关命令**
- **`make menuconfig`**  
  进入 Buildroot 的图形化配置界面，用于选择目标架构、工具链、软件包等 [[1]]。
  
- **`make savedefconfig`**  
  保存当前的配置为默认配置文件（`defconfig`），便于后续复用 [[2]]。
	eg: 将修改保存到配置文件 `configs/rockchip_px30_64_defconfig`。
- **`make <board>_defconfig`**  
  加载特定开发板的默认配置文件。例如：
  ```bash
  make raspberrypi4_defconfig
  ```
  这会加载 Raspberry Pi 4 的默认配置 [[10]]。

---

### 2. **编译相关命令**
- **`make`**  
  编译整个 Buildroot 系统，包括工具链、内核、根文件系统等。编译完成后，生成的镜像文件会存放在 `output/images/` 目录中 [[3]]。

- **`make <package>`**  
  单独编译某个软件包。例如：
  ```bash
  make cairo
  ```
  这会下载、解压、配置、编译并安装指定的软件包（如 `cairo`）[[4]]。

- **`make <package>-rebuild`**  
  如果修改了某个软件包的源码或配置，可以使用此命令重新编译该软件包，而无需清理整个构建目录 [[6]]。

- **`make <package>-dirclean`**  
  清理指定软件包的构建缓存，删除其源码和编译结果。例如：
  ```bash
  make cairo-dirclean
  ```
  这会确保下次编译时从头开始 [[5]]。

---

### 3. **工具链相关命令**
- **`make toolchain`**  
  单独编译工具链（如果使用的是内部工具链）。外部工具链通常不需要此步骤 [[8]]。

- **`make sdk`**  
  生成一个 SDK，供其他项目使用。SDK 包含交叉编译工具链和 Buildroot 配置 [[7]]。

---

### 4. **清理与重建**
- **`make clean`**  
  清理所有已编译的目标文件，但保留配置文件和下载的源码 [[1]]。

- **`make distclean`**  
  彻底清理整个 Buildroot 构建环境，包括配置文件和下载的源码。执行后需要重新配置和编译 [[5]]。

---

### 5. **运行与调试**
- **`make qemu`**  
  使用 QEMU 模拟器运行生成的系统镜像（适用于支持 QEMU 的目标平台）[[7]]。

- **`make legal-info`**  
  生成法律信息（如许可证和版权声明），存放在 `output/legal-info/` 目录中 [[9]]。

---

### 6. **其他常用命令**
- **`make graph-depends`**  
  生成软件包依赖关系图，帮助分析软件包之间的依赖关系 [[8]]。

- **`make printvars`**  
  打印 Buildroot 的变量值，用于调试配置问题 [[8]]。

---

### 总结
以上是 Buildroot 常见的编译命令，涵盖了从配置到编译、清理和调试的完整流程。根据具体需求选择合适的命令，可以大大提高开发效率 [[1]]。


# 常用命令
在 Buildroot 中，软件包的构建过程包括下载、解压、打补丁（patch）、配置、编译和安装等步骤。以下是每个步骤的常用命令及其说明：

```bash
make <package>-<target>
```

```bash
Package-specific:
  <pkg>                  - Build and install <pkg> and all its dependencies
  <pkg>-source           - Only download the source files for <pkg>
  <pkg>-extract          - Extract <pkg> sources
  <pkg>-patch            - Apply patches to <pkg>
  <pkg>-depends          - Build <pkg>'s dependencies
  <pkg>-configure        - Build <pkg> up to the configure step
  <pkg>-build            - Build <pkg> up to the build step
  <pkg>-graph-depends    - Generate a graph of <pkg>'s dependencies
  <pkg>-dirclean         - Remove <pkg> build directory
  <pkg>-reconfigure      - Restart the build from the configure step
  <pkg>-rebuild          - Restart the build from the build step
```

```bash
make bluez5_utils-dirclean      # 清理构建目录
make bluez5_utils-source        # 下载源码（可选）
make bluez5_utils-extract       # 解压源码
make bluez5_utils-patch         # 应用补丁
make bluez5_utils-depends       # 编译依赖项
make bluez5_utils-configure     # 配置阶段
make bluez5_utils-build         # 编译阶段
make bluez5_utils-install       # 安装阶段
```

---

### 1. **下载软件包**
Buildroot 会根据 `package/<package-name>/<package-name>.mk` 文件中的定义自动下载软件包源码。如果需要手动触发下载，可以使用以下命令：

1. **下载所有配置中选定的软件包源代码：**
    
    ```bash
    make source
    ```

    此命令会根据当前配置，下载所有选定的软件包源代码到 `buildroot/dl/` 目录，但不会进行编译。 

2. **下载特定软件包的源代码：**
    
    ```bash
    make <package>-source
    ```
    
    将 `<package>` 替换为具体的软件包名称，例如 `make iperf3-source`。此命令仅下载指定软件包的源代码。 
    
3. **手动下载软件包：** 如果某个软件包无法通过上述命令下载，您可以手动下载对应的源码包，并将其放入 `buildroot/dl/` 目录中。然后，重新运行编译命令即可。
    

请注意，`make source` 和 `make <package>-source` 命令仅下载源代码，不会进行编译。在需要编译时，仍需执行 `make` 命令。

这会将软件包的源码下载到 `output/build/` 目录中 

---

### 2. **解压软件包**
下载完成后，Buildroot 会自动解压软件包。如果需要手动解压，可以使用以下命令：
```bash
make <package>-extract
```
解压后的文件会存放在 `output/build/<package>-<version>/` 目录中 [[8]]。

---

### 3. **打补丁（Patch）**
Buildroot 支持为软件包应用补丁文件。补丁文件通常存放在 `package/<package-name>/` 目录下，文件名需符合特定格式（如 `<package>-<version>*.patch`）。  
- 如果需要重新打补丁，可以清理并重新解压软件包：
  ```bash
make <package>-patch
  ```
  这会重新下载、解压并应用补丁 [[9]]。

---

### 4. **配置软件包**
配置阶段会根据软件包的 `configure` 脚本或类似工具生成 Makefile 等文件。可以通过以下命令单独执行配置：
```bash
make <package>-configure
```
配置过程中传递的参数可以通过 `CONFIGURE_ARGS` 变量定义 [[5]]。

---

### 5. **编译软件包**
编译阶段会根据配置生成的目标文件进行编译。可以通过以下命令单独编译某个软件包：
```bash
make <package>
```
编译日志会保存在 `output/build/<package>-<version>/` 目录中 [[2]]。

---

### 6. **安装软件包**
编译完成后，Buildroot 会将生成的文件安装到目标文件系统目录（`output/target/`）。可以通过以下命令单独执行安装：
```bash
make <package>-install
```
安装路径可以通过 `INSTALL_TARGET_PATH` 变量指定 [[5]]。

---

### 7. **清理与重建**
- **清理软件包**：
  如果需要清理某个软件包的构建缓存，可以使用以下命令：
  ```bash
  make <package>-dirclean
  ```
  这会删除该软件包的源码和编译结果 [[5]]。

- **重新编译软件包**：
  如果修改了软件包的源码或配置，可以使用以下命令重新编译：
  ```bash
  make <package>-rebuild
  ```

---

### 8. **其他常用命令**
- **查看依赖关系**：
  使用以下命令生成软件包依赖关系图：
  ```bash
  make graph-depends
  ```
  这有助于分析软件包之间的依赖关系 [[8]]。

- **生成 SDK**：
  如果需要为其他项目生成交叉编译工具链和配置，可以使用以下命令：
  ```bash
  make sdk
  ```

---

### 总结
Buildroot 提供了一套完整的工具链和命令，用于自动化处理软件包的下载、解压、打补丁、配置、编译和安装等步骤。通过灵活使用这些命令，可以高效地完成嵌入式系统的开发工作 [[2]]。



# 安装的文件列表
```bash
-rw-r--r--   1 root root       0 Feb 19 08:18 .files-list-host.txt
-rw-r--r--   1 root root       0 Feb 19 08:18 .files-list-images.txt
-rw-r--r--   1 root root    4694 Feb 19 08:18 .files-list-staging.txt
-rw-r--r--   1 root root    4694 Feb 19 08:18 .files-list.txt
```
这些文件是 Buildroot 在构建和安装包时生成的文件列表，用于记录在不同安装阶段中实际安装了哪些文件，方便后续打包、清理和依赖管理。具体说明如下：

- **.files-list-host.txt**  
    记录安装到“host”环境的文件（也就是在主机上运行的工具或文件）。当前为空，说明这个包没有为主机环境安装任何文件。
- **.files-list-images.txt**  
    记录直接要打包进目标镜像中的文件。如果有些文件只在最终镜像中需要，而不在 staging 区使用，则会出现在这里。当前为空，表示没有额外的仅用于镜像的文件。
- **.files-list-staging.txt**  
    记录安装到 staging 区（即交叉编译工具链的 sysroot 中）的文件，这些文件会供其他软件包编译时使用。这里有内容，说明 cairo 的库文件、头文件等被安装到了交叉编译环境中。
- **.files-list.txt**  
    记录最终安装到目标文件系统中的文件。当前它与 .files-list-staging.txt 内容相同，表示该包在 staging 区安装的文件也被直接复制到目标文件系统中。
    
总结来说，这些文件帮助 Buildroot 跟踪各阶段安装的文件情况，确保各部分（host、staging、镜像、目标系统）的文件都能被正确管理和打包。


# 最终输出
在 Buildroot 中，编译完成后生成的输出目录结构是非常有规律的，每个子目录都有特定的用途。以下是对你提供的目录说明的详细解析和补充：

---

### 1. **`build/`**
- **说明**：包含所有软件包的源码以及主机工具的构建文件。
- **用途**：
  - 这是 Buildroot 解压并编译软件包的地方。
  - 每个软件包都会有一个对应的子目录，例如 `cairo-1.16.0/`，其中包含该软件包的源码、补丁、配置文件等 [[1]]。
  - 编译日志和中间文件也会存储在这里。

---

### 2. **`host/`**
- **说明**：存放主机端编译所需的工具，包括交叉编译工具链和其他辅助工具。
- **用途**：
  - 主机工具链（如 `gcc`、`binutils`）会安装到这里。
  - 这些工具用于为目标架构生成可执行文件或库。
  - 如果需要调试主机工具，可以在这个目录中找到相关二进制文件 [[7]]。

---

### 3. **`images/`**
- **说明**：包含压缩好的根文件系统镜像文件以及其他目标镜像。
- **用途**：
  - 这是最终生成的镜像文件存放位置，例如：
    - 根文件系统镜像（如 `rootfs.tar` 或 `rootfs.ext4`）。
    - 内核镜像（如 `zImage` 或 `uImage`）。
    - 启动加载器（如 `u-boot.bin`）。
  - 这些文件可以直接烧录到目标设备上，用于部署系统 [[8]]。

---

### 4. **`staging/`**
- **说明**：类似于根文件系统的目录结构，但包含了所有开发相关的文件。
- **用途**：
  - 包含编译生成的所有头文件、库文件和其他开发资源。
  - 这个目录通常用于开发阶段，因为它包含了未裁剪的内容（如调试符号和头文件）。
  - 不适用于目标文件系统，因为它的体积较大，且包含不必要的开发文件 [[10]]。

---

### 5. **`target/`**
- **说明**：包含完整的根文件系统，经过裁剪和优化，适合部署到目标设备。
- **用途**：
  - 对比 `staging/`，这个目录不包含开发文件（如头文件），并且二进制文件经过 `strip` 处理以减小体积。
  - 这是最终的目标文件系统，可以直接打包成镜像文件（如 `rootfs.tar`）[[10]]。

---

### 总结
Buildroot 的输出目录结构清晰地划分了不同阶段的文件用途：
- **`build/`**：存放软件包的源码和编译中间文件。
- **`host/`**：存放主机端工具链和辅助工具。
- **`images/`**：存放最终生成的镜像文件，用于部署到目标设备。
- **`staging/`**：存放开发相关的文件，适合开发阶段使用。
- **`target/`**：存放裁剪后的根文件系统，适合目标设备使用 [[1]]。

通过理解这些目录的作用，可以更高效地管理和调试 Buildroot 构建过程。

