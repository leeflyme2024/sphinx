# 命令集
## systemctl
### 查看所有服务的状态
```bash
systemctl list-units --type=service
```

### 启动服务
```bash
systemctl start <service-name>.service
```

例如，启动 SSH 服务：

```bash
systemctl start sshd.service
```

### 停止服务
```bash
systemctl stop <service-name>.service
```

例如，停止 SSH 服务：

```bash
systemctl stop sshd.service
```

### 重启服务
```bash
systemctl restart <service-name>.service
```

例如，重启 SSH 服务：

```bash
systemctl restart sshd.service
```

### 使服务开机启动
```bash
systemctl enable <service-name>.service
```

例如，设置 SSH 服务在开机时启动：

```bash
systemctl enable sshd.service
```

### 取消服务开机启动
```bash
systemctl disable <service-name>.service
```

例如，取消 SSH 服务在开机时启动：

```bash
systemctl disable sshd.service
```

### 查看服务状态
```bash
systemctl status <service-name>.service
```

例如，查看 SSH 服务的状态：

```bash
systemctl status sshd.service
```

### 查看服务日志
```bash
journalctl -u <service-name>.service
```

例如，查看 SSH 服务的日志：
```bash
journalctl -u sshd.service
```

### 重启系统
```bash
systemctl reboot
```

#### reboot 命令 和 systemctl reboot 命令区别

在 Systemd 环境中，`reboot` 和 `systemctl reboot` 这两个命令最终都是用来重启系统的，但是它们的执行上下文和机制有所不同。

##### reboot 命令

`reboot` 是一个传统的 Unix/Linux 命令，它直接调用内核的重启功能。这个命令通常位于 `/sbin` 目录下，它通过向内核发送一个特定信号（SIGSYS）来请求重启，或者通过调用 `libc` 库中的 `sync()` 和 `reboot()` 系统调用来同步文件系统并重启系统。

##### systemctl reboot 命令

`systemctl reboot` 是 Systemd 系统和服务管理器的一部分。当你使用 `systemctl reboot` 时，Systemd 会尝试优雅地关闭所有的服务和系统组件，确保所有正在运行的服务都正确地保存状态并终止。然后，Systemd 会执行重启操作。

Systemd 在执行 `reboot` 之前会尝试执行 `shutdown` 操作，这意味着它会按照服务依赖关系的反向顺序停止服务，确保所有的服务都有机会进行清理。这通常比直接使用 `reboot` 命令更加安全和可靠，因为它减少了数据丢失的风险。

##### 总结

- **`reboot`**：直接重启系统，可能不会等待所有服务和程序正确关闭，这在某些情况下可能导致数据丢失或不完整的服务终止。
- **`systemctl reboot`**：通过 Systemd 服务管理器重启系统，会尝试优雅地停止所有服务和组件，减少数据丢失风险，更适合于生产环境。

在 Systemd 主导的系统中，推荐使用 `systemctl reboot`，因为它能够更好地利用 Systemd 的服务管理能力，确保系统重启过程中的稳定性和安全性。然而，在某些紧急情况下，或者在不需要考虑服务终止细节的场景下，直接使用 `reboot` 也是可行的。

### 关机
```bash
systemctl poweroff
```

### 显示系统当前运行的目标
```bash
systemctl
```

### 显示系统默认目标
```bash
systemctl get-default
```

### 设置系统默认目标
```bash
systemctl set-default <target>
```

例如，设置系统默认目标为多用户模式：

```bash
systemctl set-default multi-user.target
```



## journalctl
### 查看所有日志
```bash
journalctl
```

### 查看特定服务的日志
```bash
journalctl -u <service-name>.service
```

例如，查看 SSH 服务的日志：

```bash
journalctl -u sshd.service
```

### 查看日志的最后几行
```bash
journalctl -n <number-of-lines>
```

例如，查看最后 10 行日志：

```bash
journalctl -n 10
```

### 查看今天的日志
```bash
journalctl --today
```

### 查看昨天的日志
```bash
journalctl --yesterday
```

### 查看指定时间范围内的日志
```bash
journalctl --since "<date-time>" --until "<date-time>"
```

例如，查看从今天上午 8 点到下午 2 点的日志：

```bash
journalctl --since "today 08:00" --until "today 14:00"
```

