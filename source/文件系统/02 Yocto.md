# 命令集
## systemctl


## journalctl


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