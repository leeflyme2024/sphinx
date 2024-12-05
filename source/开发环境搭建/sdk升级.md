# 问题
#  工具链的c库版本不匹配
```bash

root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04# ll /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc
-rwxr-xr-x 1 root root 1710848 Sep  3 11:35 /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc*


root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04# /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc --version
bash: /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc: No such file or directory
```
![[Pasted image 20240912193841.png]]

## 排查
```bash
root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04# ldd /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc
/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.38' not found (required by /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc)
/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.36' not found (required by /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc)
        linux-vdso.so.1 (0x00007ffdb1362000)
        libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f0cf7042000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f0cf6e19000)
        /home/liweiyu/am62x/src/ti/linux-devkit/sysroots/x86_64-arago-linux/lib/ld-linux-x86-64.so.2 => /lib64/ld-linux-x86-64.so.2 (0x00007f0cf7131000)

root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04# file /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc
/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), dynamically linked, interpreter /home/liweiyu/am62x/src/ti/linux-devkit/sysroots/x86_64-arago-linux/lib/ld-linux-x86-64.so.2, BuildID[sha1]=defbf7c0db4f5a73c2e73e8241e402a725501149, for GNU/Linux 3.2.0, stripped
```

![[Pasted image 20240912194020.png]]

c库版本
```bash
root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04# ldd --version
ldd (Ubuntu GLIBC 2.35-0ubuntu3.8) 2.35
Copyright (C) 2022 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
Written by Roland McGrath and Ulrich Drepper.
```
![[Pasted image 20240912194059.png]]

根据您提供的信息，问题的核心在于您的系统上的 GLIBC 版本与 aarch64-oe-linux-gcc 所需的版本不匹配。具体来说：
1.您的系统当前使用的是 GLIBC 2.35 版本。
2.aarch64-oe-linux-gcc 需要 GLIBC 2.36 和 GLIBC 2.38 版本。
![[Pasted image 20240912195054.png]]

根本原因是：

![[Pasted image 20240912195111.png]]


要自动执行解压 `ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin` 文件的操作，你可以使用以下方法：

1. 首先，确保该文件有执行权限：

   ```bash
   chmod +x ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin
   ```

2. 然后，你可以使用以下命令来自动执行解压操作：

   ```bash
   ./ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin --mode unattended --prefix /path/to/install/directory
   ```

   这里的参数解释如下：
   - `--mode unattended`: 这个选项告诉安装程序以无人值守模式运行，不需要用户交互。
   - `--prefix /path/to/install/directory`: 指定安装目录。将 `/path/to/install/directory` 替换为你想要安装 SDK 的实际路径。

3. 如果你想要更多的控制或自定义选项，你可能需要创建一个配置文件。例如，创建一个名为 `silent.cfg` 的文件，内容如下：

   ```
   # Silent configuration file
   ACCEPT_EULA=1
   INSTALL_DIR=/path/to/install/directory
   INSTALL_COMPONENTS=all
   ```

   然后使用这个配置文件运行安装程序：

   ```bash
   ./ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin --mode silent --silentconfig silent.cfg
   ```

4. 如果你只是想解压文件而不安装，你可以尝试以下命令：

   ```bash
   ./ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin --target /path/to/extract/directory
   ```

   这将把文件的内容解压到指定的目录，而不进行实际的安装。

请注意，具体的选项和参数可能会因 TI 的不同版本而略有变化。如果这些方法不起作用，你可能需要查看 TI 提供的具体文档，或者运行 `./ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin --help` 来查看可用的选项。


