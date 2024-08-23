# 命令
## 分区格式化
在cmd里面执行 `diskpart`，在出现的窗口中输入下面命令
```bash
select disk 1
clean
create partition primary size=16384
select partition 1
active
list disk
list volume
list partition

select disk 1
select partition 1
format fs=fat32 quick label=M62xx-Boot
list disk
list volume
list partition


select disk 1
select partition 1
set id=0C
select partition 1
active
list disk
list volume
list partition

select disk 1
create partition primary
select partition 2
format fs=exfat quick label=M62xx-Root
assign letter=M
list disk
list volume
list partition
```

## 镜像拷贝
```bash
C:\Windows\system32\cmd.exe /c "robocopy "C:\Users\leefly\Desktop\镜像" "F:" /E /ETA /BYTES"
```


# exe生成

![[Pasted image 20240823101445.png]]

![[Pasted image 20240823101520.png]]

# 问题汇总
##  问题1
格式化过程中出现问题导致驱动器变为 `RAW`：
```bash
Microsoft DiskPart 版本 10.0.19041.3636

Copyright (C) Microsoft Corporation.
在计算机上: DESKTOP-GC4LAR7

DISKPART> list disk

  磁盘 ###  状态           大小     可用     Dyn  Gpt
  --------  -------------  -------  -------  ---  ---
  磁盘 0    联机              931 GB  3072 KB        *
  磁盘 1    联机               59 GB      0 B

DISKPART> select disk 1

磁盘 1 现在是所选磁盘。

DISKPART> clean

DiskPart 成功地清除了磁盘。

DISKPART> create partition primary

DiskPart 成功地创建了指定分区。

DISKPART> select partition 1

分区 1 现在是所选分区。

DISKPART> format fs=fat32 quick label=M62xx-T

    0 百分比已完成

虚拟磁盘服务错误:
卷大小太大。


DISKPART> list volume

  卷 ###      LTR  标签         FS     类型        大小     状态       信息
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  卷     0     C                NTFS   磁盘分区         654 GB  正常         启动
  卷     1     D   新加卷          NTFS   磁盘分区         275 GB  正常
  卷     2                      FAT32  磁盘分区         100 MB  正常         系统
  卷     3                      NTFS   磁盘分区         591 MB  正常         已隐藏
* 卷     4     G                RAW    可移动           59 GB  正常
```

根据 diskpart 返回的错误信息，`虚拟磁盘服务错误: 卷大小太大`，这是由于 FAT32 文件系统无法处理超过` 32GB` 的卷大小。虽然 FAT32 实际上可以支持更大的卷，但 Windows 内置工具在格式化超过 32GB 的卷时存在限制。

###  解决方法
在Linux中进行格式化，但是还是无法从TF卡启动
```bash
U-Boot SPL 2021.01-00001-ga347ec164e-dirty (Aug 15 2024 - 17:50:42 +0800)
SYSFW ABI: 3.1 (firmware rev 0x0008 '8.6.4--v08.06.04 (Chill Capybar')
SPL initial stack usage: 13424 bytes
WDT:   Not found!
Trying to boot from MMC2
Authentication passed

Authentication passed

Authentication passed

Authentication passed

Authentication passed

Starting ATF on ARM64 core...

NOTICE:  BL31: v2.8(release):v2.8-226-g2fcd408bb
NOTICE:  BL31: Built : 17:50:08, Aug 15 2024
I/TC:
I/TC: OP-TEE version: Unknown_3.20 (gcc version 9.2.1 20191025 (GNU Toolchain for the A-profile Architecture 9.2-2019.12 (arm-9.10))) #1 Thu Aug 15 09:50:19 UTC 2024 aarch64
I/TC: WARNING: This OP-TEE configuration might be insecure!
I/TC: WARNING: Please check https://optee.readthedocs.io/en/latest/architecture/porting_guidelines.html
I/TC: Primary CPU initializing
I/TC: SYSFW ABI: 3.1 (firmware rev 0x0008 '8.6.4--v08.06.04 (Chill Capybar')
I/TC: HUK Initialized
I/TC: Activated SA2UL device
I/TC: Enabled firewalls for SA2UL TRNG device
I/TC: SA2UL TRNG initialized
I/TC: SA2UL Drivers initialized
I/TC: Primary CPU switching to normal world boot

U-Boot SPL 2021.01-00001-ga347ec164e-dirty (Aug 15 2024 - 17:50:25 +0800)
SYSFW ABI: 3.1 (firmware rev 0x0008 '8.6.4--v08.06.04 (Chill Capybar')
WDT:   Not found!
Trying to boot from MMC2
Error: FAT cluster size too big (cs=32768, max=16384)
spl_load_image_fat: error reading image u-boot.img, err - -6
SPL: failed to boot from all boot devices
### ERROR ### Please RESET the board ###
```

