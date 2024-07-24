
# 启动顺序

````Bash
在SysVinit体系下，系统启动和初始化过程遵循一个有序的步骤，主要通过在`/etc/inittab`文件中定义的脚本和进程控制。这些步骤大致包括以下阶段：

### SysVinit 启动顺序：

1. **引导加载器**：如GRUB，负责加载内核。
2. **内核初始化**：内核被加载后进行自身的初始化，包括检测硬件和加载驱动等。
3. **init进程**：内核完成初始化后，首先启动PID为1的进程`init`。`init`是所有其他进程的祖先，根据`/etc/inittab`文件配置进行系统初始化。
4. **运行级别**：尽管BusyBox init不支持运行级别（runlevels），但传统SysVinit通过`/etc/inittab`设定默认运行级别，根据运行级别执行不同的脚本和服务。Linux系统通常有7个运行级别，从0到6，每个有各自的目的，例如级别0用于关机，6用于重启。
5. **系统和服务初始化**：
   - **sysinit**：系统初始化阶段，进行必要的文件系统挂载和基础配置。
   - **rc脚本**：随后，init会根据运行级别执行对应`/etc/rc*.d/`或`/etc/init.d/`目录下的启动脚本（脚本命名有S和K前缀，分别代表启动和停止服务）。

根据您提供的`/etc/inittab`内容，自动登录被设置在`/etc/inittab`中通过下面这行实现：

```bash
ttyS2::respawn:/sbin/getty -n -l /sbin/autologinroot -L ttyS2 0 vt100 # GENERIC_SERIAL
```

这意味着在TTY设备`ttyS2`上启用了自动登录功能，登录的用户是root。

### 第一个执行的脚本：

在自动登录完成后，将会执行用户的shell。如果这里的root使用的是bash（或被设置为默认shell），则`/etc/profile`文件将会是login shell进程的第一个执行的系统级脚本。`/etc/profile`通常用于设置全局环境变量和运行`/etc/profile.d/`下的所有适用于shell的脚本。

从您提供的`/etc/profile`内容来看，它设置了基本的`PATH`环境变量，定义了提示符`PS1`，并指定了默认的编辑器。最后，它遍历`/etc/profile.d/`目录下的所有`.sh`文件并执行它们——这些通常用于进一步的环境设置。

### 结论：

在通过`/etc/inittab`文件中的自动登录设置自动登录后，第一个执行的脚本（假设使用bash作为shell）通常是`/etc/profile`。之后，shell将继续执行用户的个人初始化文件，如`.bash_profile`，`.bash_login`或`.profile`（具体取决于用户的shell和配置）。
````

## /etc/inittab

````Bash
上面的启动脚本是 BusyBox 映像的 `/etc/inittab` 文件，它定义系统启动和关闭时需要执行的一些关键任务。这个文件对初始化和管理系统运行时的动作至关重要。以下是对这个脚本的详解：

### 文件顶部解释与说明
```plaintext
# /etc/inittab
#
# Copyright (C) 2001 Erik Andersen <andersen@codepoet.org>
#
# Note: BusyBox init doesn't support runlevels.  The runlevels field is
# completely ignored by BusyBox init. If you want runlevels, use
# sysvinit.
#
# Format for each entry: <id>:<runlevels>:<action>:<process>
#
# id        == tty to run on, or empty for /dev/console
# runlevels == ignored
# action    == one of sysinit, respawn, askfirst, wait, and once
# process   == program to run
```
这部分解释了文件用途和格式：
- `id` 是要运行的 tty 或者留空用于 `/dev/console`
- `runlevels` 在 BusyBox Init 中被忽略
- `action` 可以是 `sysinit`, `respawn`, `askfirst`, `wait`, 以及 `once` 之一
- `process` 是要运行的程序

