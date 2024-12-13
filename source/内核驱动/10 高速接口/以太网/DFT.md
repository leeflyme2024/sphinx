# U-Boot

# Kernel
## 静态IP和动态IP配置
```bash
root@am62xx:~# ll /etc/network/interfaces
-rw-r--r--    1 root     root           781 Jun 16 12:00 /etc/network/interfaces

root@am62xx:~# ll /etc/systemd/network/
-rw-r--r--    1 root     root            99 Jun 16 11:59 01-wlan1-static.network
-rw-r--r--    1 root     root            54 Dec 13  2024 03-eth0-static.network
-rw-r--r--    1 root     root            54 Dec 13  2024 04-eth1-static.network
-rw-r--r--    1 root     root           100 Dec 13  2024 05-eth-dhcp.network
-rw-r--r--    1 root     root            71 Jun 16 10:54 10-eth.network
-rw-r--r--    1 root     root           100 Jun 16 10:54 15-eth.network
-rw-r--r--    1 root     root            39 Jun 16 10:54 30-wlan.network
-rw-r--r--    1 root     root            38 Jun 16 10:54 60-usb.network
root@am62xx:~#
```

### 网络管理方式

在 `systemd` 中，`/etc/network/interfaces` 和 `/etc/systemd/network/` 目录分别用于配置网络的不同方式，主要的区别在于所使用的网络管理工具和配置格式。下面详细解释这两个目录的区别：

1. **`/etc/network/interfaces` 目录**（传统的 `ifupdown` 配置）

- 这是传统的 Debian/Ubuntu 系统中用于配置网络接口的文件，使用的是 `ifupdown` 工具来管理网络。
- 该文件用于静态配置 IP 地址、网络接口的启用与禁用等操作。它使用类似以下格式：
    - `iface eth0 inet dhcp`：表示接口 `eth0` 使用 DHCP 获取 IP 地址。
    - `iface eth0 inet static`：表示接口 `eth0` 使用静态 IP 配置。

**特点**：

- 使用 `ifup` 和 `ifdown` 命令来启用和禁用网络接口。
- 通常适用于老式的基于 `ifupdown` 的网络管理方式。
- 配置相对简单，适用于单一的网络接口配置。

2. **`/etc/systemd/network/` 目录**（`systemd-networkd` 配置）

- 这个目录是 `systemd-networkd` 的配置目录，`systemd-networkd` 是 `systemd` 提供的一种网络管理服务，它负责处理系统的网络配置。该工具更现代，支持更复杂的网络配置需求，并能与 `systemd` 的其他组件深度集成。
- `systemd-networkd` 使用 `.network` 配置文件格式来管理网络接口，每个文件可以配置一个或多个网络接口的设置，通常使用较为细致的配置项，如 `DHCP=yes` 或 `Address=` 等。

**特点**：

- 使用 `.network` 文件来配置网络接口。
- 配置更为灵活，支持多个接口的精细化控制，比如网络接口匹配条件、物理接口属性等。
- 配置更符合现代 `systemd` 管理方式，可以与 `systemd` 的其他功能（如 `systemd-resolved`）无缝集成。
- 使用 `systemd-networkd` 服务启动和管理网络，命令如 `systemctl restart systemd-networkd` 用于重启网络服务。

### 主要区别：

1. **工具和服务**：
    
    - `/etc/network/interfaces` 配置是为 `ifupdown` 工具服务的，适用于传统的网络管理方式。
    - `/etc/systemd/network/` 配置是为 `systemd-networkd` 服务的，适用于基于 `systemd` 的现代网络管理。
2. **配置格式**：
    
    - `/etc/network/interfaces` 使用传统的接口配置格式，如 `iface eth0 inet dhcp`。
    - `/etc/systemd/network/` 使用 `.network` 文件格式，通过 `systemd` 的 `networkd` 服务来处理网络接口配置。
