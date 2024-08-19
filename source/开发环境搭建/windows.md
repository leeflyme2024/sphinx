# Windows  常用命令示例

Windows 命令提示符（CMD）提供了一系列内置命令，用于执行多种任务，从文件和目录管理到网络诊断等。以下是一些常用的 CMD 命令及其示例：

## 目录操作

### 列出当前目录下的文件和子目录

```bash
dir
```

### 切换目录

```bash
cd <directory-path>
```

例如，切换到用户目录：

```bash
cd C:\Users\YourUsername
```

### 创建新目录

```bash
mkdir <directory-name>
```

例如，创建一个名为 `Documents` 的目录：

```bash
mkdir Documents
```

### 删除空目录

```bash
rmdir <directory-name>
```

例如，删除一个名为 `temp` 的空目录：

```bash
rmdir temp
```

## 文件操作

### 复制文件

```bash
copy <source-file> <destination-file>
```

例如，复制一个文件 `example.txt` 到 `backup.txt`：

```bash
copy example.txt backup.txt
```

#### XCOPY 示例

`xcopy` 命令用于在 Windows 命令行环境中复制文件和目录。它比 `copy` 命令更加强大，提供了更多的选项，如递归复制目录和匹配文件时间戳。以下是一些使用 `xcopy` 的常见示例：

##### 复制单个文件

```bash
xcopy source-file destination-file
```

例如，复制 `source.txt` 到 `C:\destination` 目录下：

```bash
xcopy source.txt C:\destination
```

##### 递归复制整个目录

```bash
xcopy source-directory destination-directory /s /e
```

例如，递归复制 `C:\source\dir` 到 `C:\destination`，包括空目录：

```bash
xcopy C:\source\dir C:\destination /s /e
```

##### 复制目录但不包括子目录

```bash
xcopy source-directory destination-directory /e
```

例如，复制 `C:\source\dir` 内容到 `C:\destination`，但不包括其子目录：

```bash
xcopy C:\source\dir C:\destination /e
```

##### 复制目录并覆盖现有文件

```bash
xcopy source-directory destination-directory /c
```

例如，复制 `C:\source\dir` 到 `C:\destination`，即使目标文件已存在也覆盖：

```bash
xcopy C:\source\dir C:\destination /c
```

##### 同步目录

```bash
xcopy source-directory destination-directory /m /i
```

例如，同步 `C:\source\dir` 到 `C:\destination`，只复制那些在源目录中有更改的文件：

```bash
xcopy C:\source\dir C:\destination /m /i
```

##### 复制目录并显示进度

```bash
xcopy source-directory destination-directory /p
```

例如，复制 `C:\source\dir` 到 `C:\destination` 并在复制时显示进度：

```bash
xcopy C:\source\dir C:\destination /p
```

##### 复制目录并保持时间戳

```bash
xcopy source-directory destination-directory /t
```

例如，复制 `C:\source\dir` 到 `C:\destination` 并保持文件的时间戳：

```bash
xcopy C:\source\dir C:\destination /t
```

##### 复制目录并隐藏文件

```bash
xcopy source-directory destination-directory /h
```

例如，复制 `C:\source\dir` 包括隐藏文件到 `C:\destination`：

```bash
xcopy C:\source\dir C:\destination /h
```

#### ROBOCOPY 示例

`roboCopy` 是一个强大的文件复制命令行工具，用于在 Windows 系统中复制文件和目录，特别是用于数据迁移和备份。以下是一些使用 `roboCopy` 的常见示例：

##### 复制目录和所有子目录

```bash
roboCopy source_directory destination_directory /MIR
```

例如，镜像复制 `C:\source` 到 `D:\destination`：

```bash
roboCopy C:\source D:\destination /MIR
```

##### 复制目录，但只包含新文件或更新过的文件

```bash
roboCopy source_directory destination_directory /MAXAGE:YYYY-MM-DD
```

例如，复制 `C:\source` 到 `D:\destination`，只包含在 2023-01-01 之后修改的文件：

```bash
roboCopy C:\source D:\destination /MAXAGE:2023-01-01
```

##### 复制目录，包括只读和隐藏的文件

```bash
roboCopy source_directory destination_directory /E /XJ /XO
```

例如，完整复制 `C:\source` 到 `D:\destination`，包括隐藏和只读文件：

```bash
roboCopy C:\source D:\destination /E /XJ /XO
```

##### 复制目录，但排除特定类型的文件

```bash
roboCopy source_directory destination_directory /XF *.log *.tmp
```

例如，复制 `C:\source` 到 `D:\destination`，但排除所有 `.log` 和 `.tmp` 文件：