### 按时间逆序显示日志
```bash
journalctl -r
```

### 过滤日志级别
```bash
journalctl -p <priority>
```

例如，只显示错误级别的日志：

```bash
journalctl -p err
```

### 搜索日志中的关键字
```bash
journalctl _PID=<process-id> | grep <keyword>
```

例如，搜索 PID 为 1234 的进程日志中的 "error" 关键字：

```bash
journalctl _PID=1234 | grep error
```

### 查看实时日志流
```bash
journalctl -f
```

### 保存日志到文件
```bash
journalctl > log-file.txt
```

例如，将所有日志保存到 `system-log.txt` 文件中：

```bash
journalctl > system-log.txt
```


## opkg

```Bash
root@am62xx:/# opkg --help
opkg: unrecognized option '--help'
opkg must have one sub-command argument
usage: opkg [options...] sub-command [arguments...]
where sub-command is one of:

Package Manipulation:
        update                          Update list of available packages
        upgrade                         Upgrade installed packages
        install <pkgs>                  Install package(s)
        configure <pkgs>                Configure unpacked package(s)
        remove <pkgs|glob>              Remove package(s)
        clean                           Clean internal cache
        flag <flag> <pkgs>              Flag package(s)
         <flag>=hold|noprune|user|ok|installed|unpacked (one per invocation)

Informational Commands:
        list                            List available packages
        list-installed                  List installed packages
        list-upgradable                 List installed and upgradable packages
        list-changed-conffiles          List user modified configuration files
        files <pkg>                     List files belonging to <pkg>
        search <file|glob>              List package providing <file>
        find <regexp>                   List packages with names or description matching <regexp>
        info [pkg|glob]                 Display all info for <pkg>
        status [pkg|glob]               Display all status for <pkg>
        download <pkg>                  Download <pkg> to current directory
        compare-versions <v1> <op> <v2>
                                        compare versions using <= < > >= = << >>
        print-architecture              List installable package architectures
        depends [-A] [pkgname|glob]+
        whatdepends [-A] [pkgname|glob]+
        whatdependsrec [-A] [pkgname|glob]+
        whatrecommends[-A] [pkgname|glob]+
        whatsuggests[-A] [pkgname|glob]+
        whatprovides [-A] [pkgname|glob]+
        whatconflicts [-A] [pkgname|glob]+
        whatreplaces [-A] [pkgname|glob]+
        verify [pkg|glob]               Verifies the intrgrity of <pkg>, or all packages if omitted by
                                        comparing the md5sum of each file with the information stored
                                        on the opkg metadata database
Options:
        -A                              Query all packages not just those installed
        -V[<level>]                     Set verbosity level to <level>.
        --verbosity[=<level>]           Verbosity levels:
                                          0 errors only
                                          1 normal messages (default)
                                          2 informative messages
                                          3 debug
                                          4 debug level 2
        -f <conf_file>                  Use <conf_file> as the opkg configuration file
        --conf <conf_file>
        --cache-dir <path>              Specify cache directory.
        -t, --tmp-dir <directory>       Specify tmp-dir.
        -l, --lists-dir <directory>     Specify lists-dir.
        -d <dest_name>                  Use <dest_name> as the the root directory for
        --dest <dest_name>              package installation, removal, upgrading.
                                        <dest_name> should be a defined dest name from
                                        the configuration file, (but can also be a
                                        directory name in a pinch).
        -o <dir>                        Use <dir> as the root directory for
        --offline-root <dir>            offline installation of packages.
        --add-dest <name>:<path>        Register destination with given path
        --add-arch <arch>:<prio>        Register architecture with given priority
        --add-exclude <name>            Register package to be excluded from install
        --add-ignore-recommends <name>  Register package to be ignored as a recomendee
        --prefer-arch-to-version        Use the architecture priority package rather
                                        than the higher version one if more
                                        than one candidate is found.
        --combine                       Combine upgrade and install operations, this
                                        may be needed to resolve dependency issues.
                                        Only available for the internal solver backend.
        --fields <field1>,<field2>      Limit display information to the specified fields
                                        plus the package name. Valid for info and status.
        --short-description             Display only the first line of the description.
        --size                          Print package size when listing available packages

Force Options:
        --force-depends                 Install/remove despite failed dependencies
        --force-maintainer              Overwrite preexisting config files
        --force-reinstall               Reinstall package(s)
        --force-overwrite               Overwrite files from other package(s)
        --force-downgrade               Allow opkg to downgrade packages
        --force-space                   Disable free space checks
        --force-postinstall             Run postinstall scripts even in offline mode
        --force-remove                  Remove package even if prerm script fails
        --force-checksum                Don't fail on checksum mismatches
        --noaction                      No action -- test only
        --download-only                 No action -- download only
        --nodeps                        Do not follow dependencies
        --no-install-recommends         Do not install any recommended packages
        --force-removal-of-dependent-packages
                                        Remove package and all dependencies
        --autoremove                    Remove packages that were installed
                                        automatically to satisfy dependencies
        --host-cache-dir                Don't place cache in offline root dir.
        --volatile-cache                Use volatile cache.
                                        Volatile cache will be cleared on exit

 glob could be something like 'pkgname*' '*file*' or similar
 e.g. opkg info 'libstd*' or opkg search '*libop*' or opkg remove 'libncur*'
```

