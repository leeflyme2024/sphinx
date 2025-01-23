# U-Boot

# Kernel
## rtc
```bash
journalctl | grep rtc

ls -l /dev/rtc*
ls -l /sys/class/rtc/rtc0
ls -l /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051

cat /proc/driver/rtc
cat /proc/devices | grep rtc

ll /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/pcf85063_nvram0
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/pcf85063_nvram0/type
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/pcf85063_nvram0/uevent

ll /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/rtc/rtc0
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/rtc/rtc0/date
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/rtc/rtc0/dev
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/rtc/rtc0/hctosys
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/rtc/rtc0/max_user_freq
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/rtc/rtc0/name
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/rtc/rtc0/offset
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/rtc/rtc0/range
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/rtc/rtc0/since_epoch
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/rtc/rtc0/time
cat /sys/devices/platform/bus@f0000/20010000.i2c/i2c-2/2-0051/rtc/rtc0/uevent
```

## hwclock
```bash
hwclock
hwclock -h
hwclock -r
hwclock -w
hwclock -v
hwclock -U
hwclock --test
hwclock --show    # 显示硬件时钟时间
hwclock --systohc # 强制从系统时间同步到 RTC
hwclock --hctosys # 强制从 RTC 同步到系统时间
hwclock --systohc --utc
hwclock --hctosys --utc
hwclock --localtime
```

## date
```bash
date
date -s "2023-01-30 17:33:15"
```

## timedatectl
```bash
timedatectl
timedatectl list-timezones
timedatectl set-timezone Asia/Shanghai
timedatectl set-time "2025-01-22 17:54:16"
```

## timezone
```bash
cat /etc/timezone
cat /etc/localtime

ll /usr/share/zoneinfo/
rm /etc/localtime
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
ll /etc/localtime
```

在 Linux 系统中，查看当前时区的方法有多种，以下是常用的几种方式：

### **1. 使用 `timedatectl` 命令（推荐）**
**适用系统**：基于 systemd 的发行版（如 Ubuntu 18.04+、CentOS 7+、Debian 9+）。  
**命令**：
```bash
timedatectl
```
**输出示例**：
```
               Local time: Thu 2025-01-23 09:38:05 CST  
           Universal time: Thu 2025-01-23 01:38:05 UTC  
                 RTC time: Thu 2025-01-23 01:38:05      
                Time zone: Asia/Shanghai (CST, +0800)   
System clock synchronized: yes                          
              NTP service: active                       
          RTC in local TZ: no
```
**关键信息**：  
- `Time zone` 行显示当前时区（如 `Asia/Shanghai`）。


### **2. 查看 `/etc/timezone` 文件**
**适用系统**：Debian、Ubuntu 等。  
**命令**：
```bash
cat /etc/timezone
```
**输出示例**：
```
Asia/Shanghai
```


### **3. 检查 `/etc/localtime` 符号链接**
**原理**：`/etc/localtime` 是时区配置文件的软链接，指向 `/usr/share/zoneinfo/` 下的具体时区文件。  
**命令**：
```bash
ls -l /etc/localtime
```
**输出示例**：
```
lrwxrwxrwx 1 root root 33 Jan 23 09:38 /etc/localtime -> /usr/share/zoneinfo/Asia/Shanghai
```
**关键信息**：  
- 末尾路径 `Asia/Shanghai` 即为当前时区。


### **4. 使用 `date` 命令**
**功能**：显示当前时区缩写和 UTC 偏移。  
**命令**：
```bash
date +"%Z %z"
```
**输出示例**：
```
CST +0800
```
**说明**：  
- `%Z`：时区缩写（如 `CST`）。  
- `%z`：UTC 偏移（如 `+0800` 表示 UTC+8）。