上述也是单个FAT32的分区大于`32GB`导致的，如果想用`64GB`的TF卡启动，可以使用`/home/leefly/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-08.06.00.42/bin/create-sdcard.sh`
脚本，将`64GB`的TF卡分为两个分区，一个是`fat32`分区，大小为`8GB`，另一个为`ext4`分区，占据剩余的空间。


###  命令
####  fdisk
```bash
root@AM62x:~# fdisk /dev/mmcblk1

Welcome to fdisk (util-linux 2.37.4).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): m

Help:

  DOS (MBR)
   a   toggle a bootable flag
   b   edit nested BSD disklabel
   c   toggle the dos compatibility flag

  Generic
   d   delete a partition
   F   list free unpartitioned space
   l   list known partition types
   n   add a new partition
   p   print the partition table
   t   change a partition type
   v   verify the partition table
   i   print information about a partition

  Misc
   m   print this menu
   u   change display/entry units
   x   extra functionality (experts only)

  Script
   I   load disk layout from sfdisk script file
   O   dump disk layout to sfdisk script file

  Save & Exit
   w   write table to disk and exit
   q   quit without saving changes

  Create a new label
   g   create a new empty GPT partition table
   G   create a new empty SGI (IRIX) partition table
   o   create a new empty DOS partition table
   s   create a new empty Sun partition table
```
#### 格式化命令
```bash
umount /run/media/mmcblk1p1
dd if=/dev/zero of=/dev/mmcblk1p1 bs=1024 count=1024

cat << END | fdisk /dev/mmcblk1
d
n
p
1
2048

t
c
a
w
END

mkfs.vfat -F 32 -n "boot" /dev/mmcblk1p1
```

上面的`a`是表明当前的分区为启动分区，如果没有这个设置，核心板无法从TF卡启动，可以通过

```bash
root@AM62x:~# fdisk -l /dev/mmcblk1
Disk /dev/mmcblk1: 59.45 GiB, 63831015424 bytes, 124669952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x51467675

Device         Boot Start       End   Sectors  Size Id Type
/dev/mmcblk1p1 *     2048 124669951 124667904 59.4G  c W95 FAT32 (LBA)
```

检查`/dev/mmcblk1p1 *`这里面的`*`号是否存在，如果存在，则表明当前分区为启动分区

