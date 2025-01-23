# 命令集
## systemctl

| **场景**     | **命令**                                              |
| ---------- | --------------------------------------------------- |
| 查看运行中的服务   | `systemctl list-units --type=service`               |
| 查看所有已安装的服务 | `systemctl list-unit-files --type=service`          |
| 查看失败的服务    | `systemctl --failed --type=service`                 |
| 按关键词过滤服务   | `systemctl list-units --type=service \| grep <关键词>` |
| 查看服务详情     | `systemctl status <服务名>`                            |
| 查看服务依赖     | `systemctl list-dependencies <服务名>`                 |
| 查看服务启动日志   | `journalctl -u <服务名>`                               |

### 查看所有已安装的服务（包括未运行的）
```bash
systemctl list-unit-files --type=service
```

```bash
root@am62xx:~# systemctl list-unit-files --type=service
UNIT FILE                              STATE
alsa-restore.service                   static
alsa-state.service                     static
autovt@.service                        enabled
avahi-daemon.service                   enabled
bluetooth.service                      enabled
bt-enable.service                      enabled
camera.service                         disabled
console-getty.service                  disabled
container-getty@.service               static
custom-init.service                    disabled
dbus-1.service                         masked
dbus-org.bluez.service                 enabled
dbus-org.freedesktop.Avahi.service     enabled
dbus-org.freedesktop.hostname1.service static
dbus-org.freedesktop.locale1.service   static
dbus-org.freedesktop.login1.service    static
dbus-org.freedesktop.network1.service  enabled
dbus-org.freedesktop.resolve1.service  enabled
dbus-org.freedesktop.timedate1.service static
dbus-org.freedesktop.timesync1.service enabled
dbus.service                           static
debug-shell.service                    disabled
Demo.service                           generated
dropbear.service                       generated
dropbear@.service                      static
dropbearkey.service                    static
emergency.service                      static
factory-test.service                   disabled
getty@.service                         enabled
gplv3-notice.service                   enabled
HMI.service                            generated
hmi_demo.service                       generated
hostapd.service                        disabled
hwclock.service                        masked
inetd.busybox.service                  generated
initrd-cleanup.service                 static
initrd-parse-etc.service               static
initrd-switch-root.service             static
initrd-udevadm-cleanup-db.service      static
ip6tables.service                      enabled
iptables.service                       enabled
irqbalanced.service                    enabled
kmod-static-nodes.service              static
ldconfig.service                       static
modutils.service                       masked
monitor.service                        disabled
net-config.service                     disabled
netperf.service                        generated
networking.service                     masked
nfs-statd.service                      enabled
nfscommon.service                      masked
ota_breakpoint_download.service        disabled
ota_download.service                   disabled
ota_upgrade.service                    generated
pd_detect.service                      disabled
psplash-start.service                  enabled
psplash-systemd.service                enabled
psplash.service                        generated
ptpd.service                           disabled
quotaon.service                        static
rc-local.service                       static
rc.pvr.service                         generated
rescue.service                         static
resize_rootfs.service                  generated
rpcbind.service                        enabled
run-postinsts.service                  disabled
serial-getty@.service                  indirect
snmpd.service                          enabled
soc-config.service                     disabled
sshd.service                           generated
startwlanap.service                    enabled
startwlansta.service                   enabled
strongswan-starter.service             enabled
sync-clocks.service                    enabled
system-update-cleanup.service          static
systemd-ask-password-console.service   static
systemd-ask-password-wall.service      static
systemd-backlight@.service             static
systemd-boot-check-no-failures.service disabled
systemd-coredump@.service              static
systemd-exit.service                   static
systemd-fsck-root.service              enabled-runtime
systemd-fsck@.service                  static
systemd-halt.service                   static
systemd-hibernate-resume@.service      static
systemd-hibernate.service              static
systemd-hostnamed.service              static
systemd-hwdb-update.service            static
systemd-hybrid-sleep.service           static
systemd-initctl.service                static
systemd-journal-catalog-update.service static
systemd-journal-flush.service          static
systemd-journald.service               static
systemd-kexec.service                  static
systemd-localed.service                static
systemd-logind.service                 static
systemd-machine-id-commit.service      static
systemd-modules-load.service           static
systemd-network-generator.service      disabled
systemd-networkd-wait-online.service   enabled
systemd-networkd.service               enabled
systemd-poweroff.service               static
systemd-pstore.service                 disabled
systemd-quotacheck.service             static
systemd-random-seed.service            static
systemd-reboot.service                 static
systemd-remount-fs.service             enabled-runtime
systemd-resolved.service               enabled
systemd-rfkill.service                 static
systemd-suspend-then-hibernate.service static
systemd-suspend.service                static
systemd-sysctl.service                 static
systemd-sysusers.service               static
systemd-time-wait-sync.service         disabled
systemd-timedated.service              static
systemd-timesyncd.service              enabled
systemd-tmpfiles-clean.service         static
systemd-tmpfiles-setup-dev.service     static
systemd-tmpfiles-setup.service         static
systemd-udev-settle.service            static
systemd-udev-trigger.service           static
systemd-udevd.service                  static
systemd-update-done.service            static
systemd-update-utmp-runlevel.service   static
systemd-update-utmp.service            static
systemd-user-sessions.service          static
systemd-vconsole-setup.service         static
systemd-volatile-root.service          static
tee-supplicant.service                 enabled
telnetd.service                        generated
thermal-zone-init.service              generated
user-runtime-dir@.service              static
user@.service                          static
var-volatile-cache.service             enabled
var-volatile-lib.service               enabled
var-volatile-spool.service             enabled
var-volatile-srv.service               enabled
watchdog.service                       disabled
weston.service                         generated
wpa_supplicant-nl80211@.service        disabled
wpa_supplicant-wired@.service          disabled
wpa_supplicant.service                 disabled
wpa_supplicant@.service                disabled
zy-init.service                        generated

144 unit files listed.
```