### **总结**
| 方法                  | 适用场景                     | 输出内容               |
|-----------------------|----------------------------|-----------------------|
| `timedatectl`         | 现代 systemd 系统           | 完整时区名称（如 `Asia/Shanghai`） |
| `/etc/timezone`       | Debian/Ubuntu              | 时区名称              |
| `/etc/localtime` 链接 | 所有系统                    | 时区文件路径          |
| `date +"%Z %z"`       | 快速查看时区缩写和 UTC 偏移 | `CST +0800`          |

### **修改时区（附加说明）**
若需修改时区，可使用以下命令（需 root 权限）：
```bash
sudo timedatectl set-timezone Asia/Shanghai  # 替换为目标时区
```

## systemd-timedated
```bash
journalctl -u systemd-timedated
```

## 时间设置
在 Linux 系统中，设置时间并同步到 RTC（硬件时钟）的完整步骤如下：

---

### **1. 设置时区（Time Zone）**
#### **(1) 查看当前时区**
```bash
timedatectl
```
#### **(2) 列出可用时区**
```bash
timedatectl list-timezones | grep -i asia  # 示例：查找亚洲时区
```
#### **(3) 设置时区（如 `Asia/Shanghai`）**
```bash
sudo timedatectl set-timezone Asia/Shanghai
```
#### **(4) 验证时区**
```bash
timedatectl | grep "Time zone"
```

---

### **2. 设置系统时间**
#### **(1) 方法一：通过 NTP 自动同步（推荐）**
```bash
# 启用 NTP 时间同步服务
sudo timedatectl set-ntp true

# 检查同步状态（等待几分钟）
timedatectl status
```
- **预期输出**：
  ```
  System clock synchronized: yes
  NTP service: active
  ```

#### **(2) 方法二：手动设置时间**
```bash
# 关闭 NTP 服务（避免自动覆盖手动设置）
sudo timedatectl set-ntp false

# 手动设置系统时间（格式：YYYY-MM-DD HH:MM:SS）
sudo date -s "2025-01-23 09:38:05"

# 重新启用 NTP 服务（可选）
sudo timedatectl set-ntp true
```

---

### **3. 将系统时间同步到 RTC（硬件时钟）**
#### **(1) 同步命令**
```bash
# 推荐以 UTC 格式存储 RTC 时间（避免时区混乱）
sudo hwclock --systohc --utc

# 如果 RTC 必须存储本地时间（不推荐）
sudo hwclock --systohc --localtime
```
#### **(2) 验证 RTC 时间**
```bash
hwclock --show  # 显示 RTC 时间
```

---

### **4. 完整操作示例**
```bash
# 1. 设置时区
sudo timedatectl set-timezone Asia/Shanghai

# 2. 手动设置时间（若无网络）
sudo timedatectl set-ntp false
sudo date -s "2025-01-23 09:38:05"

# 3. 同步到 RTC（推荐 UTC 格式）
sudo hwclock --systohc --utc

# 4. 验证所有时间
timedatectl
hwclock --show
```

---

### **5. 关键注意事项**
1. **RTC 存储格式**：
   - **推荐使用 UTC**：确保硬件时钟始终以 UTC 格式存储，避免时区切换导致时间混乱。
   - 如果 RTC 已存储为本地时间，需保持一致（通过 `hwclock --localtime` 查看）。

2. **NTP 服务干扰**：
   - 手动设置时间前，需关闭 NTP 服务（`timedatectl set-ntp false`），否则时间可能被自动覆盖。

3. **虚拟化环境**：
   - 虚拟机中 RTC 可能由宿主机控制，此时 `hwclock` 命令可能无效。

4. **硬件时钟电池**：
   - 如果 RTC 时间频繁重置，可能是主板电池（CR2032）电量不足。

---

### **6. 时间同步流程示意图**
```
+-------------------+       +-------------------+       +-------------------+
|   System Time     |       |   NTP Service     |       |   RTC (硬件时钟)   |
|  (软件时钟，UTC)   |<------|  (网络时间同步)    |       |  (物理存储，UTC)   |
+-------------------+       +-------------------+       +-------------------+
         |                                                      ^
         |                                                      |
         +------------------ hwclock --systohc -----------------+
```

