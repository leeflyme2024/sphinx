# 命令集
```bash
C:\Users\leefly>wsl --help
版权所有 (c) Microsoft Corporation。保留所有权利。
有关此产品的隐私信息，请访问 https://aka.ms/privacy。

用法: wsl.exe [参数] [选项...][命令行]

运行 Linux 二进制文件的参数:

   如果未提供命令行，wsl.exe 将启动默认 shell。

   --exec, -e <CommandLine>
       在不使用默认 Linux shell 的情况下执行指定的命令。

   --shell-type <standard|login|none>
       使用提供的 shell 类型执行指定的命令。

   --
       按原样传递剩余的命令行。

选项:
   --cd <Directory>
       将指定目录设置为当前工作目录。
       如果使用 ~，则将使用 Linux 用户的主路径。如果路径以
       /字符开始，它将解释为绝对 Linux 路径。
       否则，该值必须是绝对 Windows 路径。

   --distribution, -d <Distro>
       运行指定的分发版。

   --user, -u <UserName>
       以指定用户身份运行。

   --system
       为系统分发版启动 shell。

用于管理适用于 Linux 的 Windows 子系统的参数:

   --help
       显示使用情况信息。

   --debug-shell
       出于诊断目的打开 WSL2 调试 shell。

   --install [发行版] [选项...]
       安装适用于 Linux 的 Windows 子系统分发版。
       有关有效分发版的列表，请使用 'wsl.exe --list --online'。

       选项:
           --no-launch, -n
               安装后不要启动分发版。

           --web-download
               从 Internet 而不是 Microsoft Store 下载分发版。

           --no-distribution
               仅安装所需的可选组件，不安装分发版。

           --enable-wsl1
               启用 WSL1 支持。

   --manage <Distro> <Options...>
       更改发行版特定选项。

       选项:
           --set-sparse, -s <true|false>
               将发行版的 vhdx 设置为稀疏，从而允许自动回收磁盘空间。

   --mount <Disk>
       在所有 WSL 2 分发版中附加和装载物理磁盘或虚拟磁盘。

       选项:
           --vhd
               指定 <Disk> 引用虚拟硬盘。

           --bare
               将磁盘附加到 WSL2，但不要装载它。

           --name <Name>
               使用装入点的自定义名称装载磁盘。

           --type <Type>
               装载磁盘时要使用的文件系统(如果未指定)默认为 ext4。

           --options <Options>
               其他装载选项。

           --partition <Index>
               要装载的分区的索引(如果未指定)默认为整个磁盘。

   --set-default-version <Version>
       更改新分发版的默认安装版本。

   --shutdown
       立即终止所有正在运行的分发版和 WSL 2
       轻型实用工具虚拟机。

   --status
       显示适用于 Linux 的 Windows 子系统状态。

   --unmount [磁盘]
       从所有 WSL2 分发版中卸载和分离磁盘。
       如果在没有参数的情况下调用，则卸载和分离所有磁盘。

   --uninstall
       从此计算机卸载适用于 Linux 的 Windows 子系统包。

   --update
       更新适用于 Linux 的 Windows 子系统包。

       选项:
           --pre-release
               下载预发行版本(如果可用)。

   --version, -v
       显示版本信息。

用于在适用于 Linux 的 Windows 子系统中管理分发版的参数:

   --export <Distro> <FileName> [选项]
       将分发版导出到 tar 文件。
       文件名可以是 - for stdout。

       选项:
           --vhd
               指定应将分发版导出为 .vhdx 文件。

   --import <Distro> <InstallLocation> <FileName> [选项]
       将指定的 tar 文件作为新分发版导入。
       文件名可以是 - for stdin。

       选项:
           --version <Version>
               指定要用于新分发的版本。

           --vhd
               指定所提供的文件是 .vhdx 文件，而不是 tar 文件。
               此操作在指定的安装位置创建 .vhdx 文件的副本。

   --import-in-place <Distro> <FileName>
       将指定的 .vhdx 文件作为新分发版导入。
       必须使用 ext4 文件系统类型设置此虚拟硬盘的格式。

   --list, -l [选项]
       列出分发版。

       选项:
           --all
               列出所有分发版，包括当前
               正在安装或卸载的分发版。

           --running
               仅列出当前正在运行的分发版。

           --quiet, -q
               仅显示分发版名称。

           --verbose, -v
               显示有关所有分发版的详细信息。

           --online, -o
               显示适合通过 'wsl.exe --install' 安装的可用分发版列表。

   --set-default, -s <Distro>
       将分布版设置为默认值。

   --set-version <Distro> <Version>
       更改指定分发版的版本。

   --terminate, -t <Distro>
       终止指定的分发版。

   --unregister <Distro>
       取消注册分发版并删除根文件系统。
```