### 查看所有正在运行的服务
```bash
systemctl list-units --type=service
```

```bash
root@am62xx:~# systemctl list-units --type=service
  UNIT                                          LOAD   ACTIVE SUB     DESCRIPTION
  alsa-restore.service                          loaded active exited  Save/Restore Sound Card State
  avahi-daemon.service                          loaded active running Avahi mDNS/DNS-SD Stack
  custom-init.service                           loaded active exited  ZY custom init service
  dbus.service                                  loaded active running D-Bus System Message Bus
● factory-test.service                          loaded failed failed  ZY factory test service
  getty@tty1.service                            loaded active running Getty on tty1
  ip6tables.service                             loaded active exited  IPv6 Packet Filtering Framework
  iptables.service                              loaded active exited  IPv4 Packet Filtering Framework
  irqbalanced.service                           loaded active running irqbalance daemon
  kmod-static-nodes.service                     loaded active exited  Create list of static device nodes for the current kernel
  net-config.service                            loaded active exited  ZY net config service
  netperf.service                               loaded active running LSB: network benchmark
  nfs-statd.service                             loaded active running NFS status monitor for NFSv2/3 locking.
  psplash-start.service                         loaded active exited  Start psplash boot splash screen
  psplash-systemd.service                       loaded active exited  Start psplash-systemd progress communication helper
  rc.pvr.service                                loaded active exited  rc.pvr.service
  resize_rootfs.service                         loaded active exited  LSB: Expand Rootfs of boot device
  rpcbind.service                               loaded active running RPC Bind
  serial-getty@ttyS2.service                    loaded active running Serial Getty on ttyS2
  snmpd.service                                 loaded active running Simple Network Management Protocol (SNMP) Daemon.
  soc-config.service                            loaded active exited  soc config service
  startwlanap.service                           loaded active exited  startwlanap
  startwlansta.service                          loaded active exited  startwlansta
  strongswan-starter.service                    loaded active running strongSwan IPsec IKEv1/IKEv2 daemon using ipsec.conf
  systemd-backlight@backlight:backlight.service loaded active exited  Load/Save Screen Backlight Brightness of backlight:backlight
  systemd-fsck@dev-mmcblk0p1.service            loaded active exited  File System Check on /dev/mmcblk0p1
  systemd-fsck@dev-mmcblk0p2.service            loaded active exited  File System Check on /dev/mmcblk0p2
  systemd-fsck@dev-mmcblk0p3.service            loaded active exited  File System Check on /dev/mmcblk0p3
  systemd-fsck@dev-mmcblk1p1.service            loaded active exited  File System Check on /dev/mmcblk1p1
  systemd-journal-flush.service                 loaded active exited  Flush Journal to Persistent Storage
  systemd-journald.service                      loaded active running Journal Service
  systemd-logind.service                        loaded active running Login Service
  systemd-modules-load.service                  loaded active exited  Load Kernel Modules
● systemd-networkd-wait-online.service          loaded failed failed  Wait for Network to be Configured
  systemd-networkd.service                      loaded active running Network Service
  systemd-random-seed.service                   loaded active exited  Load/Save Random Seed
  systemd-remount-fs.service                    loaded active exited  Remount Root and Kernel File Systems
  systemd-resolved.service                      loaded active running Network Name Resolution
  systemd-sysctl.service                        loaded active exited  Apply Kernel Variables
  systemd-timesyncd.service                     loaded active running Network Time Synchronization
  systemd-tmpfiles-setup-dev.service            loaded active exited  Create Static Device Nodes in /dev
  systemd-tmpfiles-setup.service                loaded active exited  Create Volatile Files and Directories
  systemd-udev-trigger.service                  loaded active exited  udev Coldplug all Devices
  systemd-udevd.service                         loaded active running udev Kernel Device Manager
  systemd-update-utmp.service                   loaded active exited  Update UTMP about System Boot/Shutdown
  systemd-user-sessions.service                 loaded active exited  Permit User Sessions
  tee-supplicant.service                        loaded active running TEE Supplicant
  telnetd.service                               loaded active running telnetd.service
  thermal-zone-init.service                     loaded active exited  thermal-zone-init.service
  user-runtime-dir@0.service                    loaded active exited  User Runtime Directory /run/user/0
  user@0.service                                loaded active running User Manager for UID 0
  weston.service                                loaded active exited  weston.service

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.

52 loaded units listed. Pass --all to see loaded but inactive units, too.
To show all installed unit files use 'systemctl list-unit-files'.
```