### 系统启动
```plaintext
# Startup the system
::sysinit:/bin/mount -t proc proc /proc
::sysinit:/bin/mount -o remount,rw /
::sysinit:/bin/mkdir -p /dev/pts /dev/shm
::sysinit:/bin/mount -a
::sysinit:/bin/mkdir -p /run/lock/subsys
::sysinit:/sbin/swapon -a
null::sysinit:/bin/ln -sf /proc/self/fd /dev/fd
null::sysinit:/bin/ln -sf /proc/self/fd/0 /dev/stdin
null::sysinit:/bin/ln -sf /proc/self/fd/1 /dev/stdout
null::sysinit:/bin/ln -sf /proc/self/fd/2 /dev/stderr
::sysinit:/bin/hostname -F /etc/hostname
# now run any rc scripts
::sysinit:/etc/init.d/rcS
```
这些条目设置了系统启动时需要执行的操作，它们的 `action` 都是 `sysinit`：
1. `::sysinit:/bin/mount -t proc proc /proc`: 挂载 `proc` 文件系统
2. `::sysinit:/bin/mount -o remount,rw /`: 重新挂载根文件系统为读写模式
3. `::sysinit:/bin/mkdir -p /dev/pts /dev/shm`: 创建目录 `/dev/pts` 和 `/dev/shm`
4. `::sysinit:/bin/mount -a`: 挂载所有文件系统
5. `::sysinit:/bin/mkdir -p /run/lock/subsys`: 创建运行时锁目录
6. `::sysinit:/sbin/swapon -a`: 启用所有交换分区
7. `null::sysinit:/bin/ln -sf /proc/self/fd /dev/fd`: 创建到 `/proc/self/fd` 的符号链接
8. `null::sysinit:/bin/ln -sf /proc/self/fd/0 /dev/stdin`: 创建到 `/proc/self/fd/0` 的符号链接
9. `null::sysinit:/bin/ln -sf /proc/self/fd/1 /dev/stdout`: 创建到 `/proc/self/fd/1` 的符号链接
10. `null::sysinit:/bin/ln -sf /proc/self/fd/2 /dev/stderr`: 创建到 `/proc/self/fd/2` 的符号链接
11. `::sysinit:/bin/hostname -F /etc/hostname`: 设置主机名
12. `::sysinit:/etc/init.d/rcS`: 运行所有 rc 脚本

### 串行终端（serial port）设置
```plaintext
# Put a getty on the serial port
ttyS2::respawn:/sbin/getty -L ttyS2 0 vt100 # GENERIC_SERIAL
```
这一行将 `getty` 放在串行端口 `ttyS2` 上运行，`getty` 用于控制终端登录：
- `ttyS2` 是串行端口
- `respawn` 表明 `getty` 应该在进程结束后重新启动
- `/sbin/getty -L ttyS2 0 vt100` 是运行 `getty` 的命令，它指定 `ttyS2` 作为终端设备，波特率为 `0`（自动探测），终端类型是 `vt100`

### 三指敬礼（Ctrl-Alt-Del）处理
```plaintext
# Stuff to do for the 3-finger salute
#::ctrlaltdel:/sbin/reboot
```
这一行被注释掉了，原本用于在按下 Ctrl-Alt-Del 组合键时重启系统。

### 系统关闭
```plaintext
# Stuff to do before rebooting
::shutdown:/etc/init.d/rcK
::shutdown:/sbin/swapoff -a
::shutdown:/bin/umount -a -r
```
这些条目设置了系统关闭时需要执行的操作，它们的 `action` 都是 `shutdown`：
1. `::shutdown:/etc/init.d/rcK`: 运行关闭 rc 脚本
2. `::shutdown:/sbin/swapoff -a`: 禁用所有交换分区
3. `::shutdown:/bin/umount -a -r`: 卸载所有文件系统（只读方式）

这些设置确保系统在启动和关闭时正确地执行重要任务，如挂载文件系统、设置主机名、运行必要的初始化脚本、以及在需要时提供终端登录服务。
````

## /etc/rcS

````Bash
这是一个用 `sh` 编写的启动脚本，常用于操作系统启动（通常是嵌入式系统）时执行 `/etc/init.d` 目录下的所有启动脚本。以下是详解：

### 脚本的功能
此脚本的主要功能是遍历 `/etc/init.d/` 目录中的所有脚本，并按照文件名的排序顺序依次执行它们。这些脚本通常用于启动系统服务或者执行初始化任务。