3. **适用场景**：
    
    - `/etc/network/interfaces` 更适用于较老的系统或仍然使用传统网络管理工具的系统。
    - `/etc/systemd/network/` 更适用于现代的基于 `systemd` 的系统，尤其是在复杂的网络管理场景中，比如多接口、静态/动态 IP 配置、连接管理等。
4. **集成性**：
    
    - `/etc/systemd/network/` 更好地与其他 `systemd` 组件（如 `systemd-resolved`、`systemd-timesyncd` 等）集成，提供了更强的灵活性和扩展性。

### 总结：

- `/etc/network/interfaces` 属于传统的 `ifupdown` 网络管理方式，适用于老式的 Linux 配置。
- `/etc/systemd/network/` 属于 `systemd-networkd`，适用于更现代、模块化的网络管理方式，并更好地集成在 `systemd` 生态中。

如果你使用的是基于 `systemd` 的现代发行版（例如最新的 Debian、Ubuntu、CentOS 等），建议使用 `/etc/systemd/network/` 来配置网络。


#### `ifupdown` 网络管理方式

##### 静态IP
```bash
root@am62xx:~# cat /etc/network/interfaces
# /etc/network/interfaces -- configuration file for ifup(8), ifdown(8)

# The loopback interface
auto lo
iface lo inet loopback

# Wireless interfaces
iface wlan0 inet dhcp
        wireless_mode managed
        wireless_essid any
        wpa-driver wext
        wpa-conf /etc/wpa_supplicant.conf

iface tiwlan0 inet dhcp
        wireless_mode managed
        wireless_essid any

iface atml0 inet dhcp

# Wired or wireless interfaces
auto eth0
iface eth0 inet static
    address 192.168.1.166
    netmask 255.255.255.0

auto eth1
iface eth1 inet static
    address 192.168.2.167
    netmask 255.255.255.0


# Ethernet/RNDIS gadget (g_ether)
# ... or on host side, usbnet and random hwaddr
iface usb0 inet dhcp

# Bluetooth networking
iface bnep0 inet dhcp
```
```bash
root@am62xx:~# ifconfig
eth0      Link encap:Ethernet  HWaddr 64:1C:10:28:D6:25
          inet addr:192.168.1.136  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::661c:10ff:fe28:d625/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:9 errors:0 dropped:0 overruns:0 frame:0
          TX packets:77 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2238 (2.1 KiB)  TX bytes:9534 (9.3 KiB)

eth1      Link encap:Ethernet  HWaddr 00:14:97:A3:B9:47
          inet addr:192.168.2.136  Bcast:192.168.2.255  Mask:255.255.255.0
          inet6 addr: fe80::214:97ff:fea3:b947/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:9 errors:0 dropped:0 overruns:0 frame:0
          TX packets:74 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2238 (2.1 KiB)  TX bytes:9328 (9.1 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:82 errors:0 dropped:0 overruns:0 frame:0
          TX packets:82 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:6140 (5.9 KiB)  TX bytes:6140 (5.9 KiB)

wlan0     Link encap:Ethernet  HWaddr 1C:CE:51:6A:E4:15
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

root@am62xx:~# ifdown eth0
ifdown: interface eth0 not configured
root@am62xx:~# ifdown eth1
ifdown: interface eth1 not configured

root@am62xx:~# ifup eth0
root@am62xx:~# ifup eth1

root@am62xx:~# ifconfig
eth0      Link encap:Ethernet  HWaddr 64:1C:10:28:D6:25
          inet addr:192.168.1.166  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::661c:10ff:fe28:d625/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:15 errors:0 dropped:0 overruns:0 frame:0
          TX packets:90 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2706 (2.6 KiB)  TX bytes:11410 (11.1 KiB)

eth1      Link encap:Ethernet  HWaddr 00:14:97:A3:B9:47
          inet addr:192.168.2.167  Bcast:192.168.2.255  Mask:255.255.255.0
          inet6 addr: fe80::214:97ff:fea3:b947/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:15 errors:0 dropped:0 overruns:0 frame:0
          TX packets:88 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2706 (2.6 KiB)  TX bytes:11104 (10.8 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:82 errors:0 dropped:0 overruns:0 frame:0
          TX packets:82 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:6140 (5.9 KiB)  TX bytes:6140 (5.9 KiB)

wlan0     Link encap:Ethernet  HWaddr 1C:CE:51:6A:E4:15
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
```