### 查看失败的服务
```bash
systemctl --failed --type=service
```

```bash
root@am62xx:~# systemctl --failed --type=service
  UNIT                                 LOAD   ACTIVE SUB    DESCRIPTION
● factory-test.service                 loaded failed failed ZY factory test service
● systemd-networkd-wait-online.service loaded failed failed Wait for Network to be Configured

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.

2 loaded units listed.
```

### 按关键词过滤服务
```bash
# 过滤包含 "nginx" 的服务
systemctl list-units --type=service | grep nginx

# 过滤所有已启用的服务
systemctl list-unit-files --type=service --state=enabled
```

### 查看服务的依赖关系
```bash
systemctl list-dependencies <service-name>
```
- **示例**：
  ```bash
  systemctl list-dependencies nginx
  ```

### 高级用法
#### **(1) 显示所有服务（包括未加载的）**
```bash
systemctl list-units --type=service --all
```

```bash
root@am62xx:~# systemctl list-units --type=service --all
  UNIT                                          LOAD      ACTIVE   SUB     DESCRIPTION
  alsa-restore.service                          loaded    active   exited  Save/Restore Sound Card State
  alsa-state.service                            loaded    inactive dead    Manage Sound Card State (restore and store)
● auditd.service                                not-found inactive dead    auditd.service
  avahi-daemon.service                          loaded    active   running Avahi mDNS/DNS-SD Stack
  bt-enable.service                             loaded    inactive dead    Enable and configure wl18xx bluetooth stack
● connman.service                               not-found inactive dead    connman.service
  custom-init.service                           loaded    active   exited  ZY custom init service
  dbus.service                                  loaded    active   running D-Bus System Message Bus
● display-manager.service                       not-found inactive dead    display-manager.service
  dropbear.service                              loaded    inactive dead    LSB: Dropbear Secure Shell server
  dropbear@0.service                            loaded    inactive dead    SSH Per-Connection Server
  dropbearkey.service                           loaded    inactive dead    SSH Key Generation
  emergency.service                             loaded    inactive dead    Emergency Shell
● factory-test.service                          loaded    failed   failed  ZY factory test service
  getty@tty1.service                            loaded    active   running Getty on tty1
  gplv3-notice.service                          loaded    inactive dead    Print notice about GPLv3 packages
  initrd-cleanup.service                        loaded    inactive dead    Cleaning Up and Shutting Down Daemons
  initrd-parse-etc.service                      loaded    inactive dead    Reload Configuration from the Real Root
  initrd-switch-root.service                    loaded    inactive dead    Switch Root
  initrd-udevadm-cleanup-db.service             loaded    inactive dead    Cleanup udevd DB
  ip6tables.service                             loaded    active   exited  IPv6 Packet Filtering Framework
  iptables.service                              loaded    active   exited  IPv4 Packet Filtering Framework
  irqbalanced.service                           loaded    active   running irqbalance daemon
  kmod-static-nodes.service                     loaded    active   exited  Create list of static device nodes for the current kernel
  ldconfig.service                              loaded    inactive dead    Rebuild Dynamic Linker Cache
  net-config.service                            loaded    active   exited  ZY net config service
  netperf.service                               loaded    active   running LSB: network benchmark
  nfs-statd.service                             loaded    active   running NFS status monitor for NFSv2/3 locking.
● plymouth-quit-wait.service                    not-found inactive dead    plymouth-quit-wait.service
● plymouth-start.service                        not-found inactive dead    plymouth-start.service
  psplash-start.service                         loaded    active   exited  Start psplash boot splash screen
  psplash-systemd.service                       loaded    active   exited  Start psplash-systemd progress communication helper
  rc-local.service                              loaded    inactive dead    /etc/rc.local Compatibility
  rc.pvr.service                                loaded    active   exited  rc.pvr.service
  rescue.service                                loaded    inactive dead    Rescue Shell
  resize_rootfs.service                         loaded    active   exited  LSB: Expand Rootfs of boot device
  rpcbind.service                               loaded    active   running RPC Bind
  serial-getty@ttyS2.service                    loaded    active   running Serial Getty on ttyS2
  snmpd.service                                 loaded    active   running Simple Network Management Protocol (SNMP) Daemon.
  soc-config.service                            loaded    active   exited  soc config service
  startwlanap.service                           loaded    active   exited  startwlanap
  startwlansta.service                          loaded    active   exited  startwlansta
  strongswan-starter.service                    loaded    active   running strongSwan IPsec IKEv1/IKEv2 daemon using ipsec.conf
  sync-clocks.service                           loaded    inactive dead    Synchronize System and HW clocks
● syslog.service                                not-found inactive dead    syslog.service
  systemd-ask-password-console.service          loaded    inactive dead    Dispatch Password Requests to Console
  systemd-ask-password-wall.service             loaded    inactive dead    Forward Password Requests to Wall
  systemd-backlight@backlight:backlight.service loaded    active   exited  Load/Save Screen Backlight Brightness of backlight:backlight
  systemd-coredump@0.service                    loaded    inactive dead    Process Core Dump
  systemd-fsck-root.service                     loaded    inactive dead    File System Check on Root Device
  systemd-fsck@dev-mmcblk0p1.service            loaded    active   exited  File System Check on /dev/mmcblk0p1
  systemd-fsck@dev-mmcblk0p2.service            loaded    active   exited  File System Check on /dev/mmcblk0p2
  systemd-fsck@dev-mmcblk0p3.service            loaded    active   exited  File System Check on /dev/mmcblk0p3
  systemd-fsck@dev-mmcblk1p1.service            loaded    active   exited  File System Check on /dev/mmcblk1p1
  systemd-hwdb-update.service                   loaded    inactive dead    Rebuild Hardware Database
  systemd-initctl.service                       loaded    inactive dead    initctl Compatibility Daemon
  systemd-journal-catalog-update.service        loaded    inactive dead    Rebuild Journal Catalog
  systemd-journal-flush.service                 loaded    active   exited  Flush Journal to Persistent Storage
  systemd-journald.service                      loaded    active   running Journal Service
  systemd-logind.service                        loaded    active   running Login Service
  systemd-machine-id-commit.service             loaded    inactive dead    Commit a transient machine-id on disk
  systemd-modules-load.service                  loaded    active   exited  Load Kernel Modules
● systemd-networkd-wait-online.service          loaded    failed   failed  Wait for Network to be Configured
  systemd-networkd.service                      loaded    active   running Network Service
  systemd-quotacheck.service                    loaded    inactive dead    File System Quota Check
  systemd-random-seed.service                   loaded    active   exited  Load/Save Random Seed
  systemd-remount-fs.service                    loaded    active   exited  Remount Root and Kernel File Systems
  systemd-resolved.service                      loaded    active   running Network Name Resolution
  systemd-sysctl.service                        loaded    active   exited  Apply Kernel Variables
  systemd-sysusers.service                      loaded    inactive dead    Create System Users
  systemd-timesyncd.service                     loaded    active   running Network Time Synchronization
  systemd-tmpfiles-clean.service                loaded    inactive dead    Cleanup of Temporary Directories
  systemd-tmpfiles-setup-dev.service            loaded    active   exited  Create Static Device Nodes in /dev
  systemd-tmpfiles-setup.service                loaded    active   exited  Create Volatile Files and Directories
  systemd-udev-settle.service                   loaded    inactive dead    udev Wait for Complete Device Initialization
  systemd-udev-trigger.service                  loaded    active   exited  udev Coldplug all Devices
  systemd-udevd.service                         loaded    active   running udev Kernel Device Manager
  systemd-update-done.service                   loaded    inactive dead    Update is Completed
  systemd-update-utmp-runlevel.service          loaded    inactive dead    Update UTMP about System Runlevel Changes
  systemd-update-utmp.service                   loaded    active   exited  Update UTMP about System Boot/Shutdown
  systemd-user-sessions.service                 loaded    active   exited  Permit User Sessions
  systemd-vconsole-setup.service                loaded    inactive dead    Setup Virtual Console
  tee-supplicant.service                        loaded    active   running TEE Supplicant
  telnetd.service                               loaded    active   running telnetd.service
  thermal-zone-init.service                     loaded    active   exited  thermal-zone-init.service
  user-runtime-dir@0.service                    loaded    active   exited  User Runtime Directory /run/user/0
  user@0.service                                loaded    active   running User Manager for UID 0
  var-volatile-cache.service                    loaded    inactive dead    Bind mount volatile /var/cache
  var-volatile-lib.service                      loaded    inactive dead    Bind mount volatile /var/lib
  var-volatile-spool.service                    loaded    inactive dead    Bind mount volatile /var/spool
  var-volatile-srv.service                      loaded    inactive dead    Bind mount volatile /srv
  weston.service                                loaded    active   exited  weston.service
● weston@root.service                           not-found inactive dead    weston@root.service

LOAD   = Reflects whether the unit definition was properly loaded.
ACTIVE = The high-level unit activation state, i.e. generalization of SUB.
SUB    = The low-level unit activation state, values depend on unit type.

93 loaded units listed.
To show all installed unit files use 'systemctl list-unit-files'.
```