### 日志
```bash
root@AM62x:~# df
Filesystem     1K-blocks   Used Available Use% Mounted on
udev              471152      0    471152   0% /dev
tmpfs              95332    280     95052   1% /run
overlay          3312232   1284   3122380   1% /
tmpfs             476640      0    476640   0% /dev/shm
tmpfs             476640  16940    459700   4% /tmp
tmpfs                 40      0        40   0% /mnt/.psplash
/dev/mmcblk0p3   3312232   1284   3122380   1% /run/media/mmcblk0p3
/dev/mmcblk0p2    271280 157748     92028  64% /run/media/mmcblk0p2
/dev/mmcblk0p1     64511  20541     43970  32% /run/media/mmcblk0p1
/dev/mmcblk1p1  62318688     32  62318656   1% /run/media/mmcblk1p1
root@AM62x:~#
root@AM62x:~# umount /run/media/mmcblk1p1
root@AM62x:~#
root@AM62x:~# fdisk -l /dev/mmcblk1
Disk /dev/mmcblk1: 59.45 GiB, 63831015424 bytes, 124669952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x51467675

Device         Boot Start       End   Sectors  Size Id Type
/dev/mmcblk1p1 *     2048 124669951 124667904 59.4G  c W95 FAT32 (LBA)
root@AM62x:~#
root@AM62x:~# dd if=/dev/zero of=/dev/mmcblk1p1 bs=1024 count=1024
1024+0 records in
1024+0 records out
1048576 bytes (1.0 MB, 1.0 MiB) copied, 0.184962 s, 5.7 MB/s
root@AM62x:~#
root@AM62x:~#
root@AM62x:~# fdisk -l /dev/mmcblk1
Disk /dev/mmcblk1: 59.45 GiB, 63831015424 bytes, 124669952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x51467675

Device         Boot Start       End   Sectors  Size Id Type
/dev/mmcblk1p1 *     2048 124669951 124667904 59.4G  c W95 FAT32 (LBA)
root@AM62x:~#
root@AM62x:~# fdisk /dev/mmcblk1

Welcome to fdisk (util-linux 2.37.4).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): d
Selected partition 1
Partition 1 has been deleted.

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-124669951, default 2048): 2048
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-124669951, default 124669951):

Created a new partition 1 of type 'Linux' and of size 59.4 GiB.

Command (m for help): t
Selected partition 1
Hex code or alias (type L to list all): c
Changed type of partition 'Linux' to 'W95 FAT32 (LBA)'.

Command (m for help): p
Disk /dev/mmcblk1: 59.45 GiB, 63831015424 bytes, 124669952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x51467675

Device         Boot Start       End   Sectors  Size Id Type
/dev/mmcblk1p1       2048 124669951 124667904 59.4G  c W95 FAT32 (LBA)

Command (m for help): i
Selected partition 1
         Device: /dev/mmcblk1p1
          Start: 2048
            End: 124669951
        Sectors: 124667904
      Cylinders: 708341
           Size: 59.4G
             Id: c
           Type: W95 FAT32 (LBA)
    Start-C/H/S: 11/14/1
      End-C/H/S: 767/21/8

Command (m for help): a
Selected partition 1
The bootable flag on partition 1 is enabled now.

Command (m for help): p
Disk /dev/mmcblk1: 59.45 GiB, 63831015424 bytes, 124669952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x51467675

Device         Boot Start       End   Sectors  Size Id Type
/dev/mmcblk1p1 *     2048 124669951 124667904 59.4G  c W95 FAT32 (LBA)

Command (m for help): i
Selected partition 1
         Device: /dev/mmcblk1p1
           Boot: *
          Start: 2048
            End: 124669951
        Sectors: 124667904
      Cylinders: 708341
           Size: 59.4G
             Id: c
           Type: W95 FAT32 (LBA)
    Start-C/H/S: 11/14/1
      End-C/H/S: 767/21/8
          Attrs: 80

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
[ 2144.338721]  mmcblk1: p1
Syncing disks.

[ 2144.349801]  mmcblk1: p1
root@AM62x:~#
root@AM62x:~#
root@AM62x:~# fdisk -l /dev/mmcblk1
Disk /dev/mmcblk1: 59.45 GiB, 63831015424 bytes, 124669952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x51467675

Device         Boot Start       End   Sectors  Size Id Type
/dev/mmcblk1p1 *     2048 124669951 124667904 59.4G  c W95 FAT32 (LBA)
root@AM62x:~#
root@AM62x:~# df
Filesystem     1K-blocks   Used Available Use% Mounted on
udev              471152      0    471152   0% /dev
tmpfs              95332    280     95052   1% /run
overlay          3312232   1284   3122380   1% /
tmpfs             476640      0    476640   0% /dev/shm
tmpfs             476640  16948    459692   4% /tmp
tmpfs                 40      0        40   0% /mnt/.psplash
/dev/mmcblk0p3   3312232   1284   3122380   1% /run/media/mmcblk0p3
/dev/mmcblk0p2    271280 157748     92028  64% /run/media/mmcblk0p2
/dev/mmcblk0p1     64511  20541     43970  32% /run/media/mmcblk0p1
root@AM62x:~#
root@AM62x:~# mkfs.vfat -F 32 -n "boot" /dev/mmcblk1p1
mkfs.fat 4.2 (2021-01-31)
mkfs.fat: Warning: lowercase labels might not work properly on some systems
root@AM62x:~#
root@AM62x:~#
root@AM62x:~#
root@AM62x:~# df
Filesystem     1K-blocks   Used Available Use% Mounted on
udev              471152      0    471152   0% /dev
tmpfs              95332    280     95052   1% /run
overlay          3312232   1284   3122380   1% /
tmpfs             476640      0    476640   0% /dev/shm
tmpfs             476640  16948    459692   4% /tmp
tmpfs                 40      0        40   0% /mnt/.psplash
/dev/mmcblk0p3   3312232   1284   3122380   1% /run/media/mmcblk0p3
/dev/mmcblk0p2    271280 157748     92028  64% /run/media/mmcblk0p2
/dev/mmcblk0p1     64511  20541     43970  32% /run/media/mmcblk0p1
root@AM62x:~#
root@AM62x:~# fdisk -l /dev/mmcblk1
Disk /dev/mmcblk1: 59.45 GiB, 63831015424 bytes, 124669952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x51467675

Device         Boot Start       End   Sectors  Size Id Type
/dev/mmcblk1p1 *     2048 124669951 124667904 59.4G  c W95 FAT32 (LBA)
```

## 问题2

![[Pasted image 20240822193324.png]]

![[Pasted image 20240823100718.png]]

## 问题3

遗留问题：
![[Pasted image 20240823100822.png]]
![[Pasted image 20240823100752.png]]

不影响使用，一秒左右自动消失

偶尔弹出如下窗口

![[Pasted image 20240823101010.png]]