```bash
roboCopy C:\source D:\destination /XF *.log *.tmp
```

##### 复制目录，显示详细进度报告

```bash
roboCopy source_directory destination_directory /LOG+:logfile.txt
```

例如，复制 `C:\source` 到 `D:\destination`，并将详细日志输出到 `logfile.txt`：

```bash
roboCopy C:\source D:\destination /LOG+:logfile.txt
```

##### 复制目录，仅在发生错误时才显示消息

```bash
roboCopy source_directory destination_directory /TEE /LOG+:logfile.txt /NP /R:1 /W:1
```

例如，复制 `C:\source` 到 `D:\destination`，只在出现错误时显示消息，并将错误日志写入 `logfile.txt`：

```bash
roboCopy C:\source D:\destination /TEE /LOG+:logfile.txt /NP /R:1 /W:1
```

##### 使用 ROBOCOPY 进行增量备份

```bash
roboCopy source_directory destination_directory /B /MIR
```

例如，执行 `C:\source` 到 `D:\destination` 的增量备份：

```bash
roboCopy C:\source D:\destination /B /MIR
```

### 移动文件

```bash
move <source-file> <destination-file>
```

例如，将 `example.txt` 移动到 `C:\Backup` 目录下：

```bash
move example.txt C:\Backup
```

### 删除文件

```bash
del <file-name>
```

例如，删除一个名为 `tempfile.txt` 的文件：

```bash
del tempfile.txt
```

### 批量删除文件

```bash
del <file-pattern>
```

例如，删除所有 `.tmp` 文件：

```bash
del *.tmp
```

### 重命名文件

`rename` 命令用于在 Windows 命令行界面中批量重命名文件。

#### 更改文件扩展名
```bash
rename file.txt file.md
```

#### 批量更改文件名
```bash
rename *.txt file_*.txt
```

这将把所有 `.txt` 文件重命名为 `file_` 加上原来的文件名。

#### 替换文件名中的特定字符
```bash
rename file old new
```

例如，将所有文件名中的 "old" 替换为 "new"：

```bash
rename *.* old new
```

请注意，Windows 的 `rename` 命令在不加任何参数的情况下将显示帮助信息，因此实际使用时应直接指定文件或文件模式和新的文件名。上述示例中的语法实际上是假设了一个更通用的语法形式，而在实际使用中，你需要具体指定文件名或模式匹配以及新的文件名。

例如，如果你有一批文件名如下：

- file1_old.txt
- file2_old.txt

你可以使用以下命令将所有的 "_old" 替换为 "_new"：

```bash
for %%i in (*.txt) do ren "%%i" "%%~ni_new%%~xi"
```

这里 `%~ni` 获取文件名而不包括扩展名，`%~xi` 获取扩展名，这样你就可以构建一个新的文件名，将 "_old" 替换成 "_new"。注意，这里的 `ren` 命令是 `rename` 的别名，在 Windows 命令行中可以互换使用。

## 环境变量

`set` 命令用于在 Windows 命令行中显示、设置或删除环境变量。

### 显示所有环境变量

```bash
set
```

### 设置环境变量

```bash
set variable_name=value
```

例如，设置一个名为 `MY_VARIABLE` 的环境变量，值为 `Hello World`：

```bash
set MY_VARIABLE=Hello World
```

### 显示特定环境变量的值

```bash
set variable_name
```

例如，显示 `MY_VARIABLE` 的值：

```bash
set MY_VARIABLE
```

### 删除环境变量

```bash
set variable_name=
```

例如，删除 `MY_VARIABLE` 环境变量：

```bash
set MY_VARIABLE=
```

### 在批处理文件中使用延迟变量扩展

在批处理文件中，如果你想使用最新赋值给变量的值，你需要启用延迟变量扩展：

```bash
@echo off
setlocal enabledelayedexpansion
set MY_VAR=Initial Value
echo Original value: !MY_VAR!
set MY_VAR=New Value
echo New value: !MY_VAR!
endlocal
```

在这个例子中，`!MY_VAR!` 是用于访问延迟扩展变量的方式。

### 在批处理文件中设置和使用变量

你可以在批处理文件中设置变量，并在文件内部使用它们：

```bash
@echo off
set MY_PATH=C:\MyFolder
echo Current path is %MY_PATH%
```

在这个例子中，`%MY_PATH%` 是用来引用变量的常规方式。

## 目录树