#### **(2) 查看服务的启动时间**
```bash
systemd-analyze blame
```
- **功能**：列出各服务的启动耗时，优化启动速度。

```bash
root@am62xx:~# systemd-analyze blame
2min 239ms systemd-networkd-wait-online.service
   10.324s factory-test.service
    6.874s net-config.service
    6.396s custom-init.service
    3.651s gplv3-notice.service
    1.536s systemd-fsck@dev-mmcblk1p1.service
    1.099s systemd-udev-trigger.service
    1.090s user@0.service
     873ms systemd-resolved.service
     734ms systemd-networkd.service
     718ms snmpd.service
     528ms systemd-logind.service
     477ms ip6tables.service
     455ms iptables.service
     408ms systemd-random-seed.service
     407ms dev-mqueue.mount
     399ms sys-kernel-debug.mount
     397ms dev-hugepages.mount
     391ms tmp.mount
     388ms kmod-static-nodes.service
     367ms avahi-daemon.service
     354ms rc.pvr.service
     351ms systemd-timesyncd.service
     345ms startwlanap.service
     309ms startwlansta.service
     280ms bt-enable.service
     275ms systemd-modules-load.service
     272ms psplash-start.service
     265ms systemd-journald.service
     238ms rpcbind.service
     226ms systemd-remount-fs.service
     224ms systemd-tmpfiles-setup.service
     215ms run-media-mmcblk1p1.mount
     201ms systemd-udevd.service
     199ms systemd-fsck@dev-mmcblk0p3.service
     196ms alsa-restore.service
     191ms systemd-fsck@dev-mmcblk0p1.service
     164ms resize_rootfs.service
     161ms weston.service
     150ms user-runtime-dir@0.service
     146ms run-media-mmcblk0p1.mount
     143ms run-media-mmcblk0p2.mount
     138ms sys-fs-fuse-connections.mount
     138ms systemd-user-sessions.service
     135ms sys-kernel-config.mount
     119ms telnetd.service
     110ms netperf.service
     108ms systemd-sysctl.service
     108ms systemd-journal-flush.service
      95ms run-media-mmcblk0p3.mount
      94ms systemd-fsck@dev-mmcblk0p2.service
      91ms systemd-backlight@backlight:backlight.service
      89ms systemd-update-utmp.service
      89ms systemd-tmpfiles-setup-dev.service
      66ms sync-clocks.service
      64ms systemd-tmpfiles-clean.service
      59ms systemd-update-utmp-runlevel.service
      55ms media-ram.mount
      45ms var-volatile.mount
      30ms thermal-zone-init.service
```

#### **(3) 查看服务的启动日志**
```bash
journalctl -u <service-name>
```
- **示例**：
  ```bash
  journalctl -u ssh
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