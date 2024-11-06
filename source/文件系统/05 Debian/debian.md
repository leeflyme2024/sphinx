# 指导
[1. Overview — Debian AM62x Documentation](https://software-dl.ti.com/processor-sdk-linux-rt/esd/AM62X/10_00_07_Debian/exports/docs/debian/Overview.html)

# 版本
- [下一代 Debian 正式发行版的代号为 trixie](https://www.debian.org/releases/trixie/) — 测试（testing）版 — 发布日期尚未确定
- [Debian 12 (bookworm)](https://www.debian.org/releases/bookworm/) — 当前的稳定（stable）版
- [Debian 11 (bullseye)](https://www.debian.org/releases/bullseye/) — 当前的旧的稳定（oldstable）版
- [Debian 10（buster）](https://www.debian.org/releases/buster/) — 已存档版本，另有第三方付费[扩展长期支持](https://wiki.debian.org/LTS/Extended)
- [Debian 9（stretch）](https://www.debian.org/releases/stretch/) — 已存档版本，另有第三方付费[扩展长期支持](https://wiki.debian.org/LTS/Extended)
- [Debian 8（jessie）](https://www.debian.org/releases/jessie/) — 已存档版本，另有第三方付费[扩展长期支持](https://wiki.debian.org/LTS/Extended)
- [Debian 7（wheezy）](https://www.debian.org/releases/wheezy/) — 被淘汰的稳定版
- [Debian 6.0（squeeze）](https://www.debian.org/releases/squeeze/) — 被淘汰的稳定版
- [Debian GNU/Linux 5.0（lenny）](https://www.debian.org/releases/lenny/) — 被淘汰的稳定版
- [Debian GNU/Linux 4.0（etch）](https://www.debian.org/releases/etch/) — 被淘汰的稳定版
- [Debian GNU/Linux 3.1（sarge）](https://www.debian.org/releases/sarge/) — 被淘汰的稳定版
- [Debian GNU/Linux 3.0（woody）](https://www.debian.org/releases/woody/) — 被淘汰的稳定版
- [Debian GNU/Linux 2.2（potato）](https://www.debian.org/releases/potato/) — 被淘汰的稳定版
- [Debian GNU/Linux 2.1（slink）](https://www.debian.org/releases/slink/) — 被淘汰的稳定版
- [Debian GNU/Linux 2.0（hamm）](https://www.debian.org/releases/hamm/) — 被淘汰的稳定版

# 源
## TI’s official PPA repository
[ti-debpkgs/dists at main · TexasInstruments/ti-debpkgs · GitHub](https://github.com/TexasInstruments/ti-debpkgs/tree/main/dists)

# 编译脚本
[GitHub - TexasInstruments/ti-bdebstrap: Build custom bootstrap images using bdebstrap](https://github.com/TexasInstruments/ti-bdebstrap)
```bash
git clone https://github.com/TexasInstruments/ti-bdebstrap.git
git tag
git checkout <tag-name>
git branch
```

```bash
root@DESKTOP-GC4LAR7:ti-bdebstrap# git tag
09.00.00.06-release
09.01.00.008-release
09.02.01.009-release
09.02.01.010-release
10.00.07-release

root@DESKTOP-GC4LAR7:ti-bdebstrap# git checkout 10.00.07-release
09.00.00.06-release
09.01.00.008-release
09.02.01.009-release
09.02.01.010-release
10.00.07-release

root@DESKTOP-GC4LAR7:ti-bdebstrap# git branch
* (HEAD detached at 10.00.07-release)
  master
```

在 chroot 环境中生成 RootFS（根文件系统）的开源工具。具体来说：
1. **debootstrap**：这是一个用于从 Debian 仓库生成根文件系统的旧工具，现在已经被废弃（deprecated）。
2. **mmdebstrap**：这是一个用于生成根文件系统的复杂工具，它提供了更多的定制选项和功能。
3. **bdebstrap**：这是一个简单的工具，它作为 mmdebstrap 的简化包装（wrapper），使得使用起来更加方便。

## 代码目录
```bash
ti-bdebstrap
├── build.sh
├── builds.toml
├── configs
│   ├── bdebstrap_configs
│   │   ├── bookworm
│   │   │   ├── bookworm-<machine>.yaml
│   │   │   └── bookworm-rt-<machine>.yaml
│   │   └── trixie
│   │       ├── trixie-<machine>.yaml
│   │       └── trixie-rt-<machine>.yaml
│   ├── bsp_sources.toml
│   └── machines --> Machine configurations for each BSP version
│       ├── 09.02.00.010.toml
│       └── 10.00.05.toml
├── create-sdcard.sh
├── create-wic.sh
├── LICENSE
├── README.md
├── scripts
│   ├── build_bsp.sh
│   ├── build_distro.sh
│   ├── common.sh
│   └── setup.sh
└── target --> Custom files to deploy in target.
```
```bash
ti-bdebstrap/
├── LICENSE
├── README.md
├── build.sh
├── builds.toml
├── configs
│   ├── bdebstrap_configs
│   │   ├── bookworm
│   │   │   ├── bookworm-am62pxx-evm.yaml
│   │   │   ├── bookworm-am62xx-evm.yaml
│   │   │   ├── bookworm-am62xx-lp-evm.yaml
│   │   │   ├── bookworm-am62xxsip-evm.yaml
│   │   │   ├── bookworm-am64xx-evm.yaml
│   │   │   ├── bookworm-rt-am62pxx-evm.yaml
│   │   │   ├── bookworm-rt-am62xx-evm.yaml
│   │   │   ├── bookworm-rt-am62xx-lp-evm.yaml
│   │   │   ├── bookworm-rt-am62xxsip-evm.yaml
│   │   │   └── bookworm-rt-am64xx-evm.yaml
│   │   └── trixie
│   │       ├── trixie-am62pxx-evm.yaml
│   │       ├── trixie-am62xx-evm.yaml
│   │       ├── trixie-am62xx-lp-evm.yaml
│   │       ├── trixie-am62xxsip-evm.yaml
│   │       ├── trixie-am64xx-evm.yaml
│   │       ├── trixie-rt-am62pxx-evm.yaml
│   │       ├── trixie-rt-am62xx-evm.yaml
│   │       ├── trixie-rt-am62xx-lp-evm.yaml
│   │       ├── trixie-rt-am62xxsip-evm.yaml
│   │       └── trixie-rt-am64xx-evm.yaml
│   ├── bsp_sources.toml
│   └── machines
│       ├── 09.02.00.010.toml
│       └── 10.00.07.toml
├── create-sdcard.sh
├── create-wic.sh
├── scripts
│   ├── build_bsp.sh
│   ├── build_distro.sh
│   ├── common.sh
│   └── setup.sh
└── target
    ├── kernel
    │   └── postinst.d
    │       └── cp-kernel-and-overlays
    ├── resize_rootfs
    │   ├── resize_rootfs.service
    │   └── resize_rootfs.sh
    └── weston
        ├── weston
        ├── weston.service
        └── weston.socket
```

### 功能用途
这段内容是关于一个名为 `ti-bdebstrap` 的仓库的结构说明。下面是对指定部分的解释：

1. **build.sh**: 这是用户应该运行的“主”脚本，用于生成Debian镜像。这意味着用户通过运行这个脚本可以自动化地创建Debian操作系统的镜像文件。

2. **configs/**: 这个目录包含了机器、BSP（板级支持包）源代码和发行版变体的详细信息、配置选项和值。这些配置信息用于定制和构建不同的系统镜像。

3. **scripts/**: 这个目录包含了辅助脚本，这些脚本是为 `build.sh` 服务的，帮助完成构建过程的不同步骤。

4. **target/**: 这个目录包含了目标配置文件，比如 `weston.service`，这是为weston目标（可能是一个特定的硬件平台或用途）准备的配置文件。

5. **builds.toml**: 这个文件包含了所有有效构建的列表及其定义。TOML（Tom's Obvious, Minimal Language）是一种配置文件格式，这里用来定义构建过程中的各种参数和选项。

## 编译命令

```bash
# The `<build-name>` must be one present inside `builds.toml` file.
sudo ./build.sh <build-name>
```

```bash
./build.sh trixie-rt-am62xx-evm
./create-wic.sh trixie-rt-am62xx-evm
./create-sdcard.sh trixie-rt-am62xx-evm
```

在构建完成后，RootFS（根文件系统）、Boot分区和bsp_sources（板级支持包源代码）将被存储在`build/<build-name>`目录下。其中，`<build-name>`是构建过程中指定的名称。
构建日志将被存储在`logs/<build-name>.log`文件中。

#### chroot
```bash
Usage: /usr/sbin/chroot [OPTION] NEWROOT [COMMAND [ARG]...]
  or:  /usr/sbin/chroot OPTION
Run COMMAND with root directory set to NEWROOT.

  --groups=G_LIST        specify supplementary groups as g1,g2,..,gN
  --userspec=USER:GROUP  specify user and group (ID or name) to use
  --skip-chdir           do not change working directory to '/'
      --help     display this help and exit
      --version  output version information and exit

If no command is given, run '"$SHELL" -i' (default: '/bin/sh -i').

GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
Report any translation bugs to <https://translationproject.org/team/>
Full documentation <https://www.gnu.org/software/coreutils/chroot>
or available locally via: info '(coreutils) chroot invocation'
```


#### bdebstrap命令

[bdebstrap(1) — bdebstrap — Debian bullseye — Debian Manpages](https://manpages.debian.org/bullseye/bdebstrap/bdebstrap.1.en.html)
[Site Unreachable](https://manpages.debian.org/bullseye/mmdebstrap/mmdebstrap.1.en.html)
[Packages for Linux and Unix](https://ubuntu.pkgs.org/22.04/ubuntu-universe-amd64/bdebstrap_0.4.0-1_all.deb.html)
[GitHub - bdrung/bdebstrap at v0.4.0](https://github.com/bdrung/bdebstrap/tree/v0.4.0)
```bash
/usr/lib/python3/dist-packages/bdebstrap-0.4.0.egg-info/PKG-INFO
```
[Git : Code : bdebstrap package : Ubuntu](https://code.launchpad.net/ubuntu/+source/bdebstrap)
[josch/mmdebstrap - Muffin Gitea](https://gitlab.mister-muffin.de/josch/mmdebstrap#)



```bash
bdebstrap \
    -c ${topdir}/configs/bdebstrap_configs/${distro_codename}/${distro}.yaml \
    --name ${topdir}/build/${distro} \
    --target tisdk-debian-${distro}-rootfs \
    --hostname ${machine} \
    -f \
    &>>"${LOG_FILE}"
```

##### -c
```bash
-c CONFIG, --config CONFIG
Configuration YAML file. See YAML CONFIGURATION below for the expected structure of this file. 
This parameter can be specified multiple times. 
The content of YAML files will be merged by appending lists and recursively merging maps.
```

这段内容是命令行工具的参数说明，其中“-c CONFIG, --config CONFIG”是参数的使用方法。具体解释如下：

- `-c CONFIG` 和 `--config CONFIG` 是两个等价的参数，用于指定配置文件。用户可以通过这两个参数中的任意一个来指定一个YAML格式的配置文件。
- 配置文件用于定义该工具的运行配置，比如需要安装的软件包、环境变量等。这个配置文件的结构在文档的“YAML CONFIGURATION”部分有详细说明。
- 这个参数可以被指定多次，意味着用户可以同时指定多个配置文件。当指定多个配置文件时，这些文件的内容会被合并。合并规则是：列表类型的配置项会被追加合并，而映射（键值对）类型的配置项会被递归合并。
- 例如，如果有两个配置文件，一个指定了安装包A和B，另一个指定了安装包C，那么最终的配置会是安装包A、B和C。

简而言之，这个参数允许用户通过指定一个或多个YAML格式的配置文件来自定义工具的行为。

##### --name
```bash
-n NAME, --name NAME
name of the generated golden image. 
If no output directory is specified, the golden image will be placed in OUTPUT_BASE_DIR/NAME.
```
这段内容是命令行工具的参数说明，其中包含对 `-h, --help` 和 `-c CONFIG, --config CONFIG` 等参数的解释。这些参数用于配置和运行该工具。现在，我将解释你指定的部分：

`-n NAME, --name NAME` 这个参数用于指定生成的金质（golden）镜像的名称。如果未指定输出目录，则金质镜像会被放置在 `OUTPUT_BASE_DIR/NAME` 目录下。简单来说，这个参数决定了生成的镜像文件的名称，以及在没有明确指定输出目录时，镜像文件的存储位置。


##### --target
```bash
--target TARGET, TARGET
The optional target argument can either be the path to a directory, 
the path to a tarball filename, the path to a squashfs image or -.
```
这段内容是命令行工具的参数说明，描述了 `--target` 参数的用法。下面是对这个部分的解释：

- 参数名：`--target`
- 参数别名：没有别名，只有一个参数名 `TARGET`。
- 参数说明：这是一个可选参数。
- 功能：`--target` 参数用于指定目标位置，可以是以下几种类型：
  1. 目录路径：指定一个文件夹路径，作为操作的目标目录。
  2. tarball 文件名：指定一个 tarball 文件的路径，这个文件将被用来进行操作。
  3. squashfs 文件路径：指定一个 squashfs 镜像文件的路径，这个文件将被用来进行操作。
  4. `-`：表示使用标准输入或输出。

简单来说，`--target` 参数允许用户指定一个目标路径，这个路径可以是目录、tarball 文件、squashfs 镜像，或者使用标准输入输出。

##### -f
```bash
-f, --force
Remove existing output directory before creating a new one
```

这段内容描述了一个命令行工具的参数选项。其中 `-f` 和 `--force` 是该工具的两个参数，它们具有相同的功能。当你在命令行中使用这个工具并加上 `-f` 或 `--force` 参数时，工具会在创建新的输出目录之前删除任何已存在的同名输出目录。这确保了输出目录是干净的，不会包含之前运行时留下的任何文件或数据。

##### -v
```bash
-v, --verbose
Write informational messages 
(about configuration files, environment variables, mmdebstrap call, etc.) 
to standard error. 

Instead of progress bars, 
mmdebstrap writes the dpkg and apt output directly to standard error. 
If used together with --quiet or --debug, only the last option will take effect.
```

这段内容描述了一个命令行选项 `-v` 或 `--verbose` 的功能。当使用这个选项时，程序会向标准错误输出（通常是终端或命令行界面）提供信息性消息。这些信息可能包括关于配置文件、环境变量、mmdebstrap调用等的详细信息。

此外，使用 `-v` 或 `--verbose` 选项时，程序不会显示进度条，而是直接将 dpkg 和 apt 的输出直接写入标准错误。这意味着用户可以看到程序执行过程中的详细输出，而不是仅仅一个进度条。

如果 `-v` 或 `--verbose` 与 `--quiet` 或 `--debug` 同时使用，只有最后指定的选项会生效。这意味着如果用户先使用了 `-v`，然后又使用了 `--quiet`，那么程序将不会输出任何信息，因为 `--quiet` 会覆盖之前的 `-v` 选项。同样，如果用户先使用了 `-v`，然后又使用了 `--debug`，那么程序将输出详细的调试信息，因为 `--debug` 会覆盖 `-v` 选项。

##### --debug
```bash
--debug
In addition to the output produced by --verbose, write detailed debugging information to standard error. 
Errors of mmdebstrap will print a backtrace. 
If used together with --quiet or --verbose, only the last option will take effect.
```

这段内容是命令行工具的参数说明，具体解释了`--debug`参数的作用：

- `--debug`：这个选项用于增加额外的详细调试信息输出到标准错误（stderr）。当使用`--debug`时，除了`--verbose`选项产生的输出外，还会输出更详细的调试信息。如果`mmdebstrap`（一个用于创建Debian系统镜像的工具）出现错误，它还会打印出错误回溯信息。如果`--debug`与`--quiet`或`--verbose`一起使用，只有最后指定的那个选项会生效。


```bash
usage: bdebstrap [-h] [-c CONFIG] [-n NAME] [-e ENV] [-s] [-b OUTPUT_BASE_DIR] [-o OUTPUT] [-q] [-v] [--debug] [-f] [-t TMPDIR] [--variant {extract,custom,essential,apt,required,minbase,buildd,important,debootstrap,-,standard}]
                 [--mode {auto,sudo,root,unshare,fakeroot,fakechroot,proot,chrootless}] [--aptopt APTOPT] [--keyring KEYRING] [--dpkgopt DPKGOPT] [--hostname HOSTNAME] [--install-recommends] [--packages PACKAGES]
                 [--components COMPONENTS] [--architectures ARCHITECTURES] [--setup-hook COMMAND] [--essential-hook COMMAND] [--customize-hook COMMAND] [--cleanup-hook COMMAND] [--suite SUITE] [--target TARGET]
                 [--mirrors MIRRORS]
                 [suite] [target] [mirrors ...]

Call mmdebstrap with parameters specified in a YAML file.

positional arguments:
  suite                 The suite may be a valid release code name (eg, sid, stretch, jessie) or a symbolic name (eg, unstable, testing, stable, oldstable).
  target                The optional target argument can either be the path to a directory, the path to a tarball filename, the path to a squashfs image or '-'.
  mirrors               APT mirror to use. If no mirror option is provided, http://deb.debian.org/debian is used.

options:
  -h, --help            show this help message and exit
  -c CONFIG, --config CONFIG
                        bdebstrap configuration YAML.
  -n NAME, --name NAME  name of the generated golden image
  -e ENV, --env ENV     add additional environment variable.
  -s, --simulate, --dry-run
                        Run apt-get with --simulate. Only the package cache is initialized but no binary packages are downloaded or installed. Use this option to quickly check whether a package selection within a certain suite
                        and variant can in principle be installed as far as their dependencies go. If the output is a tarball, then no output is produced. If the output is a directory, then the directory will be left populated
                        with the skeleton files and directories necessary for apt to run in it.
  -b OUTPUT_BASE_DIR, --output-base-dir OUTPUT_BASE_DIR
                        output base directory
  -o OUTPUT, --output OUTPUT
                        output directory (default: output-base-dir/name)
  -q, --quiet, --silent
                        Do not write anything to standard error except errors.
  -v, --verbose         Write informational messages to standard error. Instead of progress bars, mmdebstrap writes the dpkg and apt output directly to standard error.
  --debug               In addition to the output produced by --verbose, write detailed debugging information to standard error.
  -f, --force           Remove existing output directory before creating a new one
  -t TMPDIR, --tmpdir TMPDIR
                        Temporary directory for building the image (default: /tmp)
  --variant {extract,custom,essential,apt,required,minbase,buildd,important,debootstrap,-,standard}
                        Choose which package set to install.
  --mode {auto,sudo,root,unshare,fakeroot,fakechroot,proot,chrootless}
                        Choose how to perform the chroot operation and create a filesystem with ownership information different from the current user.
  --aptopt APTOPT       Pass arbitrary options or configuration files to apt.
  --keyring KEYRING     Change the default keyring to use by apt.
  --dpkgopt DPKGOPT     Pass arbitrary options or configuration files to dpkg.
  --hostname HOSTNAME   Write the given HOSTNAME into /etc/hostname in the target chroot.
  --install-recommends  Consider recommended packages as a dependency for installing.
  --packages PACKAGES, --include PACKAGES
                        Comma or whitespace separated list of packages which will be installed in addition to the packages installed by the specified variant.
  --components COMPONENTS
                        Comma or whitespace separated list of components like main, contrib and non-free which will be used for all URI-only MIRROR arguments.
  --architectures ARCHITECTURES
                        Comma or whitespace separated list of architectures. The first architecture is the native architecture inside the chroot.
  --setup-hook COMMAND  Execute arbitrary COMMAND right after initial setup (directory creation, configuration of apt and dpkg, ...) but before any packages are downloaded or installed. At that point, the chroot directory does
                        not contain any executables and thus cannot be chroot-ed into.
  --essential-hook COMMAND
                        Execute arbitrary COMMAND after the Essential:yes packages have been installed, but before installing the remaining packages.
  --customize-hook COMMAND
                        Execute arbitrary COMMAND after the chroot is set up and all packages got installed but before final cleanup actions are carried out.
  --cleanup-hook COMMAND
                        Execute arbitrary COMMAND after all customize hooks have been executed.
  --suite SUITE         The suite may be a valid release code name (eg, sid, stretch, jessie) or a symbolic name (eg, unstable, testing, stable, oldstable).
  --target TARGET       The optional target argument can either be the path to a directory, the path to a tarball filename, the path to a squashfs image or '-'.
  --mirrors MIRRORS     Comma separated list of mirrors. If no mirror option is provided, http://deb.debian.org/debian is used.

```

```bash
bdebstrap 
	-c /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/filesystem/am62xx-evm/zy/tisdk-debian-trixie-rt-am62xx-evm/src/ti-bdebstrap/configs/bdebstrap_configs/trixie/trixie-rt-am62xx-evm.yaml 
	--name /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/filesystem/am62xx-evm/zy/tisdk-debian-trixie-rt-am62xx-evm/src/ti-bdebstrap/build/trixie-rt-am62xx-evm 
	--target tisdk-debian-trixie-rt-am62xx-evm-rootfs 
	--hostname am62xx-evm 
	-f
```

`bdebstrap` 命令是一个用于创建 Debian 或其衍生系统（如 Ubuntu）的工具，它允许用户定制化地创建一个 Debian 系统的根文件系统。通过使用不同的参数，用户可以控制输出目录、安装的套件、目标文件系统的位置等。下面是对给定命令的分析以及 `bdebstrap` 的工作原理的解释：

### 命令分析

- `-c ${topdir}/configs/bdebstrap_configs/${distro_codename}/${distro}.yaml`: 指定了一个 YAML 格式的配置文件路径。这个配置文件包含了构建过程中的一些特定选项，比如要使用的镜像、安装的包列表等。

- `--name ${topdir}/build/${distro}`: 设置生成的镜像文件的名字为 `${topdir}/build/${distro}`。这将作为输出目录的基础名称。

- `--target tisdk-debian-${distro}-rootfs`: 指定目标输出的位置或格式。这里的目标似乎是一个名为 `tisdk-debian-${distro}-rootfs` 的文件或目录。

- `--hostname ${machine}`: 在新创建的系统中设置主机名为 `${machine}`。

- `-f`: 强制覆盖已存在的输出目录。如果输出路径已经存在，将会删除旧的再重新创建新的。

- `&>>"${LOG_FILE}"`: 将命令执行期间的所有标准输出和错误输出追加到 `${LOG_FILE}` 文件中，以便于后续的日志查看和调试。

### 调试方法
根据您提供的 bdebstrap 命令用法和 Debian 镜像信息，我可以为您列出几种可能的调试手段：

1. 使用 -v 或 --verbose 选项：
   这会增加输出的详细程度，将 dpkg 和 apt 的输出直接写入标准错误流。
```
bdebstrap -v ...
```

2. 使用 --debug 选项：
   这会提供比 --verbose 更详细的调试信息。
```
bdebstrap --debug ...
```

3. 使用 -q 或 --quiet 选项的反向操作：
   确保没有使用这个选项，以获取更多输出信息。

4. 使用 --simulate 或 --dry-run 选项：
   这允许您快速检查包的选择和依赖关系，而不实际下载或安装任何东西。
```
bdebstrap --simulate ...
```

5. 使用 --setup-hook、--essential-hook、--customize-hook 和 --cleanup-hook：
   在不同阶段添加自定义命令来检查状态或执行调试操作。
```
bdebstrap --setup-hook "echo 'Setup complete'" --essential-hook "ls -l /" ...
```

6. 修改 --mode 选项：
   尝试不同的模式（如 sudo、root、unshare 等）来排除权限或隔离相关问题。
```
bdebstrap --mode sudo ...
```

7. 使用 --tmpdir 选项：
   指定一个自定义的临时目录，以便更容易检查中间文件。
```
bdebstrap --tmpdir /path/to/debug/dir ...
```

8. 检查日志文件：
   查看 /var/log/apt/、/var/log/dpkg.log 等目录下的日志文件。

9. 使用 strace：
   跟踪系统调用和信号。
```
strace bdebstrap ...
```

10. 使用环境变量：
    设置 APT 或 DPKG 的调试环境变量。
```
export APT_DEBUG=1
export DPKG_DEBUG=1
bdebstrap ...
```

11. 检查网络连接：
    确保能够访问指定的 Debian 镜像，例如 http://ftp.debian.org/debian/。

12. 使用 --aptopt 和 --dpkgopt：
    传递额外的配置选项给 apt 和 dpkg 以获取更多信息。
```
bdebstrap --aptopt="Debug::pkgProblemResolver=true" ...
```

13. 分步执行：
    如果可能，尝试将过程分解为多个步骤，单独执行每个步骤并检查结果。

这些方法可以单独使用，也可以组合使用，以帮助您诊断和解决 bdebstrap 过程中遇到的问题。根据具体遇到的问题，选择最合适的调试方法。

### trixie-rt-am62xx-evm.yaml 详解
```
---
mmdebstrap:
  architectures:
    - arm64
  mode: auto
  keyrings:
    - /usr/share/keyrings/debian-archive-keyring.gpg
  suite: trixie
  variant: standard
  hostname: am62xx-evm
  components:
    - main
    - contrib
    - non-free-firmware
  packages:
    - build-essential
    - gpg
    - curl
    - firmware-ti-connectivity
    - init
    - iproute2
    - less
    - libdrm-dev
    - libpam-systemd
    - locales
    - neofetch
    - network-manager
    - net-tools
    - openssh-server
    - sudo
    - vim
    - k3conf
    - weston
    - alsa-utils
    - libasound2-plugins
    - gstreamer1.0-tools
    - gstreamer1.0-plugins-base
    - gstreamer1.0-plugins-good
    - gstreamer1.0-plugins-bad
    - i2c-tools
    - linux-image-6.6.32-k3-rt
    - linux-headers-6.6.32-k3-rt
    - linux-libc-dev
    - cryptodev-linux-dkms
    - ti-img-rogue-driver-am62-dkms
    - ti-img-rogue-firmware-am62
    - ti-img-rogue-tools-am62
    - ti-img-rogue-umlibs-am62
    - firmware-ti-ipc-am62
    - firmware-cnm-wave
    - libti-rpmsg-char
    - libti-rpmsg-char-dev
    - libd3dadapter9-mesa-dev
    - libd3dadapter9-mesa
    - libegl-mesa0
    - libegl1-mesa
    - libgbm1
    - libgl1-mesa-dri
    - libgl1-mesa-glx
    - libglapi-mesa
    - libgles2-mesa
    - libglx-mesa0
    - libosmesa6
    - libwayland-egl1-mesa
    - mesa-opencl-icd
    - mesa-va-drivers
    - mesa-vdpau-drivers
    - mesa-vulkan-drivers
    - libpru-pssp-dev
    - pru-pssp
    - parted
    - e2fsprogs
    - chromium
  mirrors:
    - http://deb.debian.org/debian
  setup-hooks:
      # Setup TI Debian Package Repository
    - 'mkdir -p $1/etc/apt/sources.list.d/'
    - 'wget https://raw.githubusercontent.com/TexasInstruments/ti-debpkgs/main/ti-debpkgs.sources -P $1/etc/apt/sources.list.d/'
    - 'sed -i "s/bookworm/trixie/g" $1/etc/apt/sources.list.d/ti-debpkgs.sources'
      # Setup Apt repository preferences
    - 'mkdir -p $1/etc/apt/preferences.d/'
    - 'printf "Package: *\nPin: origin TexasInstruments.github.io\nPin-Priority: 1001" >> $1/etc/apt/preferences.d/ti-debpkgs'
      # Setup Kernel post-install scripts
    - 'mkdir -p $1/etc/kernel/postinst.d/'
    - 'echo "PWD = $PWD"'
    - 'upload target/kernel/postinst.d/cp-kernel-and-overlays /etc/kernel/postinst.d/cp-kernel-and-overlays'
    - 'chmod a+x $1/etc/kernel/postinst.d/cp-kernel-and-overlays'
  essential-hooks:
    # FIXME: Find a better workaround instead of sleep
    - 'sleep 10' # workaround for /proc resource busy unable to umount issue
  customize-hooks:
      # Remove passwd for root user
    - 'chroot "$1" passwd --delete root'
      # Fix apt install mandb permission issue
    - 'chroot "$1" chown -R man: /var/cache/man/'
    - 'chroot "$1" chmod -R 755 /var/cache/man/'
      # update packages to avoid mandatory update after first boot
    - 'chroot "$1" apt-get update'
      # Setup .bashrc for clean command-line experience
    - 'chroot "$1" cp /etc/skel/.bashrc ~/.bashrc'
      # Weston Service and Config Files
    - 'chroot "$1" mkdir -p /etc/systemd/system/'
    - 'upload target/weston/weston.service /etc/systemd/system/weston.service'
    - 'upload target/weston/weston.socket /etc/systemd/system/weston.socket'
    - 'chroot "$1" mkdir -p /etc/default/'
    - 'upload target/weston/weston /etc/default/weston'
    - '$BDEBSTRAP_HOOKS/enable-units "$1" weston'
    - 'chroot "$1" echo "export WAYLAND_DISPLAY=wayland-1" >> $1/etc/profile'
      # Enable ssh to root user without password
    - 'chroot "$1" echo "PermitRootLogin yes" >> $1/etc/ssh/sshd_config'
    - 'chroot "$1" echo "PermitEmptyPasswords yes" >> $1/etc/ssh/sshd_config'
      # Resize Rootfs Service
    - 'chroot "$1" mkdir -p /usr/bin'
    - 'upload target/resize_rootfs/resize_rootfs.sh /usr/bin/resize_rootfs.sh'
    - 'chroot "$1" chmod a+x /usr/bin/resize_rootfs.sh'
    - 'chroot "$1" mkdir -p /etc/systemd/system/'
    - 'upload target/resize_rootfs/resize_rootfs.service /etc/systemd/system/resize_rootfs.service'
    - '$BDEBSTRAP_HOOKS/enable-units "$1" resize_rootfs'
```

#### architectures
```bash
# 需要提供一个由逗号或空格分隔的架构列表，列表中的第一个架构是chroot内部的原生架构
--architectures ARCHITECTURES
Comma or whitespace separated list of architectures. 
The first architecture is the native architecture inside the chroot.
```

#### mode
```bash
# 选择合适的方法来执行chroot操作，并创建一个新的文件系统，其所有权信息与当前用户不同。
# 这通常用于隔离不同的环境或用户，以提高系统的安全性和灵活性。
--mode {auto,sudo,root,unshare,fakeroot,fakechroot,proot,chrootless}
Choose how to perform the chroot operation and 
create a filesystem with ownership information different from the current user.
```

这里是一些与Linux系统权限和容器技术相关的选项，通常用于控制程序的运行环境。

1. **auto**：自动模式，系统会根据需要自动选择最合适的运行环境。
2. **sudo**：使用sudo运行程序，意味着以超级用户（root）的权限执行命令，通常用于需要高权限的操作。
3. **root**：直接以root用户身份运行程序，拥有系统的最高权限。
4. **unshare**：使用unshare命令创建一个新的用户命名空间，使得程序在隔离的环境中运行，不会影响到宿主机的其他部分。
5. **fakeroot**：一种模拟root环境的工具，允许非root用户模拟root权限执行命令，但实际并不具备真正的root权限。
6. **fakechroot**：类似于fakeroot，fakechroot用于创建一个虚拟的chroot环境，使得程序认为自己在根目录下运行，但实际上是在指定的目录中。
7. **proot**：一个用户空间的chroot工具，允许用户无需root权限即可运行程序在隔离的环境中。
8. **chrootless**：不使用chroot环境运行程序，即程序直接在宿主机环境中运行，没有隔离。

这些选项通常用于容器技术或需要隔离运行环境的场景，以增强安全性和灵活性。

#### keyrings
```bash
# 密钥环是用来存储软件包签名密钥的
# APT在安装或更新软件包时会使用这些密钥来验证软件包的来源和完整性。
keyrings
list of default keyring to use by apt. Additional keyring files can be specified with --keyring.
```

#### suite
```bash
--suite SUITE, SUITE
The suite may be a valid release code name (eg, sid, stretch, jessie) 
or a symbolic name (eg, unstable, testing, stable, oldstable).
```

作用：指定使用的软件包套件（suite）。
这个套件可以是一个有效的发行版代号（例如 sid, stretch, jessie）
或者是符号名称（例如 unstable, testing, stable, oldstable）。

#### variant
```bash
--variant 
{extract,custom,essential,apt,required,minbase,
buildd,important,debootstrap,-,standard}
Choose which package set to install.
```
这段文字是命令行工具的参数说明，它描述了用户可以通过命令行选项来选择安装哪种软件包集合。具体来说，`--variant` 参数允许用户从一系列预定义的软件包集合中选择一个来安装。这些选项包括：

- `extract`：可能指安装用于文件提取的软件包。
- `custom`：允许用户自定义软件包集合。
- `essential`：安装基本必需的软件包。
- `apt`：安装用于软件包管理的 `apt` 工具相关的软件包。
- `required`：安装必需的软件包。
- `minbase`：安装最小基础系统所需的软件包。
- `buildd`：安装用于构建软件包的软件包。
- `important`：安装重要的软件包。
- `debootstrap`：安装 `debootstrap` 工具，它用于从官方镜像创建 Debian 系统。
- `-`：表示不安装任何额外的软件包。
- `standard`：安装标准软件包集合。

用户可以通过指定这些选项来定制他们需要的软件包集合。

#### hostname
```bash
--hostname HOSTNAME
Write the given HOSTNAME into /etc/hostname in the target chroot.
```

#### components
```bash
--components COMPONENTS
Comma or whitespace separated list of components 
like main, contrib and non-free which will be used for all URI-only MIRROR arguments.
```
这段内容是命令行工具的参数说明，其中特定部分的解释如下：

  这个参数允许用户指定一个由逗号或空格分隔的组件列表，例如 `main`、`contrib` 和 `non-free`。
  这些组件将被用于所有仅URI（统一资源标识符）的镜像参数。
  
  简单来说，当你在构建或配置一个基于Debian的系统时，你可能需要从不同的存储库中获取软件包。这些存储库被分为不同的组件，每个组件包含不同类型的软件包。
  
  通过使用 `--components` 参数，你可以明确指定要从哪些组件中获取软件包。

#### packages
```bash
--packages PACKAGES, --include PACKAGES
Comma or whitespace separated list of packages 
which will be installed in addition to the packages 
installed by the specified variant.
```

`--packages PACKAGES, --include PACKAGES` 参数允许用户指定一个由逗号或空格分隔的包列表，这些包将被安装在由指定的变体（variant）安装的包之外。这意味着，除了默认的包集合之外，用户还可以添加额外的包。这个功能在构建定制化的系统镜像时非常有用，因为它允许用户根据需要添加额外的工具或库。

#### mirrors
```bash
--mirrors MIRRORS, MIRRORS
Comma separated list of mirrors. 
If no mirror option is provided, http://deb.debian.org/debian is used.
```

[debian镜像\_debian下载地址\_debian安装教程-阿里巴巴开源镜像站](https://developer.aliyun.com/mirror/debian/?spm=a2c6h.25603864.0.0.1c3d29e8bWnHrt)

```bash
root@am62xx-evm:/# cat /etc/apt/sources.list
deb http://deb.debian.org/debian trixie main contrib non-free-firmware
```

#### setup-hooks
```bash
setup-hooks
list of setup hooks (string). 

Execute arbitrary commands right after initial setup (directory creation, configuration of apt and dpkg, ...) 
but before any packages are downloaded or installed. 

At that point, the chroot directory does not contain any executables and thus cannot be chroot-ed into. 

See HOOKS in mmdebstrap(1) for more information and examples. 
Additional setup hooks can be specified with --setup-hook.
```

这段内容描述的是在使用 `mmdebstrap` 工具时，如何通过 `setup-hooks` 参数来指定在初始设置后、下载或安装任何软件包之前执行的一系列自定义命令。具体来说：

- **setup-hooks**：这是一个字符串列表，用于定义一系列在初始设置完成后立即执行的命令。这些命令会在创建目录、配置 `apt` 和 `dpkg` 等之后执行，但在任何软件包被下载或安装之前。

- **执行时机**：在这个阶段，chroot 目录中还没有包含任何可执行文件，因此还无法进入 chroot 环境。

- **更多信息和示例**：如果需要了解更多关于这些钩子（hooks）的信息和示例，可以参考 `mmdebstrap(1)` 手册中的 "HOOKS" 部分。

- **指定额外的 setup hooks**：可以通过 `--setup-hook` 参数来指定额外的 setup hooks。这意味着你可以在命令行中添加多个 `--setup-hook` 参数，每个参数后面跟一个要执行的命令或脚本。

这个功能允许用户在软件包管理器和包安装之前进行一些自定义操作，比如预先配置环境、设置权限、或者执行一些特定的初始化任务。


```bash
  setup-hooks:
      # Setup TI Debian Package Repository
    - 'mkdir -p $1/etc/apt/sources.list.d/'
    - 'wget https://raw.githubusercontent.com/TexasInstruments/ti-debpkgs/main/ti-debpkgs.sources -P $1/etc/apt/sources.list.d/'
    - 'sed -i "s/bookworm/trixie/g" $1/etc/apt/sources.list.d/ti-debpkgs.sources'
      # Setup Apt repository preferences
    - 'mkdir -p $1/etc/apt/preferences.d/'
    - 'printf "Package: *\nPin: origin TexasInstruments.github.io\nPin-Priority: 1001" >> $1/etc/apt/preferences.d/ti-debpkgs'
      # Setup Kernel post-install scripts
    - 'mkdir -p $1/etc/kernel/postinst.d/'
    - 'echo "PWD = $PWD"'
    - 'upload target/kernel/postinst.d/cp-kernel-and-overlays /etc/kernel/postinst.d/cp-kernel-and-overlays'
    - 'chmod a+x $1/etc/kernel/postinst.d/cp-kernel-and-overlays'
```
这段 YAML 配置文件定义了一系列用于设置目标文件系统的 `setup-hooks`。这些 hooks 会在 `bdebstrap` 构建过程的初始阶段被执行，主要用于配置 APT 仓库和相关设置。下面是每个步骤的详细解释：

1. **创建 APT 源列表目录**
   ```yaml
   - mkdir -p $1/etc/apt/sources.list.d/
   ```
   这一行会确保 `$1/etc/apt/sources.list.d/` 目录存在，其中 `$1` 是指代目标文件系统的路径。

2. **下载德州仪器（Texas Instruments）的 APT 仓库源列表**
   ```yaml
   - wget https://raw.githubusercontent.com/TexasInstruments/ti-debpkgs/main/ti-debpkgs.sources -P $1/etc/apt/sources.list.d/
   ```
   这一行使用 `wget` 命令从 GitHub 上下载德州仪器的 APT 仓库配置文件，并将其保存到 `$1/etc/apt/sources.list.d/` 目录下。

3. **修改仓库源文件中的发行版名称**
   ```yaml
   - sed -i "s/bookworm/trixie/g" $1/etc/apt/sources.list.d/ti-debpkgs.sources
   ```
   使用 `sed` 命令替换仓库源文件中的发行版名称，将 `bookworm` 替换为 `trixie`。

4. **创建 APT 优先级目录**
   ```yaml
   - mkdir -p $1/etc/apt/preferences.d/
   ```
   创建 `$1/etc/apt/preferences.d/` 目录，用于存放 APT 优先级配置文件。

5. **设置 APT 仓库优先级**
   ```yaml
   - printf "Package: *\nPin: origin TexasInstruments.github.io\nPin-Priority: 1001" >> $1/etc/apt/preferences.d/ti-debpkgs
   ```
   这一行向 `$1/etc/apt/preferences.d/` 目录中的 `ti-debpkgs` 文件追加内容，设置了来自 `TexasInstruments.github.io` 的仓库具有最高的优先级（1001）。

6. **创建内核后安装脚本目录**
   ```yaml
   - mkdir -p $1/etc/kernel/postinst.d/
   ```
   创建 `$1/etc/kernel/postinst.d/` 目录，用于存放内核安装后的脚本。

7. **打印当前工作目录**
   ```yaml
   - echo "PWD = $PWD"
   ```
   打印出当前的工作目录，有助于调试。

8. **上传内核后处理脚本**
   ```yaml
   - upload target/kernel/postinst.d/cp-kernel-and-overlays /etc/kernel/postinst.d/cp-kernel-and-overlays
   ```
   将本地目录 `target/kernel/postinst.d/cp-kernel-and-overlays` 中的脚本复制到目标文件系统的 `/etc/kernel/postinst.d/` 目录下。这里的 `upload` 命令是一个假定的命令，实际使用时需要替换为有效的命令或脚本逻辑。

9. **设置内核后处理脚本的执行权限**
   ```yaml
   - chmod a+x $1/etc/kernel/postinst.d/cp-kernel-and-overlays
   ```
   设置 `$1/etc/kernel/postinst.d/cp-kernel-and-overlays` 脚本的执行权限，使其可以被执行。

这些步骤的目的是为了确保目标文件系统能够正确地配置德州仪器提供的 APT 仓库，并且能够在安装内核之后执行一些特定的脚本任务，比如复制内核和覆盖文件等。这样可以保证目标设备上使用的内核和其他组件是最新的，并且与德州仪器的硬件平台兼容。

#### essential-hooks
```bash
essential-hooks
list of essential hooks (string). 
Execute arbitrary commands after the Essential:yes packages have been installed, 
but before installing the remaining packages. 

See HOOKS in mmdebstrap(1) for more information and examples. 
Additional essential hooks can be specified with --essential-hook.
```
在上述片段中，"essential-hooks" 指的是一系列关键的设置钩子（hooks），它们是用于在安装了标记为 "Essential:yes" 的软件包之后、但在安装剩余软件包之前执行任意命令的列表。这些钩子在 mmdebstrap 工具中被用到，mmdebstrap 是一个用于在 Debian 系统上设置 chroot 环境的工具。

- **列表（list）**: 表示可以指定多个钩子，它们将按照指定的顺序执行。
- **字符串（string）**: 每个钩子都以字符串的形式给出，可以是任何命令或脚本。
- **执行（Execute）**: 表示这些钩子将在特定的安装阶段自动执行。
- **Essential:yes**: 这是一个特定的标记，用于指明某些软件包是系统运行所必需的。
- **mmdebstrap(1)**: 这是指 mmdebstrap 的手册页，其中包含了关于如何使用这些钩子的更多信息和示例。
- **--essential-hook**: 这是一个命令行选项，用于在 mmdebstrap 命令中添加额外的关键钩子。

简而言之，"essential-hooks" 允许用户在安装过程中的关键点执行自定义的命令或脚本，这可以用于配置系统、安装额外的依赖或执行其他必要的设置。

#### customize-hooks
```bash
customize-hooks
list of customize hooks (string). 
Execute arbitrary commands after the chroot is set up 
and all packages got installed but before final cleanup actions are carried out. 

See HOOKS in mmdebstrap(1) for more information and examples. 
Additional customize hooks can be specified with --customize-hook.
```

这段内容描述的是 `customize-hooks` 参数，它是一个字符串列表，用于在 chroot 环境设置完成并且所有软件包安装完毕后，但在进行最终清理操作之前，执行任意命令。这些自定义钩子（hooks）允许用户在安装过程的特定阶段执行额外的命令或脚本。

- **customize-hooks**：这是一个参数，用户可以指定一系列自定义的钩子命令。
- **list of customize hooks (string)**：表示用户可以提供一个字符串列表，其中包含要在特定阶段执行的命令。
- **Execute arbitrary commands**：意味着用户可以执行任何他们想要的命令。
- **after the chroot is set up and all packages got installed**：指出这些命令将在 chroot 环境配置完成并且所有软件包都已安装好之后执行。
- **before final cleanup actions are carried out**：这些命令会在进行最终的清理操作之前执行。
- **See HOOKS in mmdebstrap(1) for more information and examples**：提供了一个参考，用户可以通过查看 `mmdebstrap(1)` 的手册中的 `HOOKS` 部分来获取更多信息和示例。
- **Additional customize hooks can be specified with --customize-hook**：说明用户可以通过 `--customize-hook` 选项来指定额外的自定义钩子。

简而言之，`customize-hooks` 参数允许用户在软件包安装完成后和最终清理之前，执行自定义的命令或脚本，以进行额外的配置或修改。

```bash
      # Remove passwd for root user
    - 'chroot "$1" passwd --delete root'
      # Fix apt install mandb permission issue
    - 'chroot "$1" chown -R man: /var/cache/man/'
    - 'chroot "$1" chmod -R 755 /var/cache/man/'
      # update packages to avoid mandatory update after first boot
    - 'chroot "$1" apt-get update'
      # Setup .bashrc for clean command-line experience
    - 'chroot "$1" cp /etc/skel/.bashrc ~/.bashrc'
      # Weston Service and Config Files
    - 'chroot "$1" mkdir -p /etc/systemd/system/'
    - 'upload target/weston/weston.service /etc/systemd/system/weston.service'
    - 'upload target/weston/weston.socket /etc/systemd/system/weston.socket'
    - 'chroot "$1" mkdir -p /etc/default/'
    - 'upload target/weston/weston /etc/default/weston'
    - '$BDEBSTRAP_HOOKS/enable-units "$1" weston'
    - 'chroot "$1" echo "export WAYLAND_DISPLAY=wayland-1" >> $1/etc/profile'
      # Enable ssh to root user without password
    - 'chroot "$1" echo "PermitRootLogin yes" >> $1/etc/ssh/sshd_config'
    - 'chroot "$1" echo "PermitEmptyPasswords yes" >> $1/etc/ssh/sshd_config'
      # Resize Rootfs Service
    - 'chroot "$1" mkdir -p /usr/bin'
    - 'upload target/resize_rootfs/resize_rootfs.sh /usr/bin/resize_rootfs.sh'
    - 'chroot "$1" chmod a+x /usr/bin/resize_rootfs.sh'
    - 'chroot "$1" mkdir -p /etc/systemd/system/'
    - 'upload target/resize_rootfs/resize_rootfs.service /etc/systemd/system/resize_rootfs.service'
    - '$BDEBSTRAP_HOOKS/enable-units "$1" resize_rootfs'
```

这段 YAML 配置文件定义了一系列 `customize-hooks`，这些 hooks 会在 `bdebstrap` 构建过程的后期执行，主要用于定制目标文件系统的各种配置。以下是每个步骤的详细解释：

1. 移除 root 用户的密码
```yaml
- chroot "$1" passwd --delete root
```
这条命令进入目标文件系统的 chroot 环境，并移除 root 用户的密码。这意味着 root 用户将不需要密码即可登录。

2. 解决 `man` 命令的权限问题
```yaml
- 'chroot "$1" chown -R man: /var/cache/man/'
- chroot "$1" chmod -R 755 /var/cache/man/
```
这两条命令分别改变了 `/var/cache/man/` 目录及其子目录的所有权和权限，以修复 `man` 命令安装时可能出现的权限问题。

```bash
root@am62xx-evm:/# ls -l /var/cache/
drwxr-xr-x 34 man  man   396 Aug  4  2024 man
```

3. 更新软件包列表
```yaml
- chroot "$1" apt-get update
```
这条命令更新目标文件系统中的软件包索引文件，以确保后续安装或升级软件包时能够获取到最新的版本信息。

 4. 设置 `.bashrc` 文件
```yaml
- chroot "$1" cp /etc/skel/.bashrc ~/.bashrc
```
这条命令将 `/etc/skel/.bashrc` 文件复制到用户的主目录下，作为用户的 `.bashrc` 文件。这有助于提供一个干净的命令行体验。

5. 配置 Weston 显示服务器
```yaml
- chroot "$1" mkdir -p /etc/systemd/system/
- upload target/weston/weston.service /etc/systemd/system/weston.service
- upload target/weston/weston.socket /etc/systemd/system/weston.socket
- chroot "$1" mkdir -p /etc/default/
- upload target/weston/weston /etc/default/weston
- $BDEBSTRAP_HOOKS/enable-units "$1" weston
- chroot "$1" echo "export WAYLAND_DISPLAY=wayland-1" >> $1/etc/profile
```
这些命令首先创建必要的目录，然后上传 `weston.service` 和 `weston.socket` 到目标文件系统的 `/etc/systemd/system/` 目录下，同时上传 `weston` 配置文件到 `/etc/default/` 目录。最后启用 `weston` 服务并设置环境变量 `WAYLAND_DISPLAY`，使得 Weston 可以作为 Wayland 显示服务器运行。

6. 允许无密码的 root 用户 SSH 登录
```yaml
- chroot "$1" echo "PermitRootLogin yes" >> $1/etc/ssh/sshd_config
- chroot "$1" echo "PermitEmptyPasswords yes" >> $1/etc/ssh/sshd_config
```
这两条命令允许 root 用户无需密码通过 SSH 登录，并允许空密码登录。需要注意的是，这种配置存在安全风险，应谨慎使用。

 7. 配置根文件系统调整大小的服务
```yaml
- chroot "$1" mkdir -p /usr/bin
- upload target/resize_rootfs/resize_rootfs.sh /usr/bin/resize_rootfs.sh
- chroot "$1" chmod a+x /usr/bin/resize_rootfs.sh
- chroot "$1" mkdir -p /etc/systemd/system/
- upload target/resize_rootfs/resize_rootfs.service /etc/systemd/system/resize_rootfs.service
- $BDEBSTRAP_HOOKS/enable-units "$1" resize_rootfs
```
这些命令创建必要的目录，上传 `resize_rootfs.sh` 脚本并赋予其执行权限，上传 `resize_rootfs.service` 到 `/etc/systemd/system/` 目录下，并启用该服务。这使得系统可以在启动时自动调整根文件系统的大小。

这些步骤的目的是为了确保目标文件系统在启动时能够正确配置和运行，并提供一定的便利性和安全性。需要注意的是，允许无密码的 root 用户 SSH 登录是一种高风险的操作，应在生产环境中避免使用。

### 工作原理

`bdebstrap` 工具的工作原理是创建一个新的 Debian 系统环境。这个环境可以是一个目录（模拟一个完整的操作系统），也可以是一个压缩文件（例如 tarball 或者 squashfs 形式的文件系统映像）。在创建这个环境的过程中，`bdebstrap` 会做以下几件事情：

1. **初始化**: 创建必要的目录结构，并配置 APT 和 dpkg。
2. **下载包**: 根据指定的套件（发行版）和变体（package set），从指定的镜像站点下载必要的二进制包。
3. **安装包**: 安装下载的包到目标环境中。
4. **执行钩子脚本**: 可以在不同的阶段执行用户提供的脚本，比如在初始设置后、安装必需包后、所有包安装完成后等。
5. **清理**: 执行任何最终的清理操作，并准备最终的输出。

通过这种方式，`bdebstrap` 允许用户创建一个完全自定义的 Debian 根文件系统，这对于开发嵌入式系统、服务器镜像或者是其他需要定制化操作系统的场景非常有用。

# 预编译debian
```bash
tisdk-debian-trixie-rt-am62xx-evm.wic.xz
```

## 解压xz
```bash
xz -d tisdk-debian-trixie-rt-am62xx-evm.wic.xz
```

## 复制内容
```bash
fdisk -l tisdk-debian-trixie-rt-am62xx-evm.wic
```
```bash
# fdisk -l tisdk-debian-trixie-rt-am62xx-evm.wic
Disk tisdk-debian-trixie-rt-am62xx-evm.wic: 5 GiB, 5368709120 bytes, 10485760 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xa52e57a0

Device                                 Boot  Start      End  Sectors  Size Id Type
tisdk-debian-trixie-rt-am62xx-evm.wic1 *      2048   264191   262144  128M  c W95 FAT32 (LBA)
tisdk-debian-trixie-rt-am62xx-evm.wic2      264192 10485759 10221568  4.9G 83 Linux
```

根据你提供的 `fdisk` 输出，我们可以看到 `tisdk-debian-trixie-rt-am62xx-evm.wic` 文件包含两个分区：

1. FAT32 分区（启动分区），起始扇区为 2048
2. Linux 分区（根文件系统），起始扇区为 264192

我们可以按照以下步骤将这两个分区的内容释放到目录中：

1. 创建挂载点和目标目录：

```bash
sudo mkdir -p /mnt/wic_boot /mnt/wic_root
mkdir -p extracted_content/boot extracted_content/root
```

2. 挂载并复制启动分区（FAT32）内容：

```bash
sudo mount -o loop,offset=$((2048*512)) tisdk-debian-trixie-rt-am62xx-evm.wic /mnt/wic_boot
sudo cp -a /mnt/wic_boot/* extracted_content/boot/
sudo umount /mnt/wic_boot
```

3. 挂载并复制根文件系统分区（Linux）内容：

```bash
sudo mount -o loop,offset=$((264192*512)) tisdk-debian-trixie-rt-am62xx-evm.wic /mnt/wic_root
sudo cp -a /mnt/wic_root/* extracted_content/root/
sudo umount /mnt/wic_root
```

4. 调整权限（可选，但建议执行）：

```bash
sudo chown -R $(id -u):$(id -g) extracted_content
```

完成这些步骤后，`extracted_content` 目录将包含两个子目录：

- `boot`：包含启动分区的内容
- `root`：包含根文件系统的内容

现在你已经成功地将 `tisdk-debian-trixie-rt-am62xx-evm.wic` 文件的内容释放到了 `extracted_content` 目录中。

注意事项：
- 确保你有足够的磁盘空间来存储提取的内容。
- 如果遇到权限问题，请确保你使用了 `sudo` 来执行需要root权限的命令。
- 在复制大量文件时，可能需要一些时间，请耐心等待。

如果在执行过程中遇到任何问题，请告诉我，我会进一步协助你。


# 测试
```bash
neofetch
```
![[Pasted image 20240921100801.png]]