`tree` 命令用于在 Windows 命令行界面中以图形方式显示目录结构。尽管 `tree` 命令本身不是 Windows 的内置命令，但在安装了某些工具如 Git Bash 或通过 PowerShell 的第三方工具如 `tree.exe` 后，你可以使用它来查看目录树。以下是在类似环境中的使用示例：

### 显示当前目录的目录结构

```bash
tree
```

### 显示指定目录的目录结构

```bash
tree /d /f /a <directory>
```

例如，显示 `C:\Users\YourUsername` 目录的结构：

```bash
tree /d /f /a C:\Users\YourUsername
```

这里的 `/d` 选项表示只列出目录，`/f` 表示显示完整路径，`/a` 表示显示所有目录，包括系统目录。

### 显示目录结构并包含文件大小

```bash
tree /d /f /a /z <directory>
```

例如，显示 `C:\Projects` 目录的结构，并包含每个文件和目录的大小：

```bash
tree /d /f /a /z C:\Projects
```

这里的 `/z` 选项表示显示文件大小。

### 显示目录结构并限制深度

```bash
tree /L <depth> <directory>
```

例如，显示 `C:\Windows` 目录的结构，但只显示前两级目录：

```bash
tree /L 2 C:\Windows
```

这里的 `/L` 选项后面跟的是要显示的目录深度。

### 显示目录结构并按修改时间排序

```bash
tree /t <directory>
```

例如，显示 `C:\Documents` 目录的结构，并按文件和目录的最后修改时间排序：

```bash
tree /t C:\Documents
```

这里的 `/t` 选项表示按时间排序。



## 系统信息

### 显示系统信息

```bash
systeminfo
```

### 查看磁盘空间

```bash
df
```

请注意，`df` 命令在 Windows CMD 中可能需要通过安装 Cygwin 或其他类 Unix 环境才能使用。默认情况下，你可以使用 `fsutil volume diskfree c:` 来查看 C: 盘的磁盘空间。

### 查看网络配置

```bash
ipconfig
```

## 网络工具

### Ping 测试

```bash
ping <hostname-or-ip-address>
```

例如，测试与 Google 的连通性：

```bash
ping www.google.com
```

### 打开网络命令提示符

```bash
netsh
```

一旦进入 `netsh`，你可以使用多种命令来配置和查询网络设置。

### 查看开放端口

```bash
netstat -ano
```

这将显示所有活动的 TCP 和 UDP 端口以及它们所属的进程 ID。


# PowerShell
## disk

```powershell
PS C:\Windows\system32> help *disk

Name                              Category  Module                    Synopsis
----                              --------  ------                    --------
Add-PhysicalDisk                  Function  Storage                   ...
Clear-Disk                        Function  Storage                   ...
Connect-VirtualDisk               Function  Storage                   ...
Disconnect-VirtualDisk            Function  Storage                   ...
Get-Disk                          Function  Storage                   ...
Get-PhysicalDisk                  Function  Storage                   ...
Get-VirtualDisk                   Function  Storage                   ...
Hide-VirtualDisk                  Function  Storage                   ...
Initialize-Disk                   Function  Storage                   ...
New-StorageSubsystemVirtualDisk   Function  Storage                   ...
New-VirtualDisk                   Function  Storage                   ...
Remove-PhysicalDisk               Function  Storage                   ...
Remove-VirtualDisk                Function  Storage                   ...
Repair-VirtualDisk                Function  Storage                   ...
Reset-PhysicalDisk                Function  Storage                   ...
Resize-VirtualDisk                Function  Storage                   ...
Set-Disk                          Function  Storage                   ...
Set-PhysicalDisk                  Function  Storage                   ...
Set-VirtualDisk                   Function  Storage                   ...
Show-VirtualDisk                  Function  Storage                   ...
Update-Disk                       Function  Storage                   ...
New-PmemDisk                      Cmdlet    PersistentMemory          New-PmemDisk...
Get-PmemDisk                      Cmdlet    PersistentMemory          Get-PmemDisk...
Remove-PmemDisk                   Cmdlet    PersistentMemory          Remove-PmemDisk...
Enable-StorageBusDisk             Function  StorageBusCache           ...
Suspend-StorageBusDisk            Function  StorageBusCache           ...
Disable-StorageBusDisk            Function  StorageBusCache           ...
Resume-StorageBusDisk             Function  StorageBusCache           ...
Clear-StorageBusDisk              Function  StorageBusCache           ...
Get-StorageBusDisk                Function  StorageBusCache           ...
```

