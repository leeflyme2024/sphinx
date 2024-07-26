

# 命令集

## find 

### 查找所有 .txt 文件

```bash
find /path/to/search -name "*.txt"
```

例如，查找当前目录及其子目录下所有的 `.txt` 文件：

```bash
find . -name "*.txt"
```

### 查找最近修改的文件

```bash
find /path/to/search -mtime -n
```

例如，查找过去一天内被修改过的所有文件：

```bash
find . -mtime -1
```

这里的 `-mtime -1` 表示查找在过去 24 小时内被修改的文件。

### 查找特定大小的文件

```bash
find /path/to/search -size +nK
```

例如，查找大于 100KB 的所有文件：

```bash
find . -size +100K
```

### 查找特定类型的文件

```bash
find /path/to/search -type f|d|l|b|c
```

例如，查找所有的目录：

```bash
find . -type d
```

### 查找空文件

```bash
find /path/to/search -empty
```

例如，查找当前目录及其子目录下所有空的文件或目录：

```bash
find . -empty
```

### 查找具有特定权限的文件

```bash
find /path/to/search -perm -mode
```

例如，查找具有可执行权限的文件：

```bash
find . -perm -u+x
```

### 查找属于特定用户的文件

```bash
find /path/to/search -user username
```

例如，查找属于用户 `john` 的所有文件：

```bash
find . -user john
```

### 查找文件并执行命令

```bash
find /path/to/search -name "pattern" -exec command {} \;
```

例如，查找所有 `.txt` 文件并删除它们：

```bash
find . -name "*.txt" -exec rm {} \;
```

### 查找文件并使用 xargs 执行命令

```bash
find /path/to/search -name "pattern" -print0 | xargs -0 command
```

例如，查找所有 `.txt` 文件并压缩它们：

```bash
find . -name "*.txt" -print0 | xargs -0 zip -j archive.zip
```

## grep 

### 搜索文件中包含特定字符串的行

```bash
grep "pattern" filename
```

例如，搜索文件 `example.txt` 中包含单词 "Linux" 的行：

```bash
grep "Linux" example.txt
```

### 搜索多个文件中包含特定字符串的行

```bash
grep "pattern" file1 file2 ...
```

例如，搜索多个文件 `file1.txt`, `file2.txt` 中包含单词 "Linux" 的行：

```bash
grep "Linux" file1.txt file2.txt
```

### 搜索文件中不包含特定字符串的行

```bash
grep -v "pattern" filename
```

例如，搜索文件 `example.txt` 中不包含单词 "Linux" 的行：

```bash
grep -v "Linux" example.txt
```

### 忽略大小写搜索

```bash
grep -i "pattern" filename
```

例如，忽略大小写搜索文件 `example.txt` 中包含单词 "Linux" 的行：

```bash
grep -i "Linux" example.txt
```

### 搜索整个目录及其子目录中的文件

```bash
grep -r "pattern" directory/
```

例如，搜索目录 `docs/` 及其所有子目录中的文件，查找包含单词 "Linux" 的行：

```bash
grep -r "Linux" docs/
```

### 输出匹配的行的行号

```bash
grep -n "pattern" filename
```

例如，输出文件 `example.txt` 中包含单词 "Linux" 的行及其行号：

```bash
grep -n "Linux" example.txt
```

### 使用正则表达式搜索

```bash
grep "regex_pattern" filename
```

例如，搜索文件 `example.txt` 中所有以 "www." 开头的 URL：

```bash
grep "www\.[^ ]*" example.txt
```

### 将搜索结果输出到文件

```bash
grep "pattern" filename > output.txt
```

例如，将搜索文件 `example.txt` 中包含单词 "Linux" 的行的结果保存到 `output.txt`：

```bash
grep "Linux" example.txt > output.txt
```

### 统计匹配行的数量

```bash
grep -c "pattern" filename
```

例如，统计文件 `example.txt` 中包含单词 "Linux" 的行的数量：

```bash
grep -c "Linux" example.txt
```


## sed 

### 替换单词或短语

```bash
sed 's/pattern/replacement/g' filename
```

例如，将文件 `example.txt` 中所有的 "Linux" 替换为 "Ubuntu"：

```bash
sed 's/Linux/Ubuntu/g' example.txt
```

### 替换某一行的内容

```bash
sed 'n s/pattern/replacement/' filename
```

例如，将文件 `example.txt` 中的第 3 行的 "Linux" 替换为 "Ubuntu"：

```bash
sed '3 s/Linux/Ubuntu/' example.txt
```

### 替换所有行的开头

```bash
sed 's/^/prefix /' filename
```

例如，给文件 `example.txt` 中的每一行添加前缀 "Prefix: "：

```bash
sed 's/^/Prefix: /' example.txt
```

### 删除包含特定模式的行

```bash
sed '/pattern/d' filename
```

例如，删除文件 `example.txt` 中包含 "delete" 的所有行：

```bash
sed '/delete/d' example.txt
```

### 插入一行文本

```bash
sed 'n i\inserted line' filename
```

例如，在文件 `example.txt` 的第 3 行前插入一行 "Inserted line":

```bash
sed '3 i\Inserted line' example.txt
```

### 在文件末尾追加一行

```bash
sed '$ a\appended line' filename
```

例如，在文件 `example.txt` 的末尾追加一行 "Appended line":

```bash
sed '$ a\Appended line' example.txt
```

### 将结果重定向到新文件

```bash
sed 's/pattern/replacement/g' filename > newfile
```

例如，将文件 `example.txt` 中所有的 "Linux" 替换为 "Ubuntu"，并将结果保存到 `newfile.txt`：

```bash
sed 's/Linux/Ubuntu/g' example.txt > newfile.txt
```

### 使用正则表达式替换

```bash
sed 's/regex/pattern/' filename
```

例如，将文件 `example.txt` 中所有数字替换为 "Number":

```bash
sed 's/[0-9]*/Number/g' example.txt
```

### 替换并保存原始文件

```bash
sed -i 's/pattern/replacement/g' filename
```

例如，将文件 `example.txt` 中所有的 "Linux" 替换为 "Ubuntu"，并直接修改原文件：

```bash
sed -i 's/Linux/Ubuntu/g' example.txt
```

## awk

### 打印文件中的所有行

```bash
awk '{ print }' filename
```

例如，打印文件 `data.txt` 中的所有行：

```bash
awk '{ print }' data.txt
```

### 打印文件中的第二列

```bash
awk '{ print $2 }' filename
```

例如，打印文件 `data.txt` 中的第二列：

```bash
awk '{ print $2 }' data.txt
```

### 计算列的总和

```bash
awk '{ sum += $1 } END { print sum }' filename
```

例如，计算文件 `data.txt` 第一列的总和：

```bash
awk '{ sum += $1 } END { print sum }' data.txt
```

### 根据字段分组并计数

```bash
awk '{ count[$1]++ } END { for (i in count) print i, count[i] }' filename
```

例如，计算文件 `data.txt` 第一列中每个唯一值的出现次数：

```bash
awk '{ count[$1]++ } END { for (i in count) print i, count[i] }' data.txt
```

### 打印满足条件的行

```bash
awk '$1 > limit { print }' filename
```

例如，打印文件 `data.txt` 中第一列大于 10 的所有行：

```bash
awk '$1 > 10 { print }' data.txt
```

### 计算两列的平均值

```bash
awk '{ sum1 += $1; sum2 += $2 } END { print "Average1=" sum1/NR " Average2=" sum2/NR }' filename
```

例如，计算文件 `data.txt` 第一列和第二列的平均值：

```bash
awk '{ sum1 += $1; sum2 += $2 } END { print "Average1=" sum1/NR " Average2=" sum2/NR }' data.txt
```

### 格式化输出

```bash
awk '{ printf "%-10s %-10s\n", $1, $2 }' filename
```

例如，格式化打印文件 `data.txt` 的第一列和第二列：

```bash
awk '{ printf "%-10s %-10s\n", $1, $2 }' data.txt
```

### 使用自定义分隔符

```bash
awk -F':' '{ print $1 }' filename
```

例如，使用冒号作为分隔符，打印文件 `data.txt` 的第一列：