###  包管理命令

#### 更新软件包列表
```bash
opkg update
```

#### 升级已安装的软件包
```bash
opkg upgrade
```

#### 安装软件包
```bash
opkg install <pkgs>
```

####  配置已解压的软件包
```bash
opkg configure <pkgs>
```

#### 移除软件包
```bash
opkg remove <pkgs|glob>
```

####  清理内部缓存
```bash
opkg clean
```

###  信息命令

####  列出可用的软件包
```bash
opkg list
```

#### 列出已安装的软件包
```bash
opkg list-installed
```

#### 列出可升级的软件包
```bash
opkg list-upgradable
```

#### 列出属于某个软件包的文件
```bash
opkg files <pkg>
```

#### 搜索提供特定文件的软件包
```bash
opkg search <file|glob>
```

#### 显示软件包信息
```bash
opkg info [pkg|glob]
```

#### 显示软件包状态
```bash
opkg status [pkg|glob]
```

###  选项

#### 设置配置文件
```bash
opkg -f <conf_file>
```

#### 指定缓存目录
```bash
opkg --cache-dir <path>
```

#### 指定临时目录
```bash
opkg --tmp-dir <directory>
```

#### 设置日志级别
```bash
opkg -V<level>
```

#### 不执行任何操作（测试模式）
```bash
opkg --noaction
```

#### 仅下载软件包
```bash
opkg --download-only
```

#### 忽略依赖关系
```bash
opkg --nodeps
```

###  强制选项

#### 强制安装/移除
```bash
opkg --force-depends
opkg --force-remove
```

#### 允许降级
```bash
opkg --force-downgrade
```

#### 跳过文件校验
```bash
opkg --force-checksum
```

# 应用
##  查看系统的所有安装包

```Bash
#!/bin/sh

# 检查是否存在必要的命令
command -v opkg >/dev/null 2>&1 || { echo "opkg不可用。" >&2; exit 1; }

# 使用opkg列出安装的软件包及其大小，并排序，然后输出到packages.txt
opkg list-installed --size | sort -k 5 -rn > packages.txt

# 根据packages.txt文件，使用awk累加第5列（假设第5列包含大小），并输换为MB，最后打印总大小
total_size=$(awk '{sum += $5} END {print sum / 1024 / 1024}' packages.txt)

# 打印总大小，保留两位小数
echo "所有软件包的总大小为: $(printf "%.2f" $total_size) MB" >> packages.txt
```

```Bash
#!/bin/sh

# 检查是否存在必要的命令
command -v opkg >/dev/null 2>&1 || { echo "opkg不可用。" >&2; exit 1; }

# 使用opkg列出安装的软件包及其大小，并使用awk进行筛选和排序(只筛选大于1MB的软件包)
opkg list-installed --size | awk '$5 > (1024*1024)' | sort -k 5 -rn > packages_1MB.txt

# 根据packages.txt文件，使用awk累加第5列（假设第5列包含大小），并输换为MB，最后打印总大小
total_size=$(awk '{sum += $5} END {print sum / 1024 / 1024}' packages_1MB.txt)

# 打印总大小，保留两位小数
echo "所有软件包的总大小为: $(printf "%.2f" $total_size) MB" >> packages_1MB.txt
```