##### 动态IP
```bash
root@am62xx:~# cat /etc/network/interfaces
# /etc/network/interfaces -- configuration file for ifup(8), ifdown(8)

# The loopback interface
auto lo
iface lo inet loopback

# Wireless interfaces
iface wlan0 inet dhcp
        wireless_mode managed
        wireless_essid any
        wpa-driver wext
        wpa-conf /etc/wpa_supplicant.conf

iface tiwlan0 inet dhcp
        wireless_mode managed
        wireless_essid any

iface atml0 inet dhcp

# Wired or wireless interfaces
auto eth0
iface eth0 inet dhcp
        pre-up /bin/grep -v -e "ip=[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /proc/cmdline > /dev/null
        udhcpc_opts -R -b

iface eth1 inet dhcp
iface eth2 inet dhcp
iface eth3 inet dhcp
iface eth4 inet dhcp

# Ethernet/RNDIS gadget (g_ether)
# ... or on host side, usbnet and random hwaddr
iface usb0 inet dhcp

# Bluetooth networking
iface bnep0 inet dhcp
```

这是一个用于配置网络接口的文件 `/etc/network/interfaces`，适用于 Debian、Ubuntu 以及其他基于 `ifupdown` 工具的 Linux 系统。文件中的配置项定义了如何启用和管理各种网络接口，下面是各部分的详细中文解释：

1. **回环接口 (Loopback Interface)**

```bash
auto lo
iface lo inet loopback
```

- `auto lo`：表示在系统启动时自动启用回环接口 `lo`（即 `127.0.0.1`）。
- `iface lo inet loopback`：配置回环接口的网络协议为 `loopback`，用于内部通信。

2. **无线接口 (Wireless Interfaces)**

```bash
iface wlan0 inet dhcp
        wireless_mode managed
        wireless_essid any
        wpa-driver wext
        wpa-conf /etc/wpa_supplicant.conf
```

- `iface wlan0 inet dhcp`：配置无线接口 `wlan0` 使用 DHCP 获取 IP 地址。
- `wireless_mode managed`：将无线模式设置为 `managed`，即客户端模式。
- `wireless_essid any`：允许连接到任何 SSID 的无线网络。
- `wpa-driver wext`：使用 `wext` 驱动程序来管理 WPA 加密。
- `wpa-conf /etc/wpa_supplicant.conf`：指定 WPA 配置文件，通常包含无线网络的密码和加密信息。

```bash
iface tiwlan0 inet dhcp
        wireless_mode managed
        wireless_essid any
```

- 类似上面，配置另一个无线接口 `tiwlan0`，并启用 DHCP。

```bash
iface atml0 inet dhcp
```

- 配置一个 ATM 接口 `atml0`，并使用 DHCP 获取 IP 地址。

3. **有线或无线接口 (Wired or Wireless Interfaces)**

```bash
auto eth0
iface eth0 inet dhcp
        pre-up /bin/grep -v -e "ip=[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /proc/cmdline > /dev/null
        udhcpc_opts -R -b
```

- `auto eth0`：表示在启动时自动启用 `eth0` 接口。
- `iface eth0 inet dhcp`：配置有线接口 `eth0` 使用 DHCP 获取 IP 地址。
- `pre-up`：在接口启用之前执行一个命令，命令会过滤掉内核命令行中的 `ip` 配置项，避免干扰 DHCP 配置。
- `udhcpc_opts -R -b`：为 DHCP 客户端 `udhcpc` 设置选项，`-R` 用于请求续租，`-b` 使其在后台运行。

```bash
iface eth1 inet dhcp
iface eth2 inet dhcp
iface eth3 inet dhcp
iface eth4 inet dhcp
```