```bash
awk -F':' '{ print $1 }' data.txt
```

### 删除空行

```bash
awk 'NF' filename
```

例如，删除文件 `data.txt` 中的所有空行：

```bash
awk 'NF' data.txt
```

### 打印列的最大值

```bash
awk '{ if ($1 > max) max = $1 } END { print max }' filename
```

例如，打印文件 `data.txt` 第一列的最大值：

```bash
awk '{ if ($1 > max) max = $1 } END { print max }' data.txt
```



## cp

### 复制单个文件

```bash
cp source_file destination
```

例如，复制 `example.txt` 到 `backup.txt`：

```bash
cp example.txt backup.txt
```

### 复制文件到目录

```bash
cp source_file directory/
```

例如，将 `example.txt` 复制到目录 `myfolder`：

```bash
cp example.txt myfolder/
```

### 复制目录及其内容

```bash
cp -r source_directory destination_directory
```

例如，复制目录 `mydir` 到 `newdir`：

```bash
cp -r mydir newdir
```

### 复制多个文件到同一目录

```bash
cp file1 file2 ... directory/
```

例如，将 `file1.txt` 和 `file2.txt` 复制到 `myfolder`：

```bash
cp file1.txt file2.txt myfolder/
```

### 复制文件并保留所有属性

```bash
cp -p source_file destination
```

例如，复制 `example.txt` 到 `backup.txt` 并保留文件属性：

```bash
cp -p example.txt backup.txt
```

### 更新目标文件，如果源文件更新过

```bash
cp -u source_file destination
```

例如，只有当 `example.txt` 更新后，才复制到 `backup.txt`：

```bash
cp -u example.txt backup.txt
```

### 交互式复制，询问是否覆盖

```bash
cp -i source_file destination
```

例如，复制 `example.txt` 到 `existing.txt`，并询问是否覆盖：

```bash
cp -i example.txt existing.txt
```

### 复制文件并显示进度条

```bash
cp -v source_file destination
```

例如，复制 `example.txt` 到 `backup.txt` 并显示详细信息：

```bash
cp -v example.txt backup.txt
```

### 使用符号链接而不是复制文件

```bash
cp -s source_file link_name
```

例如，创建 `example.txt` 的符号链接 `link_example`：

```bash
cp -s example.txt link_example
```

### 复制文件并更改权限

```bash
cp --chmod=mode source_file destination
```

例如，复制 `example.txt` 到 `backup.txt` 并设置权限为 `644`：

```bash
cp --chmod=644 example.txt backup.txt
```


## rm

### 删除单个文件

```bash
rm filename
```

例如，删除名为 `example.txt` 的文件：

```bash
rm example.txt
```

### 强制删除文件（不询问确认）

```bash
rm -f filename
```

例如，强制删除名为 `example.txt` 的文件，不显示任何警告：

```bash
rm -f example.txt
```

### 删除多个文件

```bash
rm file1 file2 ...
```

例如，删除多个文件 `file1.txt`, `file2.txt`：

```bash
rm file1.txt file2.txt
```

### 删除目录及其内容

```bash
rm -r directory
```

例如，删除目录 `mydir` 及其所有内容：

```bash
rm -r mydir
```

### 强制删除目录及其内容

```bash
rm -rf directory
```

例如，强制删除目录 `mydir` 及其所有内容，不显示任何警告：

```bash
rm -rf mydir
```

### 删除符合模式的所有文件

```bash
rm pattern
```

例如，删除所有以 `.bak` 结尾的备份文件：

```bash
rm *.bak
```

### 删除隐藏文件

```bash
rm .filename
```

例如，删除隐藏文件 `.hiddenfile`：

```bash
rm .hiddenfile
```

### 删除所有隐藏文件

```bash
rm .*  # 注意使用通配符时，可能需要在前面加上引号避免shell提前展开
```

例如，删除当前目录下的所有隐藏文件：

```bash
rm .*
```

### 删除文件并要求确认

```bash
rm -i filename
```

例如，删除文件 `example.txt` 但先询问确认：

```bash
rm -i example.txt
```

### 删除文件前显示文件名

```bash
rm -v filename
```

例如，删除文件 `example.txt` 并显示操作信息：

```bash
rm -v example.txt
```


## dmesg 
```bash
bash-5.1# dmesg --help
BusyBox v1.35.0 (2024-07-20 11:38:35 CST) multi-call binary.

Usage: dmesg [-cr] [-n LEVEL] [-s SIZE]

Print or control the kernel ring buffer

        -c              Clear ring buffer after printing
        -n LEVEL        Set console logging level
        -s SIZE         Buffer size
        -r              Print raw message buffer
```

### 参数解析

#### 打印并清除内核环形缓冲区

```bash
dmesg -c
```

#### 设置控制台日志记录级别

```bash
dmesg -n 3
```

#### 设置缓冲区大小

```bash
dmesg -s 1024
```

#### 打印原始消息缓冲区

```bash
dmesg -r
```

### 综合应用

#### 打印内核消息并设置日志记录级别

```bash
dmesg -c -n 5
```

#### 打印原始消息并设置缓冲区大小

```bash
dmesg -r -s 2048
```


## chroot

`chroot` 命令允许你更改一个进程的根目录，即进程将看到的文件系统的根。这在容器技术、系统修复、软件包构建和安全沙箱环境中非常有用。

### 更改根目录并运行命令

```bash
chroot /path/to/new/root command args...
```

例如，更改根目录到 `/mnt/newroot` 并运行 `bash` shell：

```bash
chroot /mnt/newroot bash
```

一旦在 `chroot` 环境中，你的 `/` 将指向 `/mnt/newroot`，并且所有路径都将相对于这个新的根目录。

### 构建软件包

假设你有一个准备好的软件包构建环境在 `/mnt/buildenv`，你可以使用 `chroot` 来运行构建过程：

```bash
chroot /mnt/buildenv make package
```

这将让你在隔离的环境中构建软件包，不会影响主机系统的文件。

### 系统修复

如果需要修复一个受损的系统，你可以从一个救援环境（如 Live CD 或 Live USB）启动，并使用 `chroot` 进入受损系统的根目录进行修复：

```bash
mount /dev/sda1 /mnt/systemroot
chroot /mnt/systemroot /bin/bash
```

在上面的例子中，`/dev/sda1` 是受损系统的根分区，`/mnt/systemroot` 是挂载点。一旦运行 `chroot`，你将能够像在正常系统中一样使用受损系统的命令和工具。

### 检查新系统

在安装或升级系统之后，你可以使用 `chroot` 来检查新系统的环境是否正常工作，而无需完全重启计算机：

```bash
chroot /mnt/newsystem /bin/bash
```

这里，`/mnt/newsystem` 是新系统所在的目录。

### 创建沙箱环境

`chroot` 可以用来创建一个安全的沙箱环境，限制进程的访问权限：

```bash
chroot /mnt/sandbox /bin/bash
```

在这个例子中，`/mnt/sandbox` 是一个严格限制的环境，可以用来运行不受信任的应用程序。

## dosfstools

### mkfs.fat
```bash
bash-5.1# mkfs.fat --help
mkfs.fat 4.2 (2021-01-31)
Usage: /usr/sbin/mkfs.fat [OPTIONS] TARGET [BLOCKS]
Create FAT filesystem in TARGET, which can be a block device or file. Use only
up to BLOCKS 1024 byte blocks if specified. With the -C option, file TARGET will be
created with a size of 1024 bytes times BLOCKS, which must be specified.

Options:
  -a              Disable alignment of data structures
  -A              Toggle Atari variant of the filesystem
  -b SECTOR       Select SECTOR as location of the FAT32 backup boot sector
  -c              Check device for bad blocks before creating the filesystem
  -C              Create file TARGET then create filesystem in it
  -D NUMBER       Write BIOS drive number NUMBER to boot sector
  -f COUNT        Create COUNT file allocation tables
  -F SIZE         Select FAT size SIZE (12, 16 or 32)
  -g GEOM         Select disk geometry: heads/sectors_per_track
  -h NUMBER       Write hidden sectors NUMBER to boot sector
  -i VOLID        Set volume ID to VOLID (a 32 bit hexadecimal number)
  -I              Ignore and disable safety checks
  -l FILENAME     Read bad blocks list from FILENAME
  -m FILENAME     Replace default error message in boot block with contents of FILENAME
  -M TYPE         Set media type in boot sector to TYPE
  --mbr[=y|n|a]   Fill (fake) MBR table with one partition which spans whole disk
  -n LABEL        Set volume name to LABEL (up to 11 characters long)
  --codepage=N    use DOS codepage N to encode label (default: 850)
  -r COUNT        Make room for at least COUNT entries in the root directory
  -R COUNT        Set minimal number of reserved sectors to COUNT
  -s COUNT        Set number of sectors per cluster to COUNT
  -S SIZE         Select a sector size of SIZE (a power of two, at least 512)
  -v              Verbose execution
  --variant=TYPE  Select variant TYPE of filesystem (standard or Atari)

  --invariant     Use constants for randomly generated or time based values
  --offset=SECTOR Write the filesystem at a specific sector into the device file.
  --help          Show this help message and exit
```