### Get-Disk
``` PowerShell
PS C:\Windows\system32> Get-Disk

Number Friendly Name                                                                                                                                                        Serial Number                    HealthStatus         OperationalStatus      Total Size Partition
                                                                                                                                                                                                                                                                    Style
------ -------------                                                                                                                                                        -------------                    ------------         -----------------      ---------- ----------
0      KINGSTON SNV2S1000G                                                                                                                                                  0000_0000_0000_0000_0026_B738... Healthy              Online                  931.51 GB GPT
3      Generic STORAGE DEVICE                                                                                                                                               [                                Healthy              Online                   59.45 GB MBR

```

### Get-PhysicalDisk
```PowerShell
PS C:\Windows\system32> Get-PhysicalDisk

Number FriendlyName           SerialNumber                             MediaType   CanPool OperationalStatus HealthStatus Usage            Size
------ ------------           ------------                             ---------   ------- ----------------- ------------ -----            ----
0      KINGSTON SNV2S1000G    0000_0000_0000_0000_0026_B738_2EB0_D4E5. SSD         False   OK                Healthy      Auto-Select 931.51 GB
3      Generic STORAGE DEVICE [                                        Unspecified False   OK                Healthy      Auto-Select  59.45 GB

```

## Partition
```powershell
PS C:\Windows\system32> help *Partition

Name                              Category  Module                    Synopsis
----                              --------  ------                    --------
Get-Partition                     Function  Storage                   ...
New-Partition                     Function  Storage                   ...
Remove-Partition                  Function  Storage                   ...
Resize-Partition                  Function  Storage                   ...
Set-Partition                     Function  Storage                   ...
```

### Get-Partition
```powershell
PS C:\Windows\system32> Get-Partition


   DiskPath:\\?\scsi#disk&ven_nvme&prod_kingston_snv2s10#5&34ac7fdb&0&000000#{53f56307-b6bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                                                                                                    Size Type
---------------  ----------- ------                                                                                                                    ---- ----
1                           1048576                                                                                                                 100 MB System
2                           105906176                                                                                                                16 MB Reserved
3                C           122683392                                                                                                            654.82 GB Basic
4                D           703230640128                                                                                                            276 GB Basic
5                           999583383552                                                                                                            591 MB Recovery


   DiskPath:\\?\usbstor#disk&ven_generic&prod_storage_device&rev_1404#7&2c1956d5&0#{53f56307-b6bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                                                                                                    Size Type
---------------  ----------- ------                                                                                                                    ---- ----
1                F           1048576                                                                                                                  16 GB FAT32 XINT13
2                G           17180917760                                                                                                           43.45 GB IFS
```

## Volume
```powershell
PS C:\Windows\system32> help *Volume

Name                              Category  Module                    Synopsis
----                              --------  ------                    --------
Flush-Volume                      Alias                               Write-VolumeCache
Initialize-Volume                 Alias                               Format-Volume
Debug-Volume                      Function  Storage                   ...
Format-Volume                     Function  Storage                   ...
Get-Volume                        Function  Storage                   ...
New-Volume                        Function  Storage                   ...
Optimize-Volume                   Function  Storage                   ...
Repair-Volume                     Function  Storage                   ...
Set-Volume                        Function  Storage                   ...
Dismount-AppxVolume               Cmdlet    Appx                      Dismount-AppxVolume...
Add-AppxVolume                    Cmdlet    Appx                      Add-AppxVolume...
Set-AppxDefaultVolume             Cmdlet    Appx                      Set-AppxDefaultVolume...
Get-AppxDefaultVolume             Cmdlet    Appx                      Get-AppxDefaultVolume...
Mount-AppxVolume                  Cmdlet    Appx                      Mount-AppxVolume...
Get-AppxVolume                    Cmdlet    Appx                      Get-AppxVolume...
Remove-AppxVolume                 Cmdlet    Appx                      Remove-AppxVolume...
Get-BitLockerVolume               Function  BitLocker                 ...
```

### Get-Volume
```powershell
PS C:\Windows\system32> Get-Volume

DriveLetter FriendlyName FileSystemType DriveType HealthStatus OperationalStatus SizeRemaining      Size
----------- ------------ -------------- --------- ------------ ----------------- -------------      ----
D           新加卷       NTFS           Fixed     Healthy      OK                     87.26 GB    276 GB
G           M62xx-TRoot  Unknown        Removable Healthy      OK                     43.44 GB  43.44 GB
                         NTFS           Fixed     Healthy      OK                      84.3 MB    591 MB
F           M62XX-TBOOT  FAT32          Removable Healthy      OK                     15.99 GB  15.99 GB
C                        NTFS           Fixed     Healthy      OK                     213.1 GB 654.82 GB
                         FAT32          Fixed     Healthy      OK                      69.2 MB     96 MB
```