---

### **7. 常见问题解决**
#### **(1) NTP 同步失败**
- **检查网络**：确保可访问 NTP 服务器（如 `ping pool.ntp.org`）。
- **检查防火墙**：开放 UDP 123 端口。
- **更换 NTP 服务器**：编辑 `/etc/systemd/timesyncd.conf` 或 `/etc/chrony/chrony.conf`。

#### **(2) 时间写入 RTC 失败**
- **权限问题**：使用 `sudo` 执行命令。
- **硬件故障**：检查主板电池或 RTC 芯片。

---

### **总结**
- **设置时区**：`timedatectl set-timezone`。
- **同步时间**：NTP 自动同步或 `date -s` 手动设置。
- **写入 RTC**：`hwclock --systohc --utc`（推荐 UTC 格式）。
- **验证**：`timedatectl` 和 `hwclock --show`。

通过以上步骤，可确保 Linux 系统时间和硬件时钟的准确性和一致性。

## 时钟设置的服务

### 1. 时间同步服务
#### systemd-timesyncd.service
```bash
root@am62xx:~# cat /lib/systemd/system/systemd-timesyncd.service
#  SPDX-License-Identifier: LGPL-2.1+
#
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

[Unit]
Description=Network Time Synchronization
Documentation=man:systemd-timesyncd.service(8)
ConditionCapability=CAP_SYS_TIME
ConditionVirtualization=!container
DefaultDependencies=no
After=systemd-remount-fs.service systemd-sysusers.service
Before=time-set.target sysinit.target shutdown.target
Conflicts=shutdown.target
Wants=time-set.target time-sync.target

[Service]
AmbientCapabilities=CAP_SYS_TIME
CapabilityBoundingSet=CAP_SYS_TIME
ExecStart=!!/lib/systemd/systemd-timesyncd
LockPersonality=yes
MemoryDenyWriteExecute=yes
NoNewPrivileges=yes
PrivateDevices=yes
PrivateTmp=yes
ProtectControlGroups=yes
ProtectHome=yes
ProtectHostname=yes
ProtectKernelModules=yes
ProtectKernelTunables=yes
ProtectKernelLogs=yes
ProtectSystem=strict
Restart=always
RestartSec=0
RestrictAddressFamilies=AF_UNIX AF_INET AF_INET6
RestrictNamespaces=yes
RestrictRealtime=yes
RestrictSUIDSGID=yes
RuntimeDirectory=systemd/timesync
StateDirectory=systemd/timesync
SystemCallArchitectures=native
SystemCallErrorNumber=EPERM
SystemCallFilter=@system-service @clock
Type=notify
User=systemd-timesync
WatchdogSec=3min

[Install]
WantedBy=sysinit.target
Alias=dbus-org.freedesktop.timesync1.service
```
- 轻量级 NTP 客户端，负责通过 NTP 协议同步系统时间。
- 默认使用 `pool.ntp.org` 服务器，配置在 `/etc/systemd/timesyncd.conf`
	``` bash
	root@am62xx:~# cat /etc/systemd/timesyncd.conf
	#  This file is part of systemd.
	#
	#  systemd is free software; you can redistribute it and/or modify it
	#  under the terms of the GNU Lesser General Public License as published by
	#  the Free Software Foundation; either version 2.1 of the License, or
	#  (at your option) any later version.
	#
	# Entries in this file show the compile time defaults.
	# You can change settings by editing this file.
	# Defaults can be restored by simply deleting this file.
	#
	# See timesyncd.conf(5) for details.
	
	[Time]
	NTP=ntp1.aliyun.com
	FallbackNTP=ntp.ntsc.ac.cn ntp1.tuna.tsinghua.edu.cn ntp2.tuna.tsinghua.edu.cn
	#RootDistanceMaxSec=5
	#PollIntervalMinSec=32
	#PollIntervalMaxSec=2048
	```