#### 参数解析

##### 创建 FAT 文件系统

```bash
mkfs.fat -F 32 /dev/sdX
```
- `-F SIZE`: 选择 FAT 的大小，`SIZE` 可以是 12, 16 或 32。
- `/dev/sdX`: 设备文件路径，用于指定要创建文件系统的设备。

##### 在文件中创建 FAT 文件系统

```bash
mkfs.fat -C image.img 2048
```
- `-C`: 创建文件 `TARGET`，然后在其中创建文件系统。
- `image.img`: 文件名，将创建一个大小为 1024 * BLOCKS 的文件。
- `2048`: 文件大小，以 1024 字节块为单位。

##### 检查坏块并创建文件系统

```bash
mkfs.fat -c /dev/sdX
```
- `-c`: 在创建文件系统之前检查设备上的坏块。

##### 设置卷标和卷 ID

```bash
mkfs.fat -n MYVOLUME -i 12345678 /dev/sdX
```
- `-n LABEL`: 设置卷标，`LABEL` 为卷的名称，最长为 11 个字符。
- `-i VOLID`: 设置卷 ID，`VOLID` 为 32 位十六进制数字。

##### 设置每簇扇区数

```bash
mkfs.fat -s 8 /dev/sdX
```
- `-s COUNT`: 设置每个簇的扇区数为 `COUNT`。

##### 选择磁盘几何结构

```bash
mkfs.fat -g 255/63 /dev/sdX
```
- `-g GEOM`: 选择磁盘几何结构，`GEOM` 为磁头数/每轨道扇区数。

##### 设置备份引导扇区位置

```bash
mkfs.fat -b 16 /dev/sdX
```
- `-b SECTOR`: 选择 `SECTOR` 作为 FAT32 备份引导扇区的位置。

##### 设置媒体类型

```bash
mkfs.fat -M 0xF8 /dev/sdX
```
- `-M TYPE`: 设置引导扇区中的媒体类型为 `TYPE`。

##### 创建指定数量的文件分配表

```bash
mkfs.fat -f 2 /dev/sdX
```
- `-f COUNT`: 创建 `COUNT` 个文件分配表。

##### 设置根目录项和保留扇区数

```bash
mkfs.fat -r 512 -R 32 /dev/sdX
```
- `-r COUNT`: 为根目录至少保留 `COUNT` 个条目。
- `-R COUNT`: 设置保留扇区的最小数量为 `COUNT`。

##### 替换引导块的错误信息

```bash
mkfs.fat -m error.txt /dev/sdX
```
- `-m FILENAME`: 用 `FILENAME` 文件的内容替换引导块中的默认错误信息。

##### 填充 MBR 表

```bash
mkfs.fat --mbr=y /dev/sdX
```
- `--mbr[=y|n|a]`: 填充（伪造）MBR 表，以一个覆盖整个磁盘的分区。

##### 使用 DOS 代码页编码标签

```bash
mkfs.fat --codepage=437 /dev/sdX
```
- `--codepage=N`: 使用 DOS 代码页 `N` 对标签进行编码（默认值为 850）。

##### 设置文件系统在特定扇区位置

```bash
mkfs.fat --offset=2048 /dev/sdX
```
- `--offset=SECTOR`: 在设备文件的特定扇区位置写入文件系统。

#### 综合应用

##### 创建具有指定簇大小和 FAT 大小的 FAT 文件系统

```bash
mkfs.fat -s 4 -F 32 /dev/sdX
```
- `-s 4`: 每簇 4 个扇区。
- `-F 32`: 使用 FAT32 文件系统。
- `/dev/sdX`: 目标设备。

##### 在文件中创建 FAT 文件系统并设置卷标

```bash
mkfs.fat -C mydisk.img 4096 -n MYDISK
```
- `-C mydisk.img 4096`: 创建一个名为 `mydisk.img` 的文件，大小为 4096 * 1024 字节。
- `-n MYDISK`: 设置卷标为 `MYDISK`。

##### 检查坏块并创建 FAT 文件系统，设置引导扇区媒体类型

```bash
mkfs.fat -c -M 0xF8 /dev/sdX
```
- `-c`: 检查坏块。
- `-M 0xF8`: 设置引导扇区中的媒体类型为 `0xF8`。


### mkfs.vfat
```bash
bash-5.1# mkfs.vfat --help
mkfs.fat 4.2 (2021-01-31)
Usage: /usr/sbin/mkfs.vfat [OPTIONS] TARGET [BLOCKS]
Create FAT filesystem in TARGET, which can be a block device or file. Use only
up to BLOCKS 1024 byte blocks if specified. With the -C option, file TARGET will be
created with a size of 1024 bytes times BLOCKS, which must be specified.

Options:
  -a              Disable alignment of data structures
  -A              Toggle Atari variant of the filesystem
  -b SECTOR       Select SECTOR as location of the FAT32 backup boot sector
  -c              Check device for bad blocks before creating the filesystem
  -C              Create file TARGET then create filesystem in it
  -D NUMBER       Write BIOS drive number NUMBER to boot sector
  -f COUNT        Create COUNT file allocation tables
  -F SIZE         Select FAT size SIZE (12, 16 or 32)
  -g GEOM         Select disk geometry: heads/sectors_per_track
  -h NUMBER       Write hidden sectors NUMBER to boot sector
  -i VOLID        Set volume ID to VOLID (a 32 bit hexadecimal number)
  -I              Ignore and disable safety checks
  -l FILENAME     Read bad blocks list from FILENAME
  -m FILENAME     Replace default error message in boot block with contents of FILENAME
  -M TYPE         Set media type in boot sector to TYPE
  --mbr[=y|n|a]   Fill (fake) MBR table with one partition which spans whole disk
  -n LABEL        Set volume name to LABEL (up to 11 characters long)
  --codepage=N    use DOS codepage N to encode label (default: 850)
  -r COUNT        Make room for at least COUNT entries in the root directory
  -R COUNT        Set minimal number of reserved sectors to COUNT
  -s COUNT        Set number of sectors per cluster to COUNT
  -S SIZE         Select a sector size of SIZE (a power of two, at least 512)
  -v              Verbose execution
  --variant=TYPE  Select variant TYPE of filesystem (standard or Atari)

  --invariant     Use constants for randomly generated or time based values
  --offset=SECTOR Write the filesystem at a specific sector into the device file.
  --help          Show this help message and exit
```

#### 参数解析

##### 禁用数据结构对齐

```bash
mkfs.vfat -a -F 32 /dev/sdX
```

- `-a`：禁用数据结构对齐。
- `-F SIZE`：选择 FAT 文件系统的大小。`SIZE` 可以是 12、16 或 32。例如，`-F 32` 表示创建 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 切换到 Atari 变体的文件系统

```bash
mkfs.vfat -A -F 32 /dev/sdX
```

- `-A`：切换到 Atari 变体的文件系统。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 选择备份引导扇区位置

```bash
mkfs.vfat -b 100 -F 32 /dev/sdX
```

- `-b SECTOR`：选择 `SECTOR` 作为 FAT32 备份引导扇区的位置。例如，`100` 表示选择第 100 个扇区。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 检查设备坏块并创建文件系统