### 脚本内容详解
```sh
#!/bin/sh
```
这行表示脚本使用 `/bin/sh` 解释执行，通常是 Bourne Shell 或兼容的 Shell。

```sh
# Start all init scripts in /etc/init.d
# executing them in numerical order.
#
for i in /etc/init.d/S??* ;do
```
这段代码是一个 `for` 循环，用于遍历 `/etc/init.d/` 中所有以 `S` 开头并且后面跟有两个字符的文件（如 `S01script`、`S99script`等）。这些文件名通常按数字顺序排列，以确保按预定义顺序依次执行。

```sh
     # Ignore dangling symlinks (if any).
     [ ! -f "$i" ] && continue
```
这个条件判断语句检查当前遍历的文件是否是一个实际文件，如果不是（例如是一个悬空链接），则跳过（使用 `continue` 跳过本次循环的剩余部分，直接开始下一次循环）。

```sh
     case "$i" in
        *.sh)
            # Source shell script for speed.
            (
                trap - INT QUIT TSTP
                set start
                . $i
            )
            ;;
        *)
            # No sh extension, so fork subprocess.
            $i start
            ;;
    esac
done
```

这段代码使用 `case` 语句，根据文件扩展名不同进行处理：

1. **有 `.sh` 扩展名的文件**：
   ```sh
   *.sh)
       # Source shell script for speed.
       (
           trap - INT QUIT TSTP
           set start
           . $i
       )
       ;;
   ```
   - **`*.sh`**：匹配所有以 `.sh` 结尾的脚本。
   - **`trap - INT QUIT TSTP`**：在括号子 shell 中禁用常见信号的处理，如 `INT` (中断)，`QUIT` (退出)，和 `TSTP` (挂起)。
   - **`set start`**：传递一个 `start` 参数到脚本环境。尽管这个参数没有立即使用，但它可能对被执行的脚本有用。
   - **`. $i`**：使用 `.` (点) 命令来执行脚本，该命令在当前 shell 环境中执行脚本，而不是在子 shell 中。这种方式比创建一个新的子进程更有效率。

2. **无 `.sh` 扩展名的文件**：
   ```sh
   *)
       # No sh extension, so fork subprocess.
       $i start
       ;;
   ```
   - **`*)`**：匹配所有其他文件类型。
   - **`$i start`**：直接在子进程中执行脚本文件，并传递 `start` 参数。文件通常是可执行的二进制文件或没有 `.sh` 扩展名的脚本。

### 总结
此脚本确保 `/etc/init.d/` 目录中的所有启动脚本按照字母排序依次执行，无论它们是 `.sh` Shell 脚本还是其他可执行文件。通过这种方式，可以在系统启动时执行一系列初始化任务，从而确保系统服务正确启动并运行。
````

### 悬空链接

````markdown
悬空链接（Dangling Symlink），即无效符号链接，是一种符号链接（Symlink），其指向的目标文件或目录不存在。符号链接是一种文件类型，它包含对另一个文件或目录的引用，而不是数据内容。

### 符号链接简介
符号链接（Symlink），也称为软链接，是一种特殊类型的文件，它包含一个路径名，这个路径名指向另外一个文件或目录。与硬链接不同，硬链接直接指向文件的物理数据，而符号链接指向的是目标文件的路径。

创建符号链接的示例：
```sh
ln -s /path/to/target /path/to/symlink
```

在这里，`/path/to/symlink` 是符号链接，它指向 `/path/to/target`。可以将符号链接视为指向目标文件路径的快捷方式。

### 悬空链接详解
当符号链接指向的目标文件或目录被删除或移动，导致符号链接指向的路径无效时，这个符号链接就成为了悬空链接。

#### 创建悬空链接示例
1. **创建目标文件和符号链接**：
   ```sh
   echo "This is a target file" > /path/to/target
   ln -s /path/to/target /path/to/symlink
   ```
   现在我们有一个名为 `/path/to/symlink` 的符号链接指向目标文件 `/path/to/target`。

2. **删除目标文件**：
   ```sh
   rm /path/to/target
   ```
   现在 `/path/to/symlink` 成为了一个悬空链接，因为它指向的 `/path/to/target` 已不存在。