- 负责同步系统时间与远程NTP（网络时间协议）服务器。它确保系统时钟与互联网时间标准保持同步
```bash
systemctl status systemd-timesyncd  # 查看服务状态
timedatectl timesync-status         # 查看同步详情
```
```bash
root@am62xx:~# systemctl status systemd-timesyncd
● systemd-timesyncd.service - Network Time Synchronization
     Loaded: loaded (/lib/systemd/system/systemd-timesyncd.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2025-01-23 13:41:49 CST; 28min ago
       Docs: man:systemd-timesyncd.service(8)
   Main PID: 431 (systemd-timesyn)
     Status: "Idle."
      Tasks: 2 (limit: 1104)
     Memory: 788.0K
     CGroup: /system.slice/systemd-timesyncd.service
             └─431 /lib/systemd/systemd-timesyncd

Jan 23 13:41:49 am62xx systemd[1]: Starting Network Time Synchronization...
Jan 23 13:41:49 am62xx systemd[1]: Started Network Time Synchronization.

root@am62xx:~# timedatectl timesync-status
       Server: (null) (ntp1.aliyun.com)
Poll interval: 0 (min: 32s; max 34min 8s)
 Packet count: 0
```

#### systemd-time-wait-sync.service
```bash
root@am62xx:~# cat /lib/systemd/system/systemd-time-wait-sync.service
#  SPDX-License-Identifier: LGPL-2.1+
#
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

[Unit]
Description=Wait Until Kernel Time Synchronized
Documentation=man:systemd-time-wait-sync.service(8)

# Note that this tool doesn't need CAP_SYS_TIME itself, but it's primary
# usecase is to run in conjunction with a local NTP service such as
# systemd-timesyncd.service, which is conditioned this way. There might be
# niche usecases where running this service independently is desired, but let's
# make this all "just work" for the general case, and leave it to local
# modifications to make it work in the remaining cases.

ConditionCapability=CAP_SYS_TIME
ConditionVirtualization=!container

DefaultDependencies=no
Before=time-sync.target shutdown.target
Wants=time-sync.target
Conflicts=shutdown.target

[Service]
Type=oneshot
ExecStart=/lib/systemd/systemd-time-wait-sync
TimeoutStartSec=infinity
RemainAfterExit=yes

[Install]
WantedBy=sysinit.target
```
- **功能**：  
	- 在系统启动时 **等待时间同步完成**，确保关键服务在正确时间后启动。  
	- 需与 `systemd-timesyncd` 或第三方 NTP 服务（如 `chronyd`）配合使用。  
- **启用方式**：  
```bash
systemctl enable systemd-time-wait-sync
```

#### dbus-org.freedesktop.timesync1.service
```bash
root@am62xx:~# cat /etc/systemd/system/dbus-org.freedesktop.timesync1.service
#  SPDX-License-Identifier: LGPL-2.1+
#
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

[Unit]
Description=Network Time Synchronization
Documentation=man:systemd-timesyncd.service(8)
ConditionCapability=CAP_SYS_TIME
ConditionVirtualization=!container
DefaultDependencies=no
After=systemd-remount-fs.service systemd-sysusers.service
Before=time-set.target sysinit.target shutdown.target
Conflicts=shutdown.target
Wants=time-set.target time-sync.target

[Service]
AmbientCapabilities=CAP_SYS_TIME
CapabilityBoundingSet=CAP_SYS_TIME
ExecStart=!!/lib/systemd/systemd-timesyncd
LockPersonality=yes
MemoryDenyWriteExecute=yes
NoNewPrivileges=yes
PrivateDevices=yes
PrivateTmp=yes
ProtectControlGroups=yes
ProtectHome=yes
ProtectHostname=yes
ProtectKernelModules=yes
ProtectKernelTunables=yes
ProtectKernelLogs=yes
ProtectSystem=strict
Restart=always
RestartSec=0
RestrictAddressFamilies=AF_UNIX AF_INET AF_INET6
RestrictNamespaces=yes
RestrictRealtime=yes
RestrictSUIDSGID=yes
RuntimeDirectory=systemd/timesync
StateDirectory=systemd/timesync
SystemCallArchitectures=native
SystemCallErrorNumber=EPERM
SystemCallFilter=@system-service @clock
Type=notify
User=systemd-timesync
WatchdogSec=3min

[Install]
WantedBy=sysinit.target
Alias=dbus-org.freedesktop.timesync1.service
```