```bash
root@DESKTOP-GC4LAR7:/home/liweiyu/tmp2# ./ti-processor-sdk-linux-rt-am62xx-evm-09.02.01.10-Linux-x86-Install.bin --help
ti-processor-sdk-linux-rt-am62xx-evm-09.02.01.10 09.02.01.10
Usage:

 --help                                      Display the list of valid options

 --version                                   Display product information

 --unattendedmodeui <unattendedmodeui>       Unattended Mode UI
                                             Default: none
                                             Allowed: none minimal minimalWithDialogs

 --optionfile <optionfile>                   Installation option file
                                             Default:

 --debuglevel <debuglevel>                   Debug information level of verbosity
                                             Default: 2
                                             Allowed: 0 1 2 3 4

 --mode <mode>                               Installation mode
                                             Default: xwindow
                                             Allowed: gtk xwindow text unattended

 --debugtrace <debugtrace>                   Debug filename
                                             Default:

 --installer-language <installer-language>   Language selection
                                             Default: en
                                             Allowed: sq ar es_AR az eu pt_BR bg ca hr cs da nl en et fi fr de el he hu id it ja kk ko lv lt no fa pl pt ro ru sr zh_CN sk sl es sv th zh_TW tr tk va vi cy

 --prefix <prefix>                           Destination Folder
                                             Default: /opt/ti-processor-sdk-linux-rt-am62xx-evm-09.02.01.10

```

该 `ti-processor-sdk-linux-rt-am62xx-evm-09.02.01.10-Linux-x86-Install.bin 安装程序的帮助信息提供了对其使用的详细指导，包括多种命令行选项，这些选项允许用户以不同的方式定制安装过程。以下是对每个选项的详细解释：

 基本选项
- `--help`: 显示有效选项的列表，可以用来快速获取可用命令的概览。
---version`: 显示产品信息，包括版本号和其他可能的详细信息，有助于确认安装程序的具体版本### 安装模式
- `--mode <mode>`: 指定安装模式。允许的值包括：
  - `gtk`: 使用 GTK 图形界面进行安装（如果可用）。
  - `xwindow`: 使用基于X窗口系统的图形界面。
  - `text`: 使用纯文本模式，不需要图形界面。
  - `unattended`: 无人值守模式，不会有任何用户交互，通常用于自动化安装脚本。
  
### 安装配置
- `--prefix <prefix>`: 指定安装目录。默认为 `/opt/ti-processor-sdk-linux-rt-am62xx-evm-09.02.01.10`，你可以更改此路径以安装到不同的位置。

### 用户界面相关
- `--unattendedmodeui <unattendedmodeui>`: 在无人值守模式下设置用户界面的类型。选项包括：
  - `none`: 无任何界面。
  - `minimal`: 最小界面。
  - `minimalWithDialogs`: 提供最小的对话界面。
- `--installer-language <installer-language>`: 设置安装程序的语言。支持多种语言，如英语(en)、汉语简体(zh_CN)、汉语繁体(zh_TW)等。

### 调试和日志
- `--debuglevel <debuglevel>`: 设置调试信息的详细级别，从 0（最少）到 4（最详细）。
- `--debugtrace <debugtrace>`: 指定一个文件名，用于存储调试信息和跟踪日志，有助于在出现问题时进行故障排除。

### 选项文件
- `--optionfile <optionfile>`: 指定一个包含安装选项的文件，该文件可以预先定义上述各种选项，以简化命令行输入。

通过结合这些选项，用户可以更精准地控制安装流程，适应不同的部署环境和需求。例如，如果您正在自动化部署流程，可能会选择使用无人值守模式(`--mode unattended`)与最小或无界面(`--unattendedmodeui none`)，并指定一个非默认的安装路径(`--prefix`)以及必要的语言环境(`--installer-language`)。



生成一个自解压的可执行安装文件（如 `-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin`）通常需要几个步骤，并且涉及到使用打包和脚本工具。下面是一个简化的过程，展示如何从头开始创建这类文件：