- 配置 `eth1` 到 `eth4` 接口，均使用 DHCP 获取 IP 地址。

4. **以太网/RNDIS Gadget (Ethernet/RNDIS Gadget)**

```bash
iface usb0 inet dhcp
```

- 配置 USB 以太网接口 `usb0` 使用 DHCP 获取 IP 地址。通常用于通过 USB 连接共享网络。

5. **蓝牙网络 (Bluetooth Networking)**

```bash
iface bnep0 inet dhcp
```

- 配置蓝牙网络接口 `bnep0` 使用 DHCP 获取 IP 地址。`bnep` 是 Bluetooth Network Encapsulation Protocol，用于蓝牙设备之间的网络连接。

总结：

- 文件通过定义 `iface` 配置来为系统的各种网络接口（包括无线、有线、USB 和蓝牙）配置 DHCP。
- 每个接口在启动时会自动获取 IP 地址，并根据需要设置特定的网络参数（如无线模式、SSID、驱动等）。
- 文件中还定义了一些特殊接口，如 USB 网络接口和蓝牙网络接口。

```bash
root@am62xx:~# ifconfig
eth0      Link encap:Ethernet  HWaddr 64:1C:10:28:D6:25
          inet addr:192.168.10.3  Bcast:192.168.10.255  Mask:255.255.255.0
          inet6 addr: fe80::661c:10ff:fe28:d625/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:8 errors:0 dropped:0 overruns:0 frame:0
          TX packets:66 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:3718 (3.6 KiB)  TX bytes:8985 (8.7 KiB)

eth1      Link encap:Ethernet  HWaddr 00:14:97:A3:B9:47
          inet addr:192.168.10.10  Bcast:192.168.10.255  Mask:255.255.255.0
          inet6 addr: fe80::214:97ff:fea3:b947/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:12 errors:0 dropped:0 overruns:0 frame:0
          TX packets:70 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:4550 (4.4 KiB)  TX bytes:9824 (9.5 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:82 errors:0 dropped:0 overruns:0 frame:0
          TX packets:82 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:6140 (5.9 KiB)  TX bytes:6140 (5.9 KiB)

wlan0     Link encap:Ethernet  HWaddr 1C:CE:51:6A:E4:15
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

root@am62xx:~#
root@am62xx:~#
root@am62xx:~#
root@am62xx:~#
root@am62xx:~# cat /etc/network/interfaces
# /etc/network/interfaces -- configuration file for ifup(8), ifdown(8)

# The loopback interface
auto lo
iface lo inet loopback

# Wireless interfaces
iface wlan0 inet dhcp
        wireless_mode managed
        wireless_essid any
        wpa-driver wext
        wpa-conf /etc/wpa_supplicant.conf

iface tiwlan0 inet dhcp
        wireless_mode managed
        wireless_essid any

iface atml0 inet dhcp

# Wired or wireless interfaces
auto eth0
iface eth0 inet static
    address 192.168.1.166
    netmask 255.255.255.0

auto eth1
iface eth1 inet static
    address 192.168.2.167
    netmask 255.255.255.0

# # dhcp config for wired interfaces
# auto eth0
# iface eth0 inet dhcp
#         pre-up /bin/grep -v -e "ip=[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /proc/cmdline > /dev/null
#         udhcpc_opts -R -b

# iface eth1 inet dhcp


# Ethernet/RNDIS gadget (g_ether)
# ... or on host side, usbnet and random hwaddr
iface usb0 inet dhcp

# Bluetooth networking
iface bnep0 inet dhcp

root@am62xx:~#
root@am62xx:~#
root@am62xx:~#
root@am62xx:~# ifdown eth0
ifdown: interface eth0 not configured
root@am62xx:~#
root@am62xx:~# ifdown eth1
ifdown: interface eth1 not configured
root@am62xx:~#
root@am62xx:~# ifup eth0
root@am62xx:~#
root@am62xx:~# ifup eth1
root@am62xx:~#
root@am62xx:~# ifconfig
eth0      Link encap:Ethernet  HWaddr 64:1C:10:28:D6:25
          inet addr:192.168.1.166  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::661c:10ff:fe28:d625/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:20 errors:0 dropped:0 overruns:0 frame:0
          TX packets:91 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:4662 (4.5 KiB)  TX bytes:12074 (11.7 KiB)

eth1      Link encap:Ethernet  HWaddr 00:14:97:A3:B9:47
          inet addr:192.168.2.167  Bcast:192.168.2.255  Mask:255.255.255.0
          inet6 addr: fe80::214:97ff:fea3:b947/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:21 errors:0 dropped:0 overruns:0 frame:0
          TX packets:90 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:5198 (5.0 KiB)  TX bytes:12560 (12.2 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:82 errors:0 dropped:0 overruns:0 frame:0
          TX packets:82 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:6140 (5.9 KiB)  TX bytes:6140 (5.9 KiB)

wlan0     Link encap:Ethernet  HWaddr 1C:CE:51:6A:E4:15
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)
```