### 2. 时间管理服务
#### systemd-timedated.service
```bash
root@am62xx:/# cat /lib/systemd/system/systemd-timedated.service
#  SPDX-License-Identifier: LGPL-2.1+
#
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

[Unit]
Description=Time & Date Service
Documentation=man:systemd-timedated.service(8) man:localtime(5)
Documentation=https://www.freedesktop.org/wiki/Software/systemd/timedated

[Service]
BusName=org.freedesktop.timedate1
CapabilityBoundingSet=CAP_SYS_TIME
DeviceAllow=char-rtc r
ExecStart=/lib/systemd/systemd-timedated
IPAddressDeny=any
LockPersonality=yes
MemoryDenyWriteExecute=yes
NoNewPrivileges=yes
PrivateTmp=yes
ProtectControlGroups=yes
ProtectHome=yes
ProtectHostname=yes
ProtectKernelModules=yes
ProtectKernelTunables=yes
ProtectKernelLogs=yes
ProtectSystem=strict
ReadWritePaths=/etc
RestrictAddressFamilies=AF_UNIX
RestrictNamespaces=yes
RestrictRealtime=yes
RestrictSUIDSGID=yes
SystemCallArchitectures=native
SystemCallErrorNumber=EPERM
SystemCallFilter=@system-service @clock
WatchdogSec=3min
```
- **功能**：  
	- 提供 **D-Bus 接口**，供 `timedatectl` 命令调用。  
	- 负责设置时区、时间、RTC 模式（UTC/Local）及管理 NTP 同步状态。  
- **关键操作**：  
```bash
timedatectl set-time "2025-01-23 12:00:00"   # 设置时间
timedatectl set-timezone Asia/Shanghai       # 设置时区
```

#### systemd-timedate1.service (通过 dbus)
- 这是一个与 systemd-timedated.service 相关的 dbus 服务，用于管理时区、夏令时和硬件时钟。

#### dbus-org.freedesktop.timedate1.service
- **功能**：  
	- D-Bus 接口服务，提供时间与时区管理的 API（由 `systemd-timedated` 实现）。  
	- 主要用于图形界面工具或应用程序通过 D-Bus 调用时间操作。

### 3. 硬件时钟
#### hwclock.service
- 这个服务用于在系统启动时将硬件时钟（RTC）的时间同步到系统时钟，以及在系统关闭时将系统时钟的时间保存到硬件时钟。

### 4. RTC（实时时钟）
#### sync-clocks.service
- 自定义服务，执行 `hwclock --systohc  --utc` 将系统时间同步到硬件时钟。  
```bash
root@am62xx:/# cat /etc/systemd/system/sync-clocks.service
[Unit]
Description=Synchronize System and HW clocks
DefaultDependencies=no
Wants=sysinit.target
Conflicts=shutdown.target
After=getty.target

[Service]
Type=oneshot
ExecStart=/sbin/hwclock --systohc --utc
StandardOutput=syslog

[Install]
WantedBy=multi-user.target
```

### 关键排查建议
1. **检查 `sync-clocks.service`**：  
   - 确保其强制使用 `--utc` 参数写入 RTC
2. **验证 NTP 同步状态**：  
```bash
timedatectl status | grep "System clock synchronized"
```  
3. **检查 RTC 存储模式**：  
```bash
timedatectl | grep "RTC in local TZ" # 应显示 "no"，表示 RTC 存储 UTC 时间。
```