```bash
mkfs.vfat -c -F 32 /dev/sdX
```

- `-c`：检查设备是否有坏块。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建文件并在其上创建文件系统

```bash
mkfs.vfat -C myfile.img 2048
```

- `-C`：创建文件 `TARGET`，然后在其上创建文件系统。`BLOCKS` 是指定的块数。例如，`2048` 表示文件大小为 2048 个 1024 字节的块。

##### 设置引导扇区中的 BIOS 驱动器号

```bash
mkfs.vfat -D 0x80 -F 32 /dev/sdX
```

- `-D NUMBER`：在引导扇区中写入 BIOS 驱动器号 `NUMBER`。例如，`0x80`。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建指定数量的文件分配表

```bash
mkfs.vfat -f 2 -F 32 /dev/sdX
```

- `-f COUNT`：创建 `COUNT` 个文件分配表。例如，`2` 表示创建两个文件分配表。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 设置卷标

```bash
mkfs.vfat -n MYLABEL -F 32 /dev/sdX
```

- `-n LABEL`：设置卷标为 `LABEL`（最多 11 个字符）。例如，`MYLABEL`。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 设置媒体类型

```bash
mkfs.vfat -M 0xF8 -F 16 /dev/sdX
```

- `-M TYPE`：设置引导扇区中的媒体类型为 `TYPE`。例如，`0xF8` 表示硬盘。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 16` 表示 FAT16 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 忽略安全检查

```bash
mkfs.vfat -I -F 32 /dev/sdX
```

- `-I`：忽略并禁用安全检查。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 从文件读取坏块列表

```bash
mkfs.vfat -l badblocks.txt -F 32 /dev/sdX
```

- `-l FILENAME`：从指定的文件 `FILENAME` 中读取坏块列表。例如，`badblocks.txt` 是坏块列表文件。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 替换引导块中的默认错误信息

```bash
mkfs.vfat -m errmsg.txt -F 32 /dev/sdX
```

- `-m FILENAME`：用指定的文件 `FILENAME` 中的内容替换引导块中的默认错误信息。例如，`errmsg.txt` 是错误信息文件。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 使用常量生成随机值

```bash
mkfs.vfat --invariant -F 32 /dev/sdX
```

- `--invariant`：使用常量代替随机生成或基于时间的值。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 在特定扇区写入文件系统

```bash
mkfs.vfat --offset=2048 -F 32 /dev/sdX
```

- `--offset=SECTOR`：将文件系统写入设备文件的特定扇区。例如，`2048` 表示从第 2048 个扇区开始。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

#### 综合应用

##### 创建 FAT32 文件系统并指定备份引导扇区

```bash
mkfs.vfat -b 100 -F 32 /dev/sdX
```

- `-b SECTOR`：选择 `SECTOR` 作为 FAT32 备份引导扇区的位置。例如，`100` 表示选择第 100 个扇区。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 32` 表示 FAT32 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建 FAT16 文件系统并设置根目录条目数量

```bash
mkfs.vfat -F 16 -r 512 /dev/sdX
```

- `-r COUNT`：为根目录至少留出 `COUNT` 个条目。例如，`512` 表示为根目录预留 512 个条目。
- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 16` 表示 FAT16 文件系统。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建 FAT12 文件系统并设置扇区大小

```bash
mkfs.vfat -F 12 -S 4096 /dev/sdX
```

- `-F SIZE`：选择 FAT 文件系统的大小。例如，`-F 12` 表示 FAT12 文件系统。
- `-S SIZE`：选择扇区大小为 `SIZE` 字节（必须是 2 的幂，至少为 512）。例如，`4096` 表示扇区大小为 4096 字节。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

### mkfs.exfat
```bash
bash-5.1# mkfs.exfat --help
Usage: mkfs.exfat
        -L | --volume-label=label                              Set volume label
        -c | --cluster-size=size(or suffixed by 'K' or 'M')    Specify cluster size
        -b | --boundary-align=size(or suffixed by 'K' or 'M')  Specify boundary alignment
             --pack-bitmap                                     Move bitmap into FAT segment
        -f | --full-format                                     Full format
        -V | --version                                         Show version
        -v | --verbose                                         Print debug
        -h | --help                                            Show help
```

#### 参数解析

##### 设置卷标

```bash
mkfs.exfat -L MyVolumeLabel /dev/sdX
```

- `-L | --volume-label=label`：设置卷标为 `label`。例如，`-L MyVolumeLabel` 设置卷标为 `MyVolumeLabel`。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 指定簇大小

```bash
mkfs.exfat -c 64K /dev/sdX
```

- `-c | --cluster-size=size`：指定簇大小为 `size`。`size` 可以带有 `K` 或 `M` 后缀，表示千字节或兆字节。例如，`64K` 表示簇大小为 64KB。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 指定边界对齐

```bash
mkfs.exfat -b 1M /dev/sdX
```

- `-b | --boundary-align=size`：指定边界对齐为 `size`。`size` 可以带有 `K` 或 `M` 后缀，表示千字节或兆字节。例如，`1M` 表示边界对齐为 1MB。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 移动位图到 FAT 段

```bash
mkfs.exfat --pack-bitmap /dev/sdX
```

- `--pack-bitmap`：将位图移动到 FAT 段。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 完全格式化

```bash
mkfs.exfat -f /dev/sdX
```

- `-f | --full-format`：执行完全格式化。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 显示版本信息

```bash
mkfs.exfat -V
```

- `-V | --version`：显示版本信息。

##### 启用调试输出

```bash
mkfs.exfat -v /dev/sdX
```

- `-v | --verbose`：启用调试输出，打印详细信息。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 显示帮助信息

```bash
mkfs.exfat -h
```

- `-h | --help`：显示帮助信息。

#### 综合应用

##### 创建一个带有自定义卷标和簇大小的 exFAT 文件系统

```bash
mkfs.exfat -L MyVolume -c 128K /dev/sdX
```

- `-L MyVolume`：设置卷标为 `MyVolume`。
- `-c 128K`：设置簇大小为 128KB。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建一个 exFAT 文件系统并进行完全格式化

```bash
mkfs.exfat -f /dev/sdX
```

- `-f`：执行完全格式化。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建一个 exFAT 文件系统并将位图移动到 FAT 段

```bash
mkfs.exfat --pack-bitmap /dev/sdX
```

- `--pack-bitmap`：将位图移动到 FAT 段。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。



### mkfs.msdos
```bash
bash-5.1# mkfs.msdos --help
mkfs.fat 4.2 (2021-01-31)
Usage: /usr/sbin/mkfs.msdos [OPTIONS] TARGET [BLOCKS]
Create FAT filesystem in TARGET, which can be a block device or file. Use only
up to BLOCKS 1024 byte blocks if specified. With the -C option, file TARGET will be
created with a size of 1024 bytes times BLOCKS, which must be specified.

Options:
  -a              Disable alignment of data structures
  -A              Toggle Atari variant of the filesystem
  -b SECTOR       Select SECTOR as location of the FAT32 backup boot sector
  -c              Check device for bad blocks before creating the filesystem
  -C              Create file TARGET then create filesystem in it
  -D NUMBER       Write BIOS drive number NUMBER to boot sector
  -f COUNT        Create COUNT file allocation tables
  -F SIZE         Select FAT size SIZE (12, 16 or 32)
  -g GEOM         Select disk geometry: heads/sectors_per_track
  -h NUMBER       Write hidden sectors NUMBER to boot sector
  -i VOLID        Set volume ID to VOLID (a 32 bit hexadecimal number)
  -I              Ignore and disable safety checks
  -l FILENAME     Read bad blocks list from FILENAME
  -m FILENAME     Replace default error message in boot block with contents of FILENAME
  -M TYPE         Set media type in boot sector to TYPE
  --mbr[=y|n|a]   Fill (fake) MBR table with one partition which spans whole disk
  -n LABEL        Set volume name to LABEL (up to 11 characters long)
  --codepage=N    use DOS codepage N to encode label (default: 850)
  -r COUNT        Make room for at least COUNT entries in the root directory
  -R COUNT        Set minimal number of reserved sectors to COUNT
  -s COUNT        Set number of sectors per cluster to COUNT
  -S SIZE         Select a sector size of SIZE (a power of two, at least 512)
  -v              Verbose execution
  --variant=TYPE  Select variant TYPE of filesystem (standard or Atari)

  --invariant     Use constants for randomly generated or time based values
  --offset=SECTOR Write the filesystem at a specific sector into the device file.
  --help          Show this help message and exit