#### 检测悬空链接
可以使用 `file` 命令来检测符号链接是否悬空：
```sh
file /path/to/symlink
```
如果符号链接悬空，输出通常如下：
```plaintext
/path/to/symlink: broken symbolic link to `/path/to/target`
```

也可以使用 `find` 命令查找目录中的所有悬空符号链接：
```sh
find /path/to/search -xtype l
```

#### 移除悬空链接
可以使用 `rm` 命令移除悬空链接：
```sh
rm /path/to/symlink
```

### 悬空链接的影响
悬空链接可能导致各种问题，例如：
- 一些脚本或程序试图访问符号链接指向的目标文件时会失败，从而引发错误。
- 文件系统混乱，难以管理和维护。

### 实际应用示例
回到你提供的启动脚本，它在执行 `/etc/init.d/` 中脚本时跳过了悬空链接：
```sh
# Ignore dangling symlinks (if any).
[ ! -f "$i" ] && continue
```
这行代码检查当前遍历到的文件是否存在，如果文件不存在（即悬空链接），则使用 `continue` 跳过，继续处理下一个文件。

### 总结
悬空链接是那些指向不存在目标的符号链接，可能导致系统或脚本的运行错误。检测和处理悬空链接是维护文件系统健康和确保脚本正常运行的重要步骤。通过定期检查和清理悬空链接，可以避免潜在的问题和错误。
````

### 自动登录

````Bash
要实现 root 用户的自动登录，你可以修改 `/etc/inittab` 文件，添加或修改一行启动 `getty` 的配置以实现自动登录。以下是具体步骤：

### 修改 `/etc/inittab` 实现 root 自动登录

1. **备份 `inittab` 文件：**
   在进行任何修改之前，最好先备份当前的 `inittab` 文件：
   ```sh
   cp /etc/inittab /etc/inittab.bak
   ```

2. **编辑 `/etc/inittab` 文件：**
   使用文本编辑器打开 `/etc/inittab` 文件：
   ```sh
   vi /etc/inittab
   ```

3. **添加或修改 `getty` 配置行：**
   找到类似以下的行（针对 ttyS2 终端）：
   ```sh
   ttyS2::respawn:/sbin/getty -L ttyS2 0 vt100 # GENERIC_SERIAL
   ```

   修改为：
   ```sh
   ttyS2::respawn:/sbin/getty -n -l /sbin/autologinroot -L ttyS2 0 vt100 # GENERIC_SERIAL
   ```

4. **创建 `/sbin/autologinroot` 文件：**
   然后，创建一个新的脚本 `/sbin/autologinroot` 用于自动登录 root 用户：
   ```sh
   vi /sbin/autologinroot
   ```

   在文件中添加以下内容：
   ```sh
   #!/bin/sh
   exec /bin/login -f root
   ```

5. **赋予执行权限：**
   确保新的脚本具有执行权限：
   ```sh
   chmod +x /sbin/autologinroot
   ```

### 完整的 `/etc/inittab` 示例
这是 `/etc/inittab` 完整示例文件，已经加入 root 自动登录的配置：
```plaintext
# /etc/inittab
#
# Copyright (C) 2001 Erik Andersen <andersen@codepoet.org>
#
# Note: BusyBox init doesn't support runlevels.  The runlevels field is
# completely ignored by BusyBox init. If you want runlevels, use
# sysvinit.
#
# Format for each entry: <id>:<runlevels>:<action>:<process>
#
# id        == tty to run on, or empty for /dev/console
# runlevels == ignored
# action    == one of sysinit, respawn, askfirst, wait, and once
# process   == program to run

# Startup the system
::sysinit:/bin/mount -t proc proc /proc
::sysinit:/bin/mount -o remount,rw /
::sysinit:/bin/mkdir -p /dev/pts /dev/shm
::sysinit:/bin/mount -a
::sysinit:/bin/mkdir -p /run/lock/subsys
::sysinit:/sbin/swapon -a
null::sysinit:/bin/ln -sf /proc/self/fd /dev/fd
null::sysinit:/bin/ln -sf /proc/self/fd/0 /dev/stdin
null::sysinit:/bin/ln -sf /proc/self/fd/1 /dev/stdout
null::sysinit:/bin/ln -sf /proc/self/fd/2 /dev/stderr
::sysinit:/bin/hostname -F /etc/hostname
# now run any rc scripts
::sysinit:/etc/init.d/rcS

# Put a getty on the serial port
ttyS2::respawn:/sbin/getty -n -l /sbin/autologinroot -L ttyS2 0 vt100 # GENERIC_SERIAL

# Stuff to do for the 3-finger salute
#::ctrlaltdel:/sbin/reboot

# Stuff to do before rebooting
::shutdown:/etc/init.d/rcK
::shutdown:/sbin/swapoff -a
::shutdown:/bin/umount -a -r
```