# 配置

```ini
# Settings apply across all Linux distros running on WSL 2
[wsl2]

# Limits VM memory to use no more than 2 GB, this can be set as whole numbers using GB or MB
memory=16GB

# Sets the VM to use two virtual processors
processors=16

# Sets amount of swap storage space to 2GB, default is 25% of available RAM
swap=8GB

# Sets swapfile path location, default is %USERPROFILE%\AppData\Local\Temp\swap.vhdx
# swapfile=C:\\temp\\wsl-swap.vhdx
```

在 WSL（Windows Subsystem for Linux）中的 Ubuntu 增加内存大小并不是直接配置 Ubuntu 内部的某个设置，而是通过修改 WSL 的配置文件来实现的。以下是增加 WSL 中内存大小的步骤：

### 修改 WSL 配置文件

1. **打开 WSL 配置文件**：
   在 Windows 中，WSL 的配置文件是 `wsl.conf`。该文件通常位于 `%UserProfile%/.wslconfig`，即 `C:\Users\leefly\.wslconfig`。

2. **编辑 `.wslconfig` 文件**：
   使用你喜欢的文本编辑器（例如 Notepad）打开或创建 `.wslconfig` 文件。

3. **配置内存限制**：
   在 `.wslconfig` 文件中，添加或修改以下内容：
```ini
[wsl2]
memory=4GB   # 设置为你需要的内存大小
processors=4 # 设置为你需要的CPU核心数量（可选）
```

4. **保存并关闭文件**。

### 重新启动 WSL

在配置文件修改完毕后，需要重新启动 WSL 才能应用新的配置。

1. **关闭所有 WSL 实例**：
   打开 Windows PowerShell 或 CMD，然后运行以下命令以确保所有 WSL 实例都关闭：
```powershell
wsl --shutdown
wsl --list --verbose
```

2. **重新启动 WSL**：
   重新启动你的 WSL 终端（例如，Ubuntu），新的内存配置将生效。

### 检查新的内存配置

启动 WSL 后，你可以通过以下命令来检查当前的内存使用情况，确保新的配置已生效：
```bash
free -h
```

### 清理虚拟磁盘空间
#### 关闭WSL所有示例
```powershell
wsl --shutdown
wsl --list --verbose
```

#### 分区
```bash
diskpart
```

#### 清理空间
##### ubuntu 18.04
```bash
select vdisk file="C:\Users\leefly\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04LTS_79rhkp1fndgsc\LocalState\ext4.vhdx"
attach vdisk readonly
compact vdisk
detach vdisk
```

##### ubuntu 22.04
![[企业微信截图_17322651818733.png]]
![[企业微信截图_1732265189641.png]]
```bash
select vdisk file="C:\Users\leefly\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc\LocalState\ext4.vhdx"
attach vdisk readonly
compact vdisk
detach vdisk
```

##### WSL
```bash
select vdisk file="C:\Users\leefly\AppData\Local\Docker\wsl\data\ext4.vhdx"
attach vdisk readonly
compact vdisk
detach vdisk
```