```


#### 参数解析

##### 创建带有 FAT32 备份引导扇区的文件系统

```bash
mkfs.msdos -b 6 /dev/sdX
```

- `-b SECTOR`：选择 `SECTOR` 作为 FAT32 备份引导扇区的位置（例如 `6`）。

##### 在设备上创建 FAT 文件系统

```bash
mkfs.msdos -C myfile.img 1024
```

- `-C`：在文件 `myfile.img` 中创建文件系统，其大小为 `1024` 1024 字节块（即 1MB）。

##### 设置 FAT 大小为 16

```bash
mkfs.msdos -F 16 /dev/sdX
```

- `-F SIZE`：选择 FAT 大小为 `SIZE`（可以是 `12`、`16` 或 `32`，例如 `16`）。

##### 设置每簇的扇区数量

```bash
mkfs.msdos -s 8 /dev/sdX
```

- `-s COUNT`：设置每簇的扇区数量为 `COUNT`（例如 `8`）。

##### 设置卷标

```bash
mkfs.msdos -n MYVOL /dev/sdX
```

- `-n LABEL`：设置卷标为 `LABEL`（例如 `MYVOL`），最大长度为 11 个字符。

##### 设置扇区大小为 4096 字节

```bash
mkfs.msdos -S 4096 /dev/sdX
```

- `-S SIZE`：选择扇区大小为 `SIZE`（例如 `4096` 字节）。

##### 创建文件系统并检查坏块

```bash
mkfs.msdos -c /dev/sdX
```

- `-c`：在创建文件系统之前检查坏块。

##### 设置文件系统变体为 Atari

```bash
mkfs.msdos --variant=Atari /dev/sdX
```

- `--variant=TYPE`：选择文件系统变体 `TYPE`（例如 `Atari`）。

##### 忽略安全检查

```bash
mkfs.msdos -I /dev/sdX
```

- `-I`：忽略并禁用安全检查。

##### 选择媒体类型

```bash
mkfs.msdos -M 0xF8 /dev/sdX
```

- `-M TYPE`：设置媒体类型 `TYPE`（例如 `0xF8`）。

##### 设置隐藏扇区数量

```bash
mkfs.msdos -h 32 /dev/sdX
```

- `-h NUMBER`：设置隐藏扇区数量为 `NUMBER`（例如 `32`）。

#### 综合应用

##### 创建一个具有自定义扇区大小和簇大小的 FAT 文件系统

```bash
mkfs.msdos -S 4096 -s 16 /dev/sdX
```

- `-S 4096`：设置扇区大小为 4096 字节。
- `-s 16`：设置每簇的扇区数量为 16。

##### 在文件中创建带有 FAT16 的文件系统

```bash
mkfs.msdos -F 16 -C mydisk.img 2048
```

- `-F 16`：选择 FAT16 文件系统。
- `-C mydisk.img 2048`：在文件 `mydisk.img` 中创建文件系统，大小为 `2048` 1024 字节块（即 2MB）。

##### 创建带有特定卷标并检查坏块的 FAT 文件系统

```bash
mkfs.msdos -n MYLABEL -c /dev/sdX
```

- `-n MYLABEL`：设置卷标为 `MYLABEL`。
- `-c`：在创建文件系统之前检查坏块。

##### 创建具有特定 FAT 大小并设置卷标的 FAT 文件系统

```bash
mkfs.msdos -F 32 -n NEWVOL /dev/sdX
```

- `-F 32`：选择 FAT32 文件系统。
- `-n NEWVOL`：设置卷标为 `NEWVOL`。



## e2fsprogs
### mkfs.ext2
```bash
bash-5.1# mkfs.ext2 --help
Usage: mkfs.ext2 [-c|-l filename] [-b block-size] [-C cluster-size]
        [-i bytes-per-inode] [-I inode-size] [-J journal-options]
        [-G flex-group-size] [-N number-of-inodes] [-d root-directory]
        [-m reserved-blocks-percentage] [-o creator-os]
        [-g blocks-per-group] [-L volume-label] [-M last-mounted-directory]
        [-O feature[,...]] [-r fs-revision] [-E extended-option[,...]]
        [-t fs-type] [-T usage-type ] [-U UUID] [-e errors_behavior][-z undo_file]
        [-jnqvDFSV] device [blocks-count]
```

#### 参数解析

##### 检查坏块或指定坏块列表文件

```bash
mkfs.ext2 -c /dev/sdX
```

- `-c`：在创建文件系统前检查坏块。
- `-l filename`：从 `filename` 文件中读取坏块列表。

##### 设置块大小

```bash
mkfs.ext2 -b 4096 /dev/sdX
```

- `-b block-size`：设置块大小为 `block-size` 字节（例如 `4096` 表示 4KB）。

##### 设置簇大小

```bash
mkfs.ext2 -C 16K /dev/sdX
```

- `-C cluster-size`：设置簇大小为 `cluster-size`（可以带有 `K` 或 `M` 后缀）。

##### 设置每个 inode 的字节数

```bash
mkfs.ext2 -i 1024 /dev/sdX
```

- `-i bytes-per-inode`：设置每个 inode 的字节数为 `bytes-per-inode`（例如 `1024` 表示 1KB）。

##### 设置 inode 大小

```bash
mkfs.ext2 -I 128 /dev/sdX
```

- `-I inode-size`：设置 inode 大小为 `inode-size` 字节（例如 `128` 表示 128 字节）。

##### 设置日志选项

```bash
mkfs.ext2 -J size=256M /dev/sdX
```

- `-J journal-options`：设置日志选项。选项可以包括日志大小、日志位置等。

##### 设置弹性组大小

```bash
mkfs.ext2 -G 64 /dev/sdX
```

- `-G flex-group-size`：设置弹性组大小为 `flex-group-size` 块（例如 `64` 块）。

##### 设置 inode 数量

```bash
mkfs.ext2 -N 100000 /dev/sdX
```

- `-N number-of-inodes`：设置 inode 数量为 `number-of-inodes`（例如 `100000`）。

##### 设置根目录

```bash
mkfs.ext2 -d /path/to/dir /dev/sdX
```

- `-d root-directory`：设置根目录为 `root-directory`（例如 `/path/to/dir`）。

##### 设置保留块百分比

```bash
mkfs.ext2 -m 5 /dev/sdX
```

- `-m reserved-blocks-percentage`：设置保留块的百分比（例如 `5` 表示 5%）。

##### 设置创建操作系统

```bash
mkfs.ext2 -o Linux /dev/sdX
```

- `-o creator-os`：设置创建操作系统为 `creator-os`（例如 `Linux`）。

##### 设置每组块数量

```bash
mkfs.ext2 -g 8192 /dev/sdX
```

- `-g blocks-per-group`：设置每组块的数量为 `blocks-per-group`（例如 `8192`）。

##### 设置卷标

```bash
mkfs.ext2 -L MyVolumeLabel /dev/sdX
```

- `-L volume-label`：设置卷标为 `volume-label`（例如 `MyVolumeLabel`）。

##### 设置最后挂载目录

```bash
mkfs.ext2 -M /mnt /dev/sdX
```

- `-M last-mounted-directory`：设置最后挂载目录为 `last-mounted-directory`（例如 `/mnt`）。

##### 启用文件系统特性

```bash
mkfs.ext2 -O 64bit /dev/sdX
```

- `-O feature[,...]`：启用文件系统特性，如 `64bit`。

##### 设置文件系统修订版

```bash
mkfs.ext2 -r 1.0 /dev/sdX
```

- `-r fs-revision`：设置文件系统修订版为 `fs-revision`（例如 `1.0`）。

##### 设置扩展选项

```bash
mkfs.ext2 -E test_option=value /dev/sdX
```

- `-E extended-option[,...]`：设置扩展选项，如 `test_option=value`。

##### 设置文件系统类型

```bash
mkfs.ext2 -t ext2 /dev/sdX
```

- `-t fs-type`：设置文件系统类型为 `fs-type`（例如 `ext2`）。

##### 设置使用类型

```bash
mkfs.ext2 -T largefile /dev/sdX
```

- `-T usage-type`：设置文件系统的使用类型，如 `largefile`。

##### 设置 UUID

```bash
mkfs.ext2 -U 12345678-1234-1234-1234-1234567890ab /dev/sdX
```

- `-U UUID`：设置文件系统的 UUID 为 `UUID`（例如 `12345678-1234-1234-1234-1234567890ab`）。

##### 设置错误行为

```bash
mkfs.ext2 -e remount-ro /dev/sdX
```

- `-e errors_behavior`：设置错误行为，如 `remount-ro`。

##### 设置撤销文件

```bash
mkfs.ext2 -z /path/to/undo_file /dev/sdX
```

- `-z undo_file`：设置撤销文件为 `undo_file`（例如 `/path/to/undo_file`）。

##### 其他选项

```bash
mkfs.ext2 -j -n -q -v /dev/sdX
```

- `-j`：启用日志。
- `-n`：不实际写入文件系统。
- `-q`：静默模式。
- `-v`：详细模式。

#### 综合应用

##### 创建具有指定块大小和簇大小的 ext2 文件系统

```bash
mkfs.ext2 -b 4096 -C 8K /dev/sdX
```

- `-b 4096`：设置块大小为 4KB。
- `-C 8K`：设置簇大小为 8KB。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建具有自定义卷标和日志的 ext2 文件系统

```bash
mkfs.ext2 -L MyVolumeLabel -J size=128M /dev/sdX
```

- `-L MyVolumeLabel`：设置卷标为 `MyVolumeLabel`。
- `-J size=128M`：设置日志大小为 128MB。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建具有特定 UUID 和保留块百分比的 ext2 文件系统

```bash
mkfs.ext2 -U 12345678-1234-1234-1234-1234567890ab -m 10 /dev/sdX
```

- `-U 12345678-1234-1234-1234-1234567890ab`：设置 UUID 为 `12345678-1234-1234-1234-1234567890ab`。
- `-m 10`：设置保留块的百分比为 10%。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。



### mkfs.ext3
```bash
Usage: mkfs.ext3 [-c|-l filename] [-b block-size] [-C cluster-size]
        [-i bytes-per-inode] [-I inode-size] [-J journal-options]
        [-G flex-group-size] [-N number-of-inodes] [-d root-directory]
        [-m reserved-blocks-percentage] [-o creator-os]
        [-g blocks-per-group] [-L volume-label] [-M last-mounted-directory]
        [-O feature[,...]] [-r fs-revision] [-E extended-option[,...]]
        [-t fs-type] [-T usage-type ] [-U UUID] [-e errors_behavior][-z undo_file]
        [-jnqvDFSV] device [blocks-count]