[Linux .bin安装文件制作 - iTech - 博客园](https://www.cnblogs.com/itech/archive/2010/02/01/1661248.html)
[Linux下创建可执行bin安装文件 - Cocowool - 博客园](https://www.cnblogs.com/cocowool/p/7733089.html)
[linux下解压bin文件 - 白菜不是菜](https://iminto.github.io/post/linux%E4%B8%8B%E8%A7%A3%E5%8E%8Bbin%E6%96%87%E4%BB%B6/)

### 步骤 1: 准备文件和目录结构

首先，你需要准备所有要包含在安装包中的文件和目录。这可能包二进制文件、库文件、文档、配置文件等。将这些文件组到一个清晰的目录结构中，这将是你安装程序解压后的结构。

例如：
```
/my-sdk/
├── bin/
│   └── some-binary
├── lib/
│   └── some-library.so
├── share/
│   └── docs/
└── install-script.sh
```

### 步骤 2: 创建安装脚本

创建一个安装脚（例如 `install-script.sh`），这个脚本将会在用户执行自解压文件时运行。脚本中应包括复制文件、设置环境变量、检查依赖等步骤。示例脚本如下：

```bash
#!/bin/bashecho "Installing My SDK..."
cp -r /tmp/my-sdk/* /opt/my-sdk/
echo "Installation Complete."
```

```bash
# sdk-install.sh
#!/bin/sh

# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
# THE SOFTWARE.

# Process command line...
while [ $# -gt 0 ]; do
  case $1 in
    --install_dir)
      shift;
      install_dir=$1;
      ;;
     *)
      shift;
      ;;
  esac
done

if [ "x$install_dir" = "x" ]; then
    # Verify that the script is being called within untar SDK directory
    if [ ! -d "$PWD/board-support" -a ! -f "$PWD/Rules.make" ]; then
        echo "Script must be called within untarred sdk directory"
        exit 1
    else
        install_dir="$PWD"
    fi
fi

# Get full path to SDK directory
cd "$install_dir"
install_dir="$PWD"
cd -

if [ ! -d $install_dir ]; then
        echo "Installation directory does not exist"
        exit 1
fi

# Remove any .svn directories that were packaged by the sourceipks
rm -rf `find $install_dir -name ".svn"`


# Update Rules.make variables
sed -i -e s=__SDK__INSTALL_DIR__=$install_dir= $install_dir/Rules.make

# Find the linux directory name
linux=`find $install_dir/board-support -maxdepth 1 -name "linux*"`
linux=`basename $linux`
sed -i -e s=__KERNEL_NAME__=$linux= $install_dir/Rules.make

threads=`cat /proc/cpuinfo | grep -c processor`

# Create a newline
echo >> $install_dir/Rules.make
# Set optimal value for the make file's -j option
echo "MAKE_JOBS=$threads" >> $install_dir/Rules.make

cd -


# Install toolchain sdk
$install_dir/linux-devkit.sh -y -d $install_dir/linux-devkit
if [ -f $install_dir/k3r5-devkit.sh ]; then
    $install_dir/k3r5-devkit.sh -y -d $install_dir/k3r5-devkit
fi

# Remove toolchain sdk
rm $install_dir/linux-devkit.sh
if [ -f $install_dir/k3r5-devkit.sh ]; then
    rm $install_dir/k3r5-devkit.sh
fi


# Update example applications CCS project files

# Grab some essential variables from environment-setup
REAL_MULTIMACH_TARGET_SYS=`sed -n 's/^export REAL_MULTIMACH_TARGET_SYS=//p' $install_dir/linux-devkit/environment-setup`
TOOLCHAIN_SYS=`sed -n 's/^export TOOLCHAIN_SYS=//p' $install_dir/linux-devkit/environment-setup`
SDK_SYS=`sed -n 's/^export SDK_SYS=//p' $install_dir/linux-devkit/environment-setup`

# Grab toolchain's GCC version
GCC_VERSION=`ls $install_dir/linux-devkit/sysroots/$SDK_SYS/usr/lib/gcc/$TOOLCHAIN_SYS/`

TOOLCHAIN_TARGET_INCLUDE_DIR="linux-devkit/sysroots/$REAL_MULTIMACH_TARGET_SYS/usr/include"
TOOLCHAIN_INCLUDE_DIR="linux-devkit/sysroots/$SDK_SYS/usr/lib/gcc/$TOOLCHAIN_SYS/$GCC_VERSION/include"

SDK_PATH_TARGET="linux-devkit/sysroots/$REAL_MULTIMACH_TARGET_SYS/"

sed -i -e s=__SDK_PATH_TARGET__=$SDK_PATH_TARGET= $install_dir/Rules.make

if [ -f "$install_dir/bin/unshallow-repositories.sh" ]
then
    sed -i -e s=__SDK_INSTALL_DIR__=$install_dir= $install_dir/bin/unshallow-repositories.sh
fi

if [ -f "$install_dir/bin/create-ubifs.sh" ]
then
    sed -i -e s=__SDK_INSTALL_DIR__=$install_dir= $install_dir/bin/create-ubifs.sh
fi

# Modify create-sdcard.sh to have user-supplied installation directory
if [ -f "${install_dir}/bin/create-sdcard.sh" ]
then
    sed -i -e "s|ti-sdk.*\[0-9\]|${install_dir##*/}|g" ${install_dir}/bin/create-sdcard.sh
fi

# Update CCS project files using important paths to headers

find $install_dir/example-applications -type f -exec sed -i "s|<TOOLCHAIN_TARGET_INCLUDE_DIR>|$TOOLCHAIN_TARGET_INCLUDE_DIR|g" {} \;
find $install_dir/example-applications -type f -exec sed -i "s|<TOOLCHAIN_INCLUDE_DIR>|$TOOLCHAIN_INCLUDE_DIR|g" {} \;


if [ -f "$install_dir/sdk-install.sh" ]; then
        # File is only able to run once so delete it from the SDK
        rm "$install_dir/sdk-install.sh"
fi

echo "Installation completed!"
exit 0

```

![[Pasted image 20240913104515.png]]

### 步骤 3: 创建自解压脚本

使用 `makeself` 或 `shar` 等工具来创建自解压脚。这些工可以将目录打包成一个自解压的 shell 脚本。`makeself` 是一个流行的选择，它允许你创建一个包含完整目录的压缩自解压档案。

[用Makeself工具打包镜像 | Escape](https://www.escapelife.site/posts/766c5534.html)


首先，安装 `makeself`：

```bash
sudo apt-get install makeself  # Debian/Ubuntu
```

然后，使用 `makeself` 打包目录：

```bash
makeself /my-sdk my-sdk-inst.bin "My SDK Installation" ./install-script.sh
```

这条命令将会把 `/my-sdk` 目录下的所有文件打包成一个名为 `my-sdk-installer.bin` 的自解压安装文件，并在用户执行该文件时自动运行 `install-script.sh` 安装脚本。

### 步骤 4: 测试安装文件

在不同的环境测试你的安装文件，确保它在所有目标平台上都能正确执行。

### 步骤 5: 分发

一旦测试无误，你可以通过网站、FTP 或其他方法分发这个装文件。

这只是创建自解压安装文件的基本步骤。根据你的具体需求，可能还需要添加更多的错误检测、用户交互和配置功能。


# uboot
## dts
```dts
k3-am625-r5-sk.dts
	#include "k3-am625-sk.dts"
		#include "k3-am62x-sk-common.dtsi"
			#include "k3-am625.dtsi"
				#include "k3-am62.dtsi"
					#include "k3-am62-main.dtsi"
					#include "k3-am62-mcu.dtsi"
					#include "k3-am62-wakeup.dtsi"
					#include "k3-am62-thermal.dtsi"

	#include "k3-am62x-sk-ddr4-1600MTs.dtsi"

	#include "k3-am62-ddr.dtsi"
		#include "k3-am64-ddr.dtsi"

	#include "k3-am625-sk-u-boot.dtsi"
		#include "k3-am625-sk-binman.dtsi"
			#include "k3-binman.dtsi"
```

### 自动在k3-am625-sk.dts加k3-am625-sk-uboot.dtsi
![[Pasted image 20241101191522.png]]
![[Pasted image 20241101191630.png]]
![[Pasted image 20241101192244.png]]
![[Pasted image 20241101191719.png]]
![[Pasted image 20241101191735.png]]

## 安全启动
```bash
doc/usage/fit/howto.rst
boot/image-fit.c

./arch/arm/mach-k3/keys/custMpk.pem
./arch/arm/mach-k3/keys/ti-degenerate-key.pem


root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/board-support/ti-u-boot-2024.04+git# ll ./board/ti/am62x/
total 80
drwxr-xr-x  2 root  root  4096 Sep 25 07:32 ./
drwxr-xr-x 20 root  root  4096 Sep 18 09:34 ../
-rw-r--r--  1 60898 1373   606 Aug  5 11:40 Kconfig
-rw-r--r--  1 60898 1373   213 Aug  5 11:40 MAINTAINERS
-rw-r--r--  1 60898 1373   170 Aug  5 11:40 Makefile
-rwxrwxrwx  1 root  root  2768 Sep 19 06:36 am62x.env*
-rw-r--r--  1 60898 1373   798 Aug  5 11:40 board-cfg.yaml
-rwxrwxrwx  1 root  root  4414 Sep 18 09:33 evm.c*
-rw-r--r--  1 60898 1373   244 Aug  5 11:40 pm-cfg.yaml
-rw-r--r--  1 60898 1373 25889 Aug  5 11:40 rm-cfg.yaml
-rw-r--r--  1 60898 1373 11009 Aug  5 11:40 sec-cfg.yaml

ll tools/binman/
	etype/x509_cert.py
	etype/ti_secure.py
	etype/ti_secure_rom.py

	btool/openssl.py

	entries.rst
	ftest.py
```

## lvds
### 时钟确认
![[Pasted image 20241129162518.png]]

![[Pasted image 20241129162600.png]]

![[Pasted image 20241129200228.png]]

![[Pasted image 20241129162838.png]]

![[Pasted image 20241129163014.png]]


![[Pasted image 20241129161453.png]]
![[Pasted image 20241129161436.png]]

![[Pasted image 20241129195623.png]]

![[Pasted image 20241129194748.png]]

![[Pasted image 20241129195856.png]]

[manual.zlg.cn/Public/Uploads/2024-02-18/65d1b3b2b8c3f.pdf](https://manual.zlg.cn/Public/Uploads/2024-02-18/65d1b3b2b8c3f.pdf)

![[Pasted image 20241129162044.png]]
![[Pasted image 20241129162107.png]]
![[Pasted image 20241129161659.png]]
![[Pasted image 20241129161543.png]]

![[Pasted image 20241129161610.png]]


### oldi
```bash
enum dss_oldi_modes {
	OLDI_MODE_OFF,				/* OLDI turned off / tied off in IP. */
	OLDI_SINGLE_LINK_SINGLE_MODE,		/* Single Output over OLDI 0. */
	OLDI_SINGLE_LINK_DUPLICATE_MODE,	/* Duplicate Output over OLDI 0 and 1. */
	OLDI_DUAL_LINK,				/* Combined Output over OLDI 0 and 1. */
};


struct tidss_drv_priv {

	// priv->dev = dev;
	struct udevice *dev;


	// priv->base_common = dev_remap_addr_name(dev, priv->feat->common);
	void __iomem *base_common; /* common register region of dss*/


	// priv->base_vid[i] = dev_remap_addr_name(dev, priv->feat->vid_name[i]);
	void __iomem *base_vid[TIDSS_MAX_PLANES]; /* plane register region of dss*/



	// priv->base_ovr[i] = dev_remap_addr_name(dev, priv->feat->ovr_name[i]);
	void __iomem *base_ovr[TIDSS_MAX_PORTS]; /* overlay register region of dss*/


	// priv->base_vp[i] = dev_remap_addr_name(dev, priv->feat->vp_name[i]);
	void __iomem *base_vp[TIDSS_MAX_PORTS]; /* video port register region of dss*/



	struct regmap *oldi_io_ctrl;

	// ret = clk_get_by_name(dev, "vp1", &priv->vp_clk[0]);
	struct clk vp_clk[TIDSS_MAX_PORTS];


	// priv->feat = &dss_am625_feats;
	const struct dss_features *feat;


	// ret = clk_get_by_name(dev, "fck", &priv->fclk);
	struct clk fclk;


	struct dss_vp_data vp_data[TIDSS_MAX_PORTS];


	// priv->oldi_mode = OLDI_DUAL_LINK;
	enum dss_oldi_modes oldi_mode;

	// priv->bus_format = &dss_bus_formats[7];
	struct dss_bus_format *bus_format;


	// priv->pixel_format = DSS_FORMAT_XRGB8888;
	u32 pixel_format;



	u32 memory_bandwidth_limit;


};


struct video_uc_plat {
	uint align;
	uint size;
	ulong base;
	ulong copy_base;
	ulong copy_size;
	bool hide_logo;
};


struct video_priv {

	// uc_priv->xsize = timings.hactive.typ;
	ushort xsize;

	// uc_priv->ysize = timings.vactive.typ;
	ushort ysize;



	ushort rot;

	// uc_priv->bpix = VIDEO_BPP32;
	enum video_log2_bpp bpix;


	enum video_format format;
	const char *vidconsole_drv_name;
	int font_size;

	/*
	 * Things that are private to the uclass: don't use these in the
	 * driver
	 */
	void *fb;
	int fb_size;
	void *copy_fb;
	int line_length;
	u32 colour_fg;
	u32 colour_bg;
	bool flush_dcache;
	u8 fg_col_idx;
	u8 bg_col_idx;
};


struct video_ops {
	int (*video_sync)(struct udevice *vid);
};


dss_common_regmap = priv->feat->common_regs;


struct display_timing timings;
	ret = panel_get_display_timing(panel, &timings);
	if (ret) {
		ret = ofnode_decode_panel_timing(dev_ofnode(panel),
						 &timings);
		if (ret) {
			dev_err(dev, "decode display timing error %d\n", ret);
			return ret;
		}
	}

	dss_vid_write(priv, 0, DSS_VID_BA_0, uc_plat->base & 0xffffffff);
	dss_vid_write(priv, 0, DSS_VID_BA_EXT_0, (u64)uc_plat->base >> 32);
	dss_vid_write(priv, 0, DSS_VID_BA_1, uc_plat->base & 0xffffffff);
	dss_vid_write(priv, 0, DSS_VID_BA_EXT_1, (u64)uc_plat->base >> 32);

dss_init_am65x_oldi_io_ctrl

video_set_flush_dcache



/* common */
dss_write
dss_read

FLD_MASK
FLD_VAL
FLD_GET
FLD_MOD



/* read and modify common register region of DSS*/
REG_GET
REG_FLD_MOD




/* read and modify planes vid1 and vid2 register of DSS*/
VID_REG_GET
VID_REG_FLD_MOD

dss_vid_write
dss_vid_read

dss_plane_setup
dss_plane_enable
dss_plane_init



/* read and modify overlay ovr1 and ovr2 registers of DSS*/
OVR_REG_GET
OVR_REG_FLD_MOD

dss_ovr_write
dss_ovr_read

dss_ovr_set_plane
dss_ovr_enable_layer



/* read and modify port vp1 and vp2 registers of DSS*/
VP_REG_GET
VP_REG_FLD_MOD

dss_vp_write
dss_vp_read

dss_vp_enable_clk
dss_vp_set_clk_rate
dss_vp_prepare
dss_vp_enable
dss_vp_init



------------------------------------------------------------
tidss_drv_probe
	dss_vp_enable_clk

	dss_vp_set_clk_rate

	dss_init_am65x_oldi_io_ctrl

	dss_vp_prepare
		dss_oldi_tx_power
		dss_enable_oldi

	dss_vp_enable

	dss_vp_init

```
![[Pasted image 20241129195054.png]]

[PROCESSOR-SDK-AM62X: How to decide LCD parameters in panel-simple.c - Processors forum - Processors - TI E2E support forums](https://e2e.ti.com/support/processors-group/processors/f/processors-forum/1387625/processor-sdk-am62x-how-to-decide-lcd-parameters-in-panel-simple-c?tisearch=e2e-sitesearch&keymatch=PROCESSOR-SDK-J722S)
![[Pasted image 20241129162240.png]]

#### 电源配置
![[Pasted image 20241129163915.png]]
![[Pasted image 20241129163930.png]]
![[Pasted image 20241129164004.png]]

![[Pasted image 20241129164210.png]]

![[Pasted image 20241129164021.png]]

#### 使能
![[Pasted image 20241129170921.png]]
![[Pasted image 20241129170940.png]]
![[Pasted image 20241129170955.png]]

![[Pasted image 20241129172016.png]]

![[Pasted image 20241129194535.png]]

![[Pasted image 20241129172031.png]]

# 内核
## lvds
![[Pasted image 20241031161024.png]]
![[img_v3_02g6_be16b9aa-bf40-4b30-bfd8-4d1cc6c5915g.jpg]]

原因是tidss先于panel先初始化，这个错误无影响
![[Pasted image 20241031161120.png]]