```bash
root@am62xx:~# ifconfig
eth0      Link encap:Ethernet  HWaddr 64:1C:10:28:D6:25
          inet addr:192.168.10.3  Bcast:192.168.10.255  Mask:255.255.255.0
          inet6 addr: fe80::661c:10ff:fe28:d625/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:7 errors:0 dropped:0 overruns:0 frame:0
          TX packets:71 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:2598 (2.5 KiB)  TX bytes:9886 (9.6 KiB)

eth1      Link encap:Ethernet  HWaddr 00:14:97:A3:B9:47
          inet addr:192.168.10.10  Bcast:192.168.10.255  Mask:255.255.255.0
          inet6 addr: fe80::214:97ff:fea3:b947/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:11 errors:0 dropped:0 overruns:0 frame:0
          TX packets:75 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:3430 (3.3 KiB)  TX bytes:10426 (10.1 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:82 errors:0 dropped:0 overruns:0 frame:0
          TX packets:82 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:6140 (5.9 KiB)  TX bytes:6140 (5.9 KiB)

wlan0     Link encap:Ethernet  HWaddr 1C:CE:51:6A:E4:15
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

root@am62xx:~#
root@am62xx:~#
root@am62xx:~# ifdown eth0
ifdown: interface eth0 not configured
root@am62xx:~#
root@am62xx:~# ifdown eth1
ifdown: interface eth1 not configured
root@am62xx:~#
root@am62xx:~# ifup eth0
udhcpc: started, v1.31.1
udhcpc: sending discover
udhcpc: sending select for 192.168.10.3
udhcpc: lease of 192.168.10.3 obtained, lease time 864000
/etc/udhcpc.d/50default: Adding DNS 192.168.10.1
root@am62xx:~#
root@am62xx:~#
root@am62xx:~# ifup eth1
udhcpc: started, v1.31.1
udhcpc: sending discover
udhcpc: sending select for 192.168.10.10
udhcpc: lease of 192.168.10.10 obtained, lease time 864000
RTNETLINK answers: File exists
/etc/udhcpc.d/50default: Adding DNS 192.168.10.1
root@am62xx:~#
root@am62xx:~#
root@am62xx:~# ifconfig
eth0      Link encap:Ethernet  HWaddr 64:1C:10:28:D6:25
          inet addr:192.168.10.3  Bcast:192.168.10.255  Mask:255.255.255.0
          inet6 addr: fe80::661c:10ff:fe28:d625/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:27 errors:0 dropped:0 overruns:0 frame:0
          TX packets:100 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:6792 (6.6 KiB)  TX bytes:13666 (13.3 KiB)

eth1      Link encap:Ethernet  HWaddr 00:14:97:A3:B9:47
          inet addr:192.168.10.10  Bcast:192.168.10.255  Mask:255.255.255.0
          inet6 addr: fe80::214:97ff:fea3:b947/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:24 errors:0 dropped:0 overruns:0 frame:0
          TX packets:96 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:6968 (6.8 KiB)  TX bytes:13638 (13.3 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:82 errors:0 dropped:0 overruns:0 frame:0
          TX packets:82 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:6140 (5.9 KiB)  TX bytes:6140 (5.9 KiB)

wlan0     Link encap:Ethernet  HWaddr 1C:CE:51:6A:E4:15
          UP BROADCAST MULTICAST  MTU:1500  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

root@am62xx:~#
root@am62xx:~# cat /etc/network/interfaces
# /etc/network/interfaces -- configuration file for ifup(8), ifdown(8)

# The loopback interface
auto lo
iface lo inet loopback

# Wireless interfaces
iface wlan0 inet dhcp
        wireless_mode managed
        wireless_essid any
        wpa-driver wext
        wpa-conf /etc/wpa_supplicant.conf

iface tiwlan0 inet dhcp
        wireless_mode managed
        wireless_essid any

iface atml0 inet dhcp

# Wired or wireless interfaces
# dhcp config for wired interfaces
auto eth0
iface eth0 inet dhcp
        pre-up /bin/grep -v -e "ip=[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+" /proc/cmdline > /dev/null
        udhcpc_opts -R -b

iface eth1 inet dhcp


# Ethernet/RNDIS gadget (g_ether)
# ... or on host side, usbnet and random hwaddr
iface usb0 inet dhcp

# Bluetooth networking
iface bnep0 inet dhcp
```