```

#### 参数解析

##### 检查坏块或指定坏块列表文件

```bash
mkfs.ext3 -c /dev/sdX
```

- `-c`：在创建文件系统前检查坏块。
- `-l filename`：从 `filename` 文件中读取坏块列表。

##### 设置块大小

```bash
mkfs.ext3 -b 4096 /dev/sdX
```

- `-b block-size`：设置块大小为 `block-size` 字节（例如 `4096` 表示 4KB）。

##### 设置簇大小

```bash
mkfs.ext3 -C 16K /dev/sdX
```

- `-C cluster-size`：设置簇大小为 `cluster-size`（可以带有 `K` 或 `M` 后缀）。

##### 设置每个 inode 的字节数

```bash
mkfs.ext3 -i 1024 /dev/sdX
```

- `-i bytes-per-inode`：设置每个 inode 的字节数为 `bytes-per-inode`（例如 `1024` 表示 1KB）。

##### 设置 inode 大小

```bash
mkfs.ext3 -I 128 /dev/sdX
```

- `-I inode-size`：设置 inode 大小为 `inode-size` 字节（例如 `128` 表示 128 字节）。

##### 设置日志选项

```bash
mkfs.ext3 -J size=256M /dev/sdX
```

- `-J journal-options`：设置日志选项。选项可以包括日志大小、日志位置等。

##### 设置弹性组大小

```bash
mkfs.ext3 -G 64 /dev/sdX
```

- `-G flex-group-size`：设置弹性组大小为 `flex-group-size` 块（例如 `64` 块）。

##### 设置 inode 数量

```bash
mkfs.ext3 -N 100000 /dev/sdX
```

- `-N number-of-inodes`：设置 inode 数量为 `number-of-inodes`（例如 `100000`）。

##### 设置根目录

```bash
mkfs.ext3 -d /path/to/dir /dev/sdX
```

- `-d root-directory`：设置根目录为 `root-directory`（例如 `/path/to/dir`）。

##### 设置保留块百分比

```bash
mkfs.ext3 -m 5 /dev/sdX
```

- `-m reserved-blocks-percentage`：设置保留块的百分比（例如 `5` 表示 5%）。

##### 设置创建操作系统

```bash
mkfs.ext3 -o Linux /dev/sdX
```

- `-o creator-os`：设置创建操作系统为 `creator-os`（例如 `Linux`）。

##### 设置每组块数量

```bash
mkfs.ext3 -g 8192 /dev/sdX
```

- `-g blocks-per-group`：设置每组块的数量为 `blocks-per-group`（例如 `8192`）。

##### 设置卷标

```bash
mkfs.ext3 -L MyVolumeLabel /dev/sdX
```

- `-L volume-label`：设置卷标为 `volume-label`（例如 `MyVolumeLabel`）。

##### 设置最后挂载目录

```bash
mkfs.ext3 -M /mnt /dev/sdX
```

- `-M last-mounted-directory`：设置最后挂载目录为 `last-mounted-directory`（例如 `/mnt`）。

##### 启用文件系统特性

```bash
mkfs.ext3 -O 64bit /dev/sdX
```

- `-O feature[,...]`：启用文件系统特性，如 `64bit`。

##### 设置文件系统修订版

```bash
mkfs.ext3 -r 1.0 /dev/sdX
```

- `-r fs-revision`：设置文件系统修订版为 `fs-revision`（例如 `1.0`）。

##### 设置扩展选项

```bash
mkfs.ext3 -E test_option=value /dev/sdX
```

- `-E extended-option[,...]`：设置扩展选项，如 `test_option=value`。

##### 设置文件系统类型

```bash
mkfs.ext3 -t ext3 /dev/sdX
```

- `-t fs-type`：设置文件系统类型为 `fs-type`（例如 `ext3`）。

##### 设置使用类型

```bash
mkfs.ext3 -T largefile /dev/sdX
```

- `-T usage-type`：设置文件系统的使用类型，如 `largefile`。

##### 设置 UUID

```bash
mkfs.ext3 -U 12345678-1234-1234-1234-1234567890ab /dev/sdX
```

- `-U UUID`：设置文件系统的 UUID 为 `UUID`（例如 `12345678-1234-1234-1234-1234567890ab`）。

##### 设置错误行为

```bash
mkfs.ext3 -e remount-ro /dev/sdX
```

- `-e errors_behavior`：设置错误行为，如 `remount-ro`。

##### 设置撤销文件

```bash
mkfs.ext3 -z /path/to/undo_file /dev/sdX
```

- `-z undo_file`：设置撤销文件为 `undo_file`（例如 `/path/to/undo_file`）。

##### 其他选项

```bash
mkfs.ext3 -j -n -q -v /dev/sdX
```

- `-j`：启用日志。
- `-n`：不实际写入文件系统。
- `-q`：静默模式。
- `-v`：详细模式。

#### 综合应用

##### 创建具有指定块大小和簇大小的 ext3 文件系统

```bash
mkfs.ext3 -b 4096 -C 8K /dev/sdX
```

- `-b 4096`：设置块大小为 4KB。
- `-C 8K`：设置簇大小为 8KB。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建具有自定义卷标和日志的 ext3 文件系统

```bash
mkfs.ext3 -L MyVolumeLabel -J size=128M /dev/sdX
```

- `-L MyVolumeLabel`：设置卷标为 `MyVolumeLabel`。
- `-J size=128M`：设置日志大小为 128MB。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建具有特定 UUID 和保留块百分比的 ext3 文件系统

```bash
mkfs.ext3 -U 12345678-1234-1234-1234-1234567890ab -m 10 /dev/sdX
```

- `-U 12345678-1234-1234-1234-1234567890ab`：设置 UUID 为 `12345678-1234-1234-1234-1234567890ab`。
- `-m 10`：设置保留块的百分比为 10%。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

### mkfs.ext4
```bash
bash-5.1# mkfs.ext4 --help
Usage: mkfs.ext4 [-c|-l filename] [-b block-size] [-C cluster-size]
        [-i bytes-per-inode] [-I inode-size] [-J journal-options]
        [-G flex-group-size] [-N number-of-inodes] [-d root-directory]
        [-m reserved-blocks-percentage] [-o creator-os]
        [-g blocks-per-group] [-L volume-label] [-M last-mounted-directory]
        [-O feature[,...]] [-r fs-revision] [-E extended-option[,...]]
        [-t fs-type] [-T usage-type ] [-U UUID] [-e errors_behavior][-z undo_file]
        [-jnqvDFSV] device [blocks-count]