### 解释
- **`-n`**: 表示不等待输入。
- **`-l /sbin/autologinroot`**: 指定一个自定义的登录程序。

新的脚本 `/sbin/autologinroot` 将自动执行 `login -f root` 命令，`-f` 选项表示不进行密码验证，直接登录指定用户。

### 注意事项
1. **安全性**：自动登录 root 用户会带来较大的安全风险，尤其是在网络连接的设备上。这种设置一般仅用于开发和受控的测试环境，不建议在生产环境中使用。
2. **权限管理**：确保 `/sbin/autologinroot` 脚本的权限设置正确，不允许普通用户修改。

通过这些步骤，你就能实现 root 用户的自动登录，方便在需要时快速进入系统进行操作。
````

  

## /etc/rcK

````Bash
这个脚本用于停止 `/etc/init.d` 目录中的所有初始化脚本，并且按照数字顺序的反序（从大到小）执行它们。这个顺序是为了确保服务在适当的顺序下关闭。以下是对这个脚本的详解：

### 脚本的功能
此脚本主要用于停止不同系统服务或执行关闭操作的初始化脚本。按照数字顺序的反序进行停止操作，确保系统服务的正常关闭顺序，例如先关闭依赖服务再关闭主服务。

### 脚本内容详解
```sh
#!/bin/sh
```
这行表示脚本使用 `sh` 来解释执行，通常是 Bourne Shell 或兼容的 Shell。

```sh
# Stop all init scripts in /etc/init.d
# executing them in reversed numerical order.
#
for i in $(ls -r /etc/init.d/S??*) ;do
```
- **注释说明**：这段代码是一个 `for` 循环，用于遍历 `/etc/init.d/` 中所有以 `S` 开头并且后面跟有两个字符的文件（如 `S01script`、`S99script`等），并按数字顺序反序排列，确保从最大的数字开始。
- **`ls -r /etc/init.d/S??*`**：列出所有匹配 `/etc/init.d/S??*` 模式的文件，并按反序（`-r` 选项）排列。
- **`for i in $(ls -r /etc/init.d/S??*)`**：对列出的文件逐一处理。

```sh
     # Ignore dangling symlinks (if any).
     [ ! -f "$i" ] && continue
```
这个条件判断语句用于检查当前遍历到的文件是否是一个实际文件（即目标文件存在），如果不是（例如是悬空链接），则跳过该文件（使用 `continue` 跳过本次循环的剩余部分，直接开始下一次循环）。

```sh
     case "$i" in
        *.sh)
            # Source shell script for speed.
            (
                trap - INT QUIT TSTP
                set stop
                . $i
            )
            ;;
        *)
            # No sh extension, so fork subprocess.
            $i stop
            ;;
    esac
done
```
这段代码使用 `case` 语句，根据文件扩展名的不同进行处理：

1. **有 `.sh` 扩展名的文件**：
   ```sh
   *.sh)
       # Source shell script for speed.
       (
           trap - INT QUIT TSTP
           set stop
           . $i
       )
       ;;
   ```
   - **`*.sh`**：匹配所有以 `.sh` 结尾的脚本。
   - **`trap - INT QUIT TSTP`**：在括号子 shell 中禁用常见信号的处理，如 `INT` (中断)，`QUIT` (退出)，和 `TSTP` (挂起)。
   - **`set stop`**：传递一个 `stop` 参数到脚本环境。
   - **`. $i`**：使用 `.` (点) 命令来执行脚本，该命令在当前 shell 环境中执行脚本，而不是在子 shell 中执行。这样做比创建一个新的子进程更高效。

2. **无 `.sh` 扩展名的文件**：
   ```sh
   *)
       # No sh extension, so fork subprocess.
       $i stop
       ;;
   ```
   - **`*)`**：匹配所有其他文件类型。
   - **`$i stop`**：直接在子进程中执行脚本文件，并传递 `stop` 参数。这些文件通常是可执行的二进制文件或没有 `.sh` 扩展名的脚本。

### 总结
此脚本在系统关闭或重启时，用于按反序顺序停止 `/etc/init.d/` 目录下的所有脚本。通过这种方式，它确保了依赖关系正确的关闭顺序，例如先关闭依赖服务再关闭主服务。通过处理悬空链接，避免了潜在的错误或异常情况，同时通过直接执行带 `.sh` 扩展名的脚本来提高效率。
````

## /etc/profile

````Bash
这个是 `/etc/profile` 文件的内容，它在用户登录时被执行，用于设置环境变量和执行其他初始化任务。以下是对此文件内容的详细解释：

### 内容详解
```sh
export PATH="/bin:/sbin:/usr/bin:/usr/sbin"
```
这行代码设置 `PATH` 环境变量，它指定了一系列目录，在这些目录下查找可执行文件。它确保了系统的标准实用程序可以在各种 shell 会话中被找到和运行。

### PS1 设置
```sh
if [ "$PS1" ]; then
        if [ "`id -u`" -eq 0 ]; then
                export PS1='# '
        else
                export PS1='$ '
        fi
fi
```
这个条件块是用于设置 shell 提示符（`PS1`），具体如下：
- `if [ "$PS1" ]; then`：检查 `PS1` 是否已被设置。这个变量通常在交互式 shell 中被设置，表示当前 shell 是交互式的。
- `if [ "\`id -u\`" -eq 0 ]; then`：检查当前用户是否是 root 用户。如果是 root 用户（`id -u` 返回 0），则 `PS1` 被设置为 `# `。
- 否则，如果不是 root 用户，`PS1` 被设置为 `$ `。

通过这段代码，root 用户的提示符将是 `# `，其他用户的提示符将是 `$ `。

### 编辑器设置
```sh
export EDITOR='/bin/vi'
```
这个选项设置默认的文本编辑器为 `vi`。当需要使用文本编辑器（例如，使用 `crontab -e` 或 `git commit`）时，系统会默认使用 `vi` 编辑器。

### `/etc/profile.d` 目录中的配置文件
```sh
# Source configuration files from /etc/profile.d
for i in /etc/profile.d/*.sh ; do
        if [ -r "$i" ]; then
                . $i
        fi
done
unset i
```
这段代码用于加载 `/etc/profile.d/` 目录下的所有 `.sh` 脚本：
- `for i in /etc/profile.d/*.sh ; do`：遍历 `/etc/profile.d/` 目录下所有以 `.sh` 结尾的文件。
- `if [ -r "$i" ]; then`：检查文件是否可读。
- `. $i`：通过 `.` (点) 命令来执行这个脚本文件。这将在当前 shell 环境中执行脚本文件中的命令。
- `unset i`：清除变量 `i`，以防止污染环境。

### 总结
这个 `/etc/profile` 文件的主要功能是设置环境变量、配置 shell 提示符以及加载额外的配置文件。

1. **环境变量设置**：
   - 设置了 `PATH` 确保常用命令可以被找到。
   - 设置了 `EDITOR` 变量确定默认的文本编辑器。

2. **提示符设置**：
   - 根据用户身份（root 或非 root）设置不同的 shell 提示符。

3. **加载额外配置**：
   - 通过加载 `/etc/profile.d/` 目录中的脚本，允许在该目录中添加额外的初始化脚本以定制化用户的环境。

这使得 `/etc/profile` 是一个非常便于管理和扩展的系统初始化文件，可以在系统登录时为用户提供一个一致且可配置的工作环境。
````

```Bash
在 shell 脚本中，`[ -r "$i" ]` 是一种检查条件表达式，用来判断文件或目录的权限。具体来说，`-r` 检查指定的路径 `$i` 是否存在且具有读取权限。

因此，在 `if [ -r "$i" ]; then` 这行代码中，它作用如下：

- 首先，检查变量 `$i` 中存储的路径是否存在；
- 然后，检查当前用户是否对该路径具有读取权限；
- 如果上述两个条件都满足，即该路径存在且当前用户有权限读取这个文件或目录，`if` 条件被认为是真（true），然后执行子句中的命令，即用 `.`（或等价的 `source` 命令）执行（或称为“source”）该路径下的脚本文件。

在这个特定的上下文中，这个检查确保只有当`.sh`脚本文件存在且当前用户有权限读取它时，才会尝试执行（source）`/etc/profile.d/`目录下的那些 shell 脚本文件。这是一种保护机制，避免尝试执行不存在或无权限访问的文件，从而可能导致错误或安全问题。
```

## /etc/profile.d

### umask.sh

```Bash
# cat /etc/profile.d/umask.sh 
umask 022
```

````Bash
`umask` 是一个用于设置文件创建权限掩码的命令。文件权限掩码定义了当用户创建文件或目录时，哪些默认权限将被禁用。更具体地说，它决定了新创建的文件和目录的权限位。

### 详细解释

首先，了解文件权限的意义：
- **读权限（r）**：数值为4，允许读取文件。
- **写权限（w）**：数值为2，允许写入文件。
- **执行权限（x）**：数值为1，允许执行文件或进入目录。

新的文件和目录的默认权限通常为：
- **文件**：666（表示所有用户都有读写权限，但没有执行权限，因为执行权限对于普通文件来说较为危险）
- **目录**：777（表示所有用户都有读、写、执行权限）

### `umask` 的作用
`umask` 值是从这些默认权限中减去的值。它定义了在文件或目录创建时需要禁用的权限。

### `umask 022` 的具体意义
- **umask 022**：表示在创建文件和目录时禁用"组"和"其他"用户的写权限。

#### 计算新文件的权限：
默认文件权限是666。
- `022` 意味着应禁用的权限是000 010 010（即组和其他的写权限）。
- 新文件的权限计算：666（默认权限）- 022（umask 值）= 644

因此，新创建的文件将有权限 `rw-r--r--`，即：
- 文件所有者有读写权限。
- 文件的组成员有读权限。
- 其他用户有读权限。

#### 计算新目录的权限：
默认目录权限是777。
- `022` 意味着应禁用的权限是000 010 010（即组和其他的写权限）。
- 新目录的权限计算：777（默认权限）- 022（umask 值）= 755

因此，新创建的目录将有权限 `rwxr-xr-x`，即：
- 目录所有者有读、写、执行权限。
- 目录的组成员有读和执行权限。
- 其他用户有读和执行权限。

### 举例说明

假设你有以下内容的 `umask.sh` 脚本：
```sh
umask 022
```
并将其放在 `/etc/profile.d/` 目录中。

#### 执行 `umask.sh` 脚本
当用户登录时，这个脚本会被自动执行，设置系统的 `umask` 值为022。此时，新创建的文件和目录的权限是受此 `umask` 值影响的。

### 实际操作

1. **新建一个文件**：
   ```sh
   touch newfile.txt
   ```
   执行 `ls -l newfile.txt` 得到：
   ```plaintext
   -rw-r--r-- 1 user group 0 date time newfile.txt
   ```
   这表明新文件的权限为 644。

2. **新建一个目录**：
   ```sh
   mkdir newdir
   ```
   执行 `ls -ld newdir` 得到：
   ```plaintext
   drwxr-xr-x 2 user group 4096 date time newdir
   ```
   这表明新目录的权限为 755。

### `umask` 的常见值

- **022**：禁用组和其他用户的写权限（最常用）。
- **027**：禁用组的写权限，以及其他用户的所有权限。
- **077**：禁用所有组和其他用户的所有权限（较为严格的设置）。

### 总结
`umask 022` 脚本在 `/etc/profile.d/` 目录中设置系统默认文件创建权限为 644，新建目录权限为 755。这是一种合理的设置，确保文件和目录具有适当的安全性，防止未经授权的写访问。每次用户登录时，系统会自动应用这些权限设置。
````