#### systemd-networkd

```bash
root@am62xx:~# systemctl status systemd-networkd.service
● systemd-networkd.service - Network Service
     Loaded: loaded (/lib/systemd/system/systemd-networkd.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2023-10-21 17:07:06 UTC; 1min 52s ago
TriggeredBy: ● systemd-networkd.socket
       Docs: man:systemd-networkd.service(8)
   Main PID: 641 (systemd-network)
     Status: "Processing requests..."
      Tasks: 1 (limit: 1104)
     Memory: 1.4M
     CGroup: /system.slice/systemd-networkd.service
             └─641 /lib/systemd/systemd-networkd

Oct 21 17:07:19 am62xx systemd-networkd[641]: eth0: DHCPv4 address 192.168.10.3/24 via 192.168.10.1
Oct 21 17:07:19 am62xx systemd-networkd[641]: eth1: Gained carrier
Oct 21 17:07:19 am62xx systemd-networkd[641]: eth1: DHCPv4 address 192.168.10.10/24 via 192.168.10.1
Oct 21 17:07:20 am62xx systemd-networkd[641]: can0: Interface name change detected, can0 has been renamed to mcan2.
Oct 21 17:07:20 am62xx systemd-networkd[641]: can2: Interface name change detected, can2 has been renamed to mcan1.
Oct 21 17:07:20 am62xx systemd-networkd[641]: can1: Interface name change detected, can1 has been renamed to mcan0.
Oct 21 17:07:20 am62xx systemd-networkd[641]: wlan0: IPv6 successfully enabled
Oct 21 17:07:20 am62xx systemd-networkd[641]: wlan0: Link UP
Oct 21 17:07:21 am62xx systemd-networkd[641]: eth1: Gained IPv6LL
Oct 21 17:07:21 am62xx systemd-networkd[641]: eth0: Gained IPv6LL


root@am62xx:~# journalctl |grep systemd-networkd
Oct 21 17:07:05 am62xx systemd-networkd[641]: /etc/systemd/network/01-wlan1-static.network:5: An address '192.168.43.1' is specified without prefix length. The behavior of parsing addresses without prefix length will be changed in the future release. Please specify prefix length explicitly.
Oct 21 17:07:05 am62xx systemd-networkd[641]: /etc/systemd/network/01-wlan1-static.network:6: Unknown key name 'Broadcast' in section 'Network', ignoring.
Oct 21 17:07:05 am62xx systemd-networkd[641]: /etc/systemd/network/01-wlan1-static.network:7: Unknown key name 'Netmask' in section 'Network', ignoring.
Oct 21 17:07:06 am62xx systemd-networkd[641]: Enumeration completed
Oct 21 17:07:06 am62xx systemd-networkd[641]: eth1: IPv6 successfully enabled
Oct 21 17:07:06 am62xx systemd-networkd[641]: eth0: IPv6 successfully enabled
Oct 21 17:07:06 am62xx systemd-networkd[641]: eth1: Link UP
Oct 21 17:07:06 am62xx systemd-networkd[641]: eth0: Link UP
Oct 21 17:07:09 am62xx systemd-networkd[641]: eth1: Gained carrier
Oct 21 17:07:09 am62xx systemd-networkd[641]: eth0: Gained carrier
Oct 21 17:07:10 am62xx systemd-networkd[641]: eth1: Gained IPv6LL
Oct 21 17:07:11 am62xx systemd-networkd[641]: eth0: Gained IPv6LL
Oct 21 17:07:11 am62xx systemd-networkd[641]: eth1: DHCPv4 address 192.168.10.17/24 via 192.168.10.1
Oct 21 17:07:11 am62xx systemd-networkd[641]: eth0: DHCPv4 address 192.168.10.3/24 via 192.168.10.1
Oct 21 17:07:15 am62xx systemd-networkd[641]: eth0: Link DOWN
Oct 21 17:07:15 am62xx systemd-networkd[641]: eth0: Lost carrier
Oct 21 17:07:15 am62xx systemd-networkd[641]: eth0: DHCP lease lost
Oct 21 17:07:16 am62xx systemd-networkd[641]: eth0: Link UP
Oct 21 17:07:16 am62xx systemd-networkd[641]: eth1: Link DOWN
Oct 21 17:07:16 am62xx systemd-networkd[641]: eth1: Lost carrier
Oct 21 17:07:16 am62xx systemd-networkd[641]: eth1: DHCP lease lost
Oct 21 17:07:16 am62xx systemd-networkd[641]: eth1: Link UP
Oct 21 17:07:16 am62xx systemd-networkd[641]: eth1: Gained carrier
Oct 21 17:07:16 am62xx systemd-networkd[641]: eth1: Lost carrier
Oct 21 17:07:19 am62xx systemd-networkd[641]: eth0: Gained carrier
Oct 21 17:07:19 am62xx systemd-networkd[641]: eth0: DHCPv4 address 192.168.10.3/24 via 192.168.10.1
Oct 21 17:07:19 am62xx systemd-networkd[641]: eth1: Gained carrier
Oct 21 17:07:19 am62xx systemd-networkd[641]: eth1: DHCPv4 address 192.168.10.10/24 via 192.168.10.1
Oct 21 17:07:20 am62xx systemd-networkd[641]: can0: Interface name change detected, can0 has been renamed to mcan2.
Oct 21 17:07:20 am62xx systemd-networkd[641]: can2: Interface name change detected, can2 has been renamed to mcan1.
Oct 21 17:07:20 am62xx systemd-networkd[641]: can1: Interface name change detected, can1 has been renamed to mcan0.
Oct 21 17:07:20 am62xx systemd-networkd[641]: wlan0: IPv6 successfully enabled
Oct 21 17:07:20 am62xx systemd-networkd[641]: wlan0: Link UP
Oct 21 17:07:21 am62xx systemd-networkd[641]: eth1: Gained IPv6LL
Oct 21 17:07:21 am62xx systemd-networkd[641]: eth0: Gained IPv6LL

```

##### 静态IP
```bash
root@am62xx:~# cat /etc/systemd/network/03-eth0-static.network
[Match]
Name=eth0

[Network]
Address=192.168.1.136/24


root@am62xx:~# cat /etc/systemd/network/04-eth1-static.network
[Match]
Name=eth1

[Network]
Address=192.168.2.136/24
```

##### 动态IP

```bash
root@am62xx:~# cat /etc/systemd/network/05-eth-dhcp.network
[Match]
Name=eth*
KernelCommandLine=!root=/dev/nfs

[Link]
RequiredForOnline=no

[Network]
DHCP=yes
```