```

### mkfs.ubifs
```bash

```


### mkfs.jffs2
```bash

```


#### 参数解析

##### 检查坏块或指定坏块列表文件

```bash
mkfs.ext4 -c /dev/sdX
```

- `-c`：在创建文件系统前检查坏块。
- `-l filename`：从 `filename` 文件中读取坏块列表。

##### 设置块大小

```bash
mkfs.ext4 -b 4096 /dev/sdX
```

- `-b block-size`：设置块大小为 `block-size` 字节（例如 `4096` 表示 4KB）。

##### 设置簇大小

```bash
mkfs.ext4 -C 16K /dev/sdX
```

- `-C cluster-size`：设置簇大小为 `cluster-size`（可以带有 `K` 或 `M` 后缀）。

##### 设置每个 inode 的字节数

```bash
mkfs.ext4 -i 1024 /dev/sdX
```

- `-i bytes-per-inode`：设置每个 inode 的字节数为 `bytes-per-inode`（例如 `1024` 表示 1KB）。

##### 设置 inode 大小

```bash
mkfs.ext4 -I 128 /dev/sdX
```

- `-I inode-size`：设置 inode 大小为 `inode-size` 字节（例如 `128` 表示 128 字节）。

##### 设置日志选项

```bash
mkfs.ext4 -J size=256M /dev/sdX
```

- `-J journal-options`：设置日志选项。选项可以包括日志大小、日志位置等。

##### 设置弹性组大小

```bash
mkfs.ext4 -G 64 /dev/sdX
```

- `-G flex-group-size`：设置弹性组大小为 `flex-group-size` 块（例如 `64` 块）。

##### 设置 inode 数量

```bash
mkfs.ext4 -N 100000 /dev/sdX
```

- `-N number-of-inodes`：设置 inode 数量为 `number-of-inodes`（例如 `100000`）。

##### 设置根目录

```bash
mkfs.ext4 -d /path/to/dir /dev/sdX
```

- `-d root-directory`：设置根目录为 `root-directory`（例如 `/path/to/dir`）。

##### 设置保留块百分比

```bash
mkfs.ext4 -m 5 /dev/sdX
```

- `-m reserved-blocks-percentage`：设置保留块的百分比（例如 `5` 表示 5%）。

##### 设置创建操作系统

```bash
mkfs.ext4 -o Linux /dev/sdX
```

- `-o creator-os`：设置创建操作系统为 `creator-os`（例如 `Linux`）。

##### 设置每组块数量

```bash
mkfs.ext4 -g 8192 /dev/sdX
```

- `-g blocks-per-group`：设置每组块的数量为 `blocks-per-group`（例如 `8192`）。

##### 设置卷标

```bash
mkfs.ext4 -L MyVolumeLabel /dev/sdX
```

- `-L volume-label`：设置卷标为 `volume-label`（例如 `MyVolumeLabel`）。

##### 设置最后挂载目录

```bash
mkfs.ext4 -M /mnt /dev/sdX
```

- `-M last-mounted-directory`：设置最后挂载目录为 `last-mounted-directory`（例如 `/mnt`）。

##### 启用文件系统特性

```bash
mkfs.ext4 -O 64bit /dev/sdX
```

- `-O feature[,...]`：启用文件系统特性，如 `64bit`。

##### 设置文件系统修订版

```bash
mkfs.ext4 -r 1.0 /dev/sdX
```

- `-r fs-revision`：设置文件系统修订版为 `fs-revision`（例如 `1.0`）。

##### 设置扩展选项

```bash
mkfs.ext4 -E test_option=value /dev/sdX
```

- `-E extended-option[,...]`：设置扩展选项，如 `test_option=value`。

##### 设置文件系统类型

```bash
mkfs.ext4 -t ext4 /dev/sdX
```

- `-t fs-type`：设置文件系统类型为 `fs-type`（例如 `ext4`）。

##### 设置使用类型

```bash
mkfs.ext4 -T largefile /dev/sdX
```

- `-T usage-type`：设置文件系统的使用类型，如 `largefile`。

##### 设置 UUID

```bash
mkfs.ext4 -U 12345678-1234-1234-1234-1234567890ab /dev/sdX
```

- `-U UUID`：设置文件系统的 UUID 为 `UUID`（例如 `12345678-1234-1234-1234-1234567890ab`）。

##### 设置错误行为

```bash
mkfs.ext4 -e remount-ro /dev/sdX
```

- `-e errors_behavior`：设置错误行为，如 `remount-ro`。

##### 设置撤销文件

```bash
mkfs.ext4 -z /path/to/undo_file /dev/sdX
```

- `-z undo_file`：设置撤销文件为 `undo_file`（例如 `/path/to/undo_file`）。

##### 其他选项

```bash
mkfs.ext4 -j -n -q -v /dev/sdX
```

- `-j`：启用日志。
- `-n`：不实际写入文件系统。
- `-q`：静默模式。
- `-v`：详细模式。

#### 综合应用

##### 创建具有指定块大小和簇大小的 ext4 文件系统

```bash
mkfs.ext4 -b 4096 -C 8K /dev/sdX
```

- `-b 4096`：设置块大小为 4KB。
- `-C 8K`：设置簇大小为 8KB。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建具有自定义卷标和日志的 ext4 文件系统

```bash
mkfs.ext4 -L MyVolumeLabel -J size=128M /dev/sdX
```

- `-L MyVolumeLabel`：设置卷标为 `MyVolumeLabel`。
- `-J size=128M`：设置日志大小为 128MB。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

##### 创建具有特定 UUID 和保留块百分比的 ext4 文件系统

```bash
mkfs.ext4 -U 12345678-1234-1234-1234-1234567890ab -m 10 /dev/sdX
```

- `-U 12345678-1234-1234-1234-1234567890ab`：设置 UUID 为 `12345678-1234-1234-1234-1234567890ab`。
- `-m 10`：设置保留块的百分比为 10%。
- `TARGET`：指定目标设备或文件。例如，`/dev/sdX` 是目标设备。

## fdisk
```bash
bash-5.1# fdisk --help
BusyBox v1.35.0 (2024-07-20 11:38:35 CST) multi-call binary.

Usage: fdisk [-ul] [-C CYLINDERS] [-H HEADS] [-S SECTORS] [-b SSZ] DISK

Change partition table

        -u              Start and End are in sectors (instead of cylinders)
        -l              Show partition table for each DISK, then exit
        -b 2048         (for certain MO disks) use 2048-byte sectors
        -C CYLINDERS    Set number of cylinders/heads/sectors
        -H HEADS        Typically 255
        -S SECTORS      Typically 63
```

#### 参数解析

##### 显示指定磁盘的分区表

```bash
fdisk -l /dev/sdX
```

- `-l`：显示每个磁盘的分区表，然后退出。

##### 使用特定的扇区大小处理磁盘

```bash
fdisk -b 2048 /dev/sdX
```

- `-b SSZ`：为特定的 MO 磁盘使用 `SSZ` 字节的扇区（例如 `2048` 字节）。

##### 设置磁盘的柱面、磁头和扇区参数

```bash
fdisk -C 1024 -H 255 -S 63 /dev/sdX
```

- `-C CYLINDERS`：设置柱面数量（例如 `1024`）。
- `-H HEADS`：设置磁头数量（例如 `255`）。
- `-S SECTORS`：设置每个磁头的扇区数量（例如 `63`）。

##### 将起始和结束单位改为扇区

```bash
fdisk -u /dev/sdX
```

- `-u`：将起始和结束单位改为扇区（而非柱面）。

#### 综合应用

##### 显示分区表并设置特定磁盘的柱面和扇区

```bash
fdisk -l -C 1024 -H 255 -S 63 /dev/sdX
```

- `-l`：显示磁盘的分区表。
- `-C 1024 -H 255 -S 63`：设置柱面为 `1024`，磁头为 `255`，每个磁头的扇区为 `63`。

##### 使用自定义扇区大小对磁盘进行分区

```bash
fdisk -b 2048 /dev/sdX
```

- `-b 2048`：为磁盘使用 `2048` 字节的扇区。


##  ipcrm
```bash
bash-5.1# ipcrm --help
BusyBox v1.35.0 (2024-07-20 11:38:35 CST) multi-call binary.

Usage: ipcrm [-MQS key] [-mqs id]

Upper-case options MQS remove an object by shmkey value.
Lower-case options remove an object by shmid value.

        -mM     Remove memory segment after last detach
        -qQ     Remove message queue
        -sS     Remove semaphore
```

### 参数解析

#### 删除共享内存段

```bash
ipcrm -m id
```

- `-m`：删除共享内存段。
- `id`：共享内存段的标识符（shmid）。

#### 删除消息队列

```bash
ipcrm -q id
```

- `-q`：删除消息队列。
- `id`：消息队列的标识符（msgid）。

#### 删除信号量集

```bash
ipcrm -s id
```

- `-s`：删除信号量集。
- `id`：信号量集的标识符（semid）。

#### 按键值删除共享内存段、消息队列或信号量集

```bash
ipcrm -M key
```

- `-M`：通过键值删除共享内存段。
- `key`：共享内存段的键值。

```bash
ipcrm -Q key
```

- `-Q`：通过键值删除消息队列。
- `key`：消息队列的键值。

```bash
ipcrm -S key
```

- `-S`：通过键值删除信号量集。
- `key`：信号量集的键值。

### 综合应用

#### 删除特定共享内存段

```bash
ipcrm -m 1234
```

- `-m 1234`：删除共享内存段标识符为 `1234` 的共享内存段。

#### 删除特定消息队列

```bash
ipcrm -q 5678
```

- `-q 5678`：删除消息队列标识符为 `5678` 的消息队列。

#### 删除特定信号量集

```bash
ipcrm -s 91011
```

- `-s 91011`：删除信号量集标识符为 `91011` 的信号量集。

## ipcs

```bash
bash-5.1# ipcs --help
BusyBox v1.35.0 (2024-07-20 11:38:35 CST) multi-call binary.

Usage: ipcs [[-smq] -i SHMID] | [[-asmq] [-tcplu]]

        -i ID   Show specific resource
Resource specification:
        -m      Shared memory segments
        -q      Message queues
        -s      Semaphore arrays
        -a      All (default)
Output format:
        -t      Time
        -c      Creator
        -p      Pid
        -l      Limits
        -u      Summary
```

### 参数解析

#### 查看共享内存段、消息队列或信号量数组的详细信息

```bash
ipcs -m -i SHMID
```

- `-m`：显示共享内存段。
- `-i ID`：显示指定标识符（SHMID）的共享内存段。

```bash
ipcs -q -i SHMID
```

- `-q`：显示消息队列。
- `-i ID`：显示指定标识符（MSGID）的消息队列。

```bash
ipcs -s -i SHMID
```

- `-s`：显示信号量数组。
- `-i ID`：显示指定标识符（SEMID）的信号量数组。

#### 查看所有资源的状态

```bash
ipcs -a
```

- `-a`：显示所有资源的状态（默认）。

#### 显示时间、创建者、进程、限制和总结信息

```bash
ipcs -t
```

- `-t`：显示时间相关信息。

```bash
ipcs -c
```

- `-c`：显示创建者信息。

```bash
ipcs -p
```

- `-p`：显示进程信息。

```bash
ipcs -l
```

- `-l`：显示限制信息。

```bash
ipcs -u
```

- `-u`：显示总结信息。

### 综合应用

#### 查看特定共享内存段的信息

```bash
ipcs -m -i 1234
```

- `-m -i 1234`：查看标识符为 `1234` 的共享内存段的详细信息。

#### 查看所有资源的状态并显示时间和创建者信息

```bash
ipcs -a -t -c
```

- `-a -t -c`：显示所有资源的状态，包括时间和创建者信息。




# 日志

## 查看最新的日志条目

```bash
tail -f /var/log/messages
```

这将实时显示日志文件的最新内容，直到你中断该命令。

## 查看最后 10 行日志

```bash
tail -n 10 /var/log/messages
```

这将显示日志文件的最后 10 行。

## 搜索特定的错误信息

```bash
grep "error" /var/log/messages
```

这将搜索日志文件中包含 "error" 的所有行。

## 搜索特定日期的日志

```bash
grep "Jul 24" /var/log/messages
```

这将搜索日志文件中包含特定日期（例如 2024 年 7 月 24 日）的所有行。

## 搜索特定服务的日志

```bash
grep "sshd" /var/log/messages
```

这将搜索与 SSH 服务相关的所有日志条目。

## 按照时间戳排序显示日志

```bash
sort -k 4 /var/log/messages
```

这将按照第四列（通常为时间戳）对日志条目进行排序。

## 查看特定时间段的日志

```bash
grep -A 100 "Jul 24 12:00" /var/log/messages
```

这将显示从特定时间点（例如 2024 年 7 月 24 日中午 12 点）开始的接下来大约 100 行日志。


# Service

在 SysVinit 系统中，`service` 命令是用来控制系统服务的启动、停止、重启、状态查询等操作的标准方式。以下是 `service` 命令的一些常见用法及示例：

## 查看所有可识别的服务
```bash
service --status-all
```
这将列出所有已知服务的状态，运行中的服务会标示为绿色的 `[ + ]`，未运行的则标示为红色的 `[ - ]`。

## 启动服务
```bash
service <service_name> start
```
例如，启动 Apache HTTP 服务器：
```bash
service httpd start
```

## 停止服务
```bash
service <service_name> stop
```
例如，停止 Apache HTTP 服务器：
```bash
service httpd stop
```

## 重启服务
```bash
service <service_name> restart
```
例如，重启 Apache HTTP 服务器：
```bash
service httpd restart
```

## 检查服务状态
```bash
service <service_name> status
```
例如，检查 Apache HTTP 服务器的状态：
```bash
service httpd status
```

## 重读配置文件并应用更改
```bash
service <service_name> reload
```
这通常用于那些支持热更新配置的服务，如 Apache HTTP 服务器：
```bash
service httpd reload
```

## 将服务设置为开机启动
```bash
chkconfig <service_name> on
```
例如，设置 Apache HTTP 服务器在系统启动时自动启动：
```bash
chkconfig httpd on
```

## 将服务设置为开机不启动
```bash
chkconfig <service_name> off
```
例如，设置 Apache HTTP 服务器不在系统启动时自动启动：
```bash
chkconfig httpd off
```

## 检查服务是否设置为开机启动
```bash
chkconfig --list <service_name>
```
例如，检查 Apache HTTP 服务器是否设置为开机启动：
```bash
chkconfig --list httpd
```

请注意，`chkconfig` 命令用于管理服务的启动脚本在系统启动时的自动启动行为，而 `service` 命令则用于在运行时控制服务的状态。在 Systemd 系统中，这些功能被整合到了 `systemctl` 命令中。


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

