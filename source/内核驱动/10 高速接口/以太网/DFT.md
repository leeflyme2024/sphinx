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

## 调试log
### systemd
''' bash
``` bash
systemctl --failed
systemctl status poweroff-rtc-sync.service
systemctl status poweron-rtc-sync.service
systemctl status sync-clocks.service
systemctl status logrotate.service
systemctl status ntp-set.service
systemctl status run-media-mmcblk0.mount
systemctl status net-config.service

systemctl status ota_breakpoint_download.service
systemctl status ota_download.service
systemctl status pd_detect.service

journalctl |grep hwclock

journalctl |grep avahi-daemon
journalctl |grep systemd-networkd
journalctl |grep systemd-udevd
systemctl restart systemd-networkd
systemctl restart systemd-resolved
systemctl status systemd-networkd -l


systemctl stop networking
systemctl disable networking

systemctl stop NetworkManager
systemctl disable NetworkManager


cat /etc/systemd/network/05-eth-dhcp.network
cat /etc/systemd/network/10-eth.network
cat /etc/resolv.conf
cat /etc/systemd/resolved.conf


ifconfig
networkctl status eth0
ip addr show eth0
ip route show
ethtool eth0


journalctl -u avahi-daemon
journalctl -u systemd-networkd
journalctl -u systemd-resolved

journalctl -f -u avahi-daemon
journalctl -f -u systemd-networkd
journalctl -f -u systemd-resolved

udhcpc -i eth0 -f -v
resolvectl dns eth0 8.8.8.8 8.8.4.4
resolvectl status
resolvectl query www.baidu.com
systemctl restart systemd-resolved
ln -sf /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
ll /etc/resolv.conf

networkctl list
networkctl status
tcpdump -i eth0 port 67 or port 68 -v

ping 8.8.8.8

/etc/netplan/
netplan apply

systemctl list-units | grep -E 'network|dhcp'


ip addr add 192.168.1.100/24 dev eth0
ip route add default via 192.168.1.1

# 设置日志级别为debug
mkdir -p /etc/systemd/system/systemd-networkd.service.d/
cat > /etc/systemd/system/systemd-networkd.service.d/debug.conf << EOF
[Service]
Environment=SYSTEMD_LOG_LEVEL=debug
EOF

# 为 avahi-daemon 创建调试配置
mkdir -p /etc/systemd/system/avahi-daemon.service.d/
cat > /etc/systemd/system/avahi-daemon.service.d/debug.conf << EOF
[Service]
Environment=SYSTEMD_LOG_LEVEL=debug
EOF

# 为 systemd-resolved 创建调试配置
mkdir -p /etc/systemd/system/systemd-resolved.service.d/
cat > /etc/systemd/system/systemd-resolved.service.d/debug.conf << EOF
[Service]
Environment=SYSTEMD_LOG_LEVEL=debug
EOF

# 重新加载 systemd 配置
systemctl daemon-reload

# 重启相关服务
systemctl restart avahi-daemon
systemctl restart systemd-networkd
systemctl restart systemd-resolved

# 查看详细日志
journalctl -u systemd-networkd -f

systemctl list-units --type=service --state=running
systemctl list-unit-files --state=enabled
systemctl list-units --type=service
```

#### debug.conf
```bash
root@am62xx:~# cat /etc/systemd/system/avahi-daemon.service.d/debug.conf
[Service]
Environment=SYSTEMD_LOG_LEVEL=debug

root@am62xx:~# cat /etc/systemd/system/systemd-networkd.service.d/debug.conf
[Service]
Environment=SYSTEMD_LOG_LEVEL=debug

root@am62xx:~#
root@am62xx:~# cat /etc/systemd/system/systemd-resolved.service.d/debug.conf
[Service]
Environment=SYSTEMD_LOG_LEVEL=debug


root@am62xx:~# systemctl show -p LogLevel
LogLevel=info

root@am62xx:~# systemctl log-level
info

root@am62xx:~# systemctl log-level debug
root@am62xx:~#
root@am62xx:~# systemctl log-level
debug

root@am62xx:~# cat /etc/systemd/system.conf |grep LogLevel -C 5
# Defaults can be restored by simply deleting this file.
#
# See systemd-system.conf(5) for details.

[Manager]
LogLevel=debug
#LogTarget=journal-or-kmsg
#LogColor=yes
#LogLocation=no
#DumpCore=yes
#ShowStatus=yes
```

在 `systemd` 中，**`debug`** 级别产生的信息最多。这是因为在所有日志级别中，`debug` 级别会记录最详细的调试信息，包括系统内部的操作和状态变化，这对于开发人员和系统管理员进行故障排查非常有用。

以下是各个日志级别的简要说明，从产生信息最少到最多排序：

1. **emerg**（紧急）：仅记录系统不可用的紧急情况。
2. **alert**（警告）：必须立即采取措施的情况。
3. **crit**（严重）：关键条件，表示严重的错误。
4. **err**（错误）：其他运行错误。
5. **warning**（警告）：可能影响系统正常运行的警告信息。
6. **notice**（注意）：正常但重要的事件。
7. **info**（信息）：确认操作的状态信息，通常用于了解系统的常规操作。
8. **debug**（调试）：详细的调试信息，包含大量的细节，有助于深入分析和调试问题。

因此，如果你希望获取尽可能多的日志信息来帮助诊断问题或理解系统的行为，应该将日志级别设置为 **`debug`**。

例如，在你的配置文件 `/etc/systemd/system.conf` 中，可以看到当前设置为：
```ini
LogLevel=debug
```
这表明系统将记录最高量的日志信息。如果你想临时调整日志级别，可以使用以下命令：
```sh
systemctl log-level debug
```
或者如果你想减少日志输出量，可以将其设置为较低的级别，比如 `info` 或 `notice`：
```sh
systemctl log-level info
```
或
```sh
systemctl log-level notice
```

记得在完成调试后，可以根据需要将日志级别恢复到一个更合适的值，以避免生成过多的日志数据。

#### 02-eth-dhcp.network
```bash
root@am62xx:~# ll /etc/systemd/network
-rw-r--r--    1 root     root            99 Jun 16  2023 01-wlan1-static.network
-rw-r--r--    1 root     root           100 Jan  9 05:17 02-eth-dhcp.network
-rw-r--r--    1 root     root            54 Jan  9 05:17 03-eth0-static.network
-rw-r--r--    1 root     root            54 Jan  9 05:17 04-eth1-static.network
-rw-r--r--    1 root     root            71 Jun 16  2023 10-eth.network
-rw-r--r--    1 root     root           100 Jun 16  2023 15-eth.network
-rw-r--r--    1 root     root            39 Jun 16  2023 30-wlan.network
-rw-r--r--    1 root     root            38 Jun 16  2023 60-usb.network
root@am62xx:~#
root@am62xx:~# cat /etc/systemd/network/02-eth-dhcp.network
[Match]
Name=eth*
KernelCommandLine=!root=/dev/nfs

[Link]
RequiredForOnline=no

[Network]
DHCP=yes
```

#### resolv.conf
```bash
root@am62xx:~# cat /etc/resolv.conf
# This file is managed by man:systemd-resolved(8). Do not edit.
#
# This is a dynamic resolv.conf file for connecting local clients directly to
# all known uplink DNS servers. This file lists all configured search domains.
#
# Third party programs must not access this file directly, but only through the
# symlink at /etc/resolv.conf. To manage man:resolv.conf(5) in a different way,
# replace this symlink by a static file or a different symlink.
#
# See man:systemd-resolved.service(8) for details about the supported modes of
# operation for /etc/resolv.conf.

nameserver 192.168.1.2

root@am62xx:~# cat /etc/systemd/resolved.conf
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
# See resolved.conf(5) for details

[Resolve]
#DNS=
#FallbackDNS=1.1.1.1 8.8.8.8 1.0.0.1 8.8.4.4 2606:4700:4700::1111 2001:4860:4860::8888 2606:4700:4700::1001 2001:4860:4860::8844
#Domains=
#LLMNR=yes
#MulticastDNS=yes
#DNSSEC=no
#DNSOverTLS=no
#Cache=yes
#DNSStubListener=yes
#ReadEtcHosts=yes

root@am62xx:~# ll /etc/resolv.conf
lrwxrwxrwx    1 root     root            24 Jun 17  2023 /etc/resolv.conf -> /etc/resolv-conf.systemd



root@am62xx:~# cat /run/systemd/resolve/stub-resolv.conf
# This file is managed by man:systemd-resolved(8). Do not edit.
#
# This is a dynamic resolv.conf file for connecting local clients to the
# internal DNS stub resolver of systemd-resolved. This file lists all
# configured search domains.
#
# Run "resolvectl status" to see details about the uplink DNS servers
# currently in use.
#
# Third party programs must not access this file directly, but only through the
# symlink at /etc/resolv.conf. To manage man:resolv.conf(5) in a different way,
# replace this symlink by a static file or a different symlink.
#
# See man:systemd-resolved.service(8) for details about the supported modes of
# operation for /etc/resolv.conf.

nameserver 127.0.0.53
options edns0
```

```bash
root@am62xx:~# cat /etc/resolv.conf
# This file is managed by man:systemd-resolved(8). Do not edit.
#
# This is a dynamic resolv.conf file for connecting local clients directly to
# all known uplink DNS servers. This file lists all configured search domains.
#
# Third party programs must not access this file directly, but only through the
# symlink at /etc/resolv.conf. To manage man:resolv.conf(5) in a different way,
# replace this symlink by a static file or a different symlink.
#
# See man:systemd-resolved.service(8) for details about the supported modes of
# operation for /etc/resolv.conf.

nameserver 1.1.1.1
nameserver 8.8.8.8
nameserver 1.0.0.1
# Too many DNS servers configured, the following entries may be ignored.
nameserver 8.8.4.4
nameserver 2606:4700:4700::1111
nameserver 2001:4860:4860::8888
nameserver 2606:4700:4700::1001
nameserver 2001:4860:4860::8844
```

#### ifconfig
```bash
root@am62xx:~# ifconfig
eth0      Link encap:Ethernet  HWaddr 28:B5:E8:E2:41:D3
          inet addr:192.168.1.171  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::2ab5:e8ff:fee2:41d3/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1551 errors:0 dropped:0 overruns:0 frame:0
          TX packets:1628 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:176425 (172.2 KiB)  TX bytes:176104 (171.9 KiB)

root@am62xx:~# ip addr show eth0
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 28:b5:e8:e2:41:d3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.171/24 brd 192.168.1.255 scope global dynamic eth0
       valid_lft 42427sec preferred_lft 42427sec
    inet6 fe80::2ab5:e8ff:fee2:41d3/64 scope link
       valid_lft forever preferred_lft forever

root@am62xx:~# ip route show
default via 192.168.1.2 dev eth0 proto dhcp src 192.168.1.171 metric 1024
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.171
192.168.1.2 dev eth0 proto dhcp scope link src 192.168.1.171 metric 1024
```

#### ethtool
```bash
root@am62xx:~# ethtool eth0
Settings for eth0:
        Supported ports: [ TP    MII ]
        Supported link modes:   10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Full
        Supported pause frame use: Symmetric
        Supports auto-negotiation: Yes
        Supported FEC modes: Not reported
        Advertised link modes:  10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Full
        Advertised pause frame use: Symmetric
        Advertised auto-negotiation: Yes
        Advertised FEC modes: Not reported
        Link partner advertised link modes:  10baseT/Half 10baseT/Full
                                             100baseT/Half 100baseT/Full
                                             1000baseT/Full
        Link partner advertised pause frame use: Symmetric Receive-only
        Link partner advertised auto-negotiation: Yes
        Link partner advertised FEC modes: Not reported
        Speed: 1000Mb/s
        Duplex: Full
        Auto-negotiation: on
        Port: Twisted Pair
        PHYAD: 5
        Transceiver: external
        MDI-X: Unknown
        Supports Wake-on: g
        Wake-on: d
        Current message level: 0x000020f7 (8439)
                               drv probe link ifdown ifup rx_err tx_err hw
        Link detected: yes
```

#### networkctl
```bash
root@am62xx:~# networkctl status eth0
● 2: eth0
             Link File: /lib/systemd/network/99-default.link
          Network File: /etc/systemd/network/02-eth-dhcp.network
                  Type: ether
                 State: routable (configured)
                  Path: platform-8000000.ethernet
                Driver: am65-cpsw-nuss
            HW Address: 28:b5:e8:e2:41:d3
                   MTU: 1500 (min: 64, max: 1522)
  Queue Length (Tx/Rx): 8/1
      Auto negotiation: yes
                 Speed: 1Gbps
                Duplex: full
                  Port: tp
               Address: 192.168.1.171 (DHCP4)
                        fe80::2ab5:e8ff:fee2:41d3
               Gateway: 192.168.1.2
                   DNS: 192.168.1.2
          Connected To: n/a on port 58:47:ca:77:65:25
```

#### avahi-daemon
```bash

root@am62xx:~# journalctl -u avahi-daemon
-- Logs begin at Thu 2025-01-09 06:15:19 UTC, end at Thu 2025-01-09 06:31:04 UTC. --
Jan 09 06:15:22 am62xx systemd[1]: Starting Avahi mDNS/DNS-SD Stack...
Jan 09 06:15:22 am62xx avahi-daemon[563]: Found user 'avahi' (UID 996) and group 'avahi' (GID 996).
Jan 09 06:15:22 am62xx avahi-daemon[563]: Successfully dropped root privileges.
Jan 09 06:15:22 am62xx avahi-daemon[563]: avahi-daemon 0.7 starting up.
Jan 09 06:15:22 am62xx avahi-daemon[563]: Successfully called chroot().
Jan 09 06:15:22 am62xx avahi-daemon[563]: Successfully dropped remaining capabilities.
Jan 09 06:15:22 am62xx avahi-daemon[563]: No service file found in /etc/avahi/services.
Jan 09 06:15:22 am62xx avahi-daemon[563]: Network interface enumeration completed.
Jan 09 06:15:22 am62xx avahi-daemon[563]: Server startup complete. Host name is am62xx.local. Local service cookie is 3078474092.
Jan 09 06:15:22 am62xx systemd[1]: Started Avahi mDNS/DNS-SD Stack.
Jan 09 06:15:24 am62xx avahi-daemon[563]: Joining mDNS multicast group on interface eth0.IPv4 with address 192.168.1.171.
Jan 09 06:15:24 am62xx avahi-daemon[563]: New relevant interface eth0.IPv4 for mDNS.
Jan 09 06:15:24 am62xx avahi-daemon[563]: Registering new address record for 192.168.1.171 on eth0.IPv4.
Jan 09 06:15:25 am62xx avahi-daemon[563]: Joining mDNS multicast group on interface eth0.IPv6 with address fe80::2ab5:e8ff:fee2:41d3.
Jan 09 06:15:25 am62xx avahi-daemon[563]: New relevant interface eth0.IPv6 for mDNS.
Jan 09 06:15:25 am62xx avahi-daemon[563]: Registering new address record for fe80::2ab5:e8ff:fee2:41d3 on eth0.*.
Jan 09 06:15:36 am62xx avahi-daemon[563]: Interface eth0.IPv6 no longer relevant for mDNS.
Jan 09 06:15:36 am62xx avahi-daemon[563]: Leaving mDNS multicast group on interface eth0.IPv6 with address fe80::2ab5:e8ff:fee2:41d3.
Jan 09 06:15:36 am62xx avahi-daemon[563]: Interface eth0.IPv4 no longer relevant for mDNS.
Jan 09 06:15:36 am62xx avahi-daemon[563]: Leaving mDNS multicast group on interface eth0.IPv4 with address 192.168.1.171.
Jan 09 06:15:36 am62xx avahi-daemon[563]: Withdrawing address record for fe80::2ab5:e8ff:fee2:41d3 on eth0.
Jan 09 06:15:36 am62xx avahi-daemon[563]: Withdrawing address record for 192.168.1.171 on eth0.
Jan 09 06:15:39 am62xx avahi-daemon[563]: Joining mDNS multicast group on interface eth0.IPv4 with address 192.168.1.171.
Jan 09 06:15:39 am62xx avahi-daemon[563]: New relevant interface eth0.IPv4 for mDNS.
Jan 09 06:15:39 am62xx avahi-daemon[563]: Registering new address record for 192.168.1.171 on eth0.IPv4.
Jan 09 06:15:40 am62xx avahi-daemon[563]: Joining mDNS multicast group on interface eth0.IPv6 with address fe80::2ab5:e8ff:fee2:41d3.
Jan 09 06:15:40 am62xx avahi-daemon[563]: New relevant interface eth0.IPv6 for mDNS.
Jan 09 06:15:40 am62xx avahi-daemon[563]: Registering new address record for fe80::2ab5:e8ff:fee2:41d3 on eth0.*.
```

#### systemd-networkd
```bash

root@am62xx:~# journalctl -u systemd-networkd
-- Logs begin at Thu 2025-01-09 06:15:19 UTC, end at Thu 2025-01-09 06:31:54 UTC. --
Jan 09 06:15:20 am62xx systemd[1]: Starting Network Service...
Jan 09 06:15:20 am62xx systemd-networkd[535]: /etc/systemd/network/01-wlan1-static.network:5: An address '192.168.43.1' is specified without prefix length. The behavior of parsing addresses without prefix length will be changed in>
Jan 09 06:15:20 am62xx systemd-networkd[535]: /etc/systemd/network/01-wlan1-static.network:6: Unknown key name 'Broadcast' in section 'Network', ignoring.
Jan 09 06:15:20 am62xx systemd-networkd[535]: /etc/systemd/network/01-wlan1-static.network:7: Unknown key name 'Netmask' in section 'Network', ignoring.
Jan 09 06:15:20 am62xx systemd-networkd[535]: Enumeration completed
Jan 09 06:15:20 am62xx systemd-networkd[535]: eth0: IPv6 successfully enabled
Jan 09 06:15:21 am62xx systemd[1]: Started Network Service.
Jan 09 06:15:21 am62xx systemd-networkd[535]: eth0: Link UP
Jan 09 06:15:24 am62xx systemd-networkd[535]: eth0: Gained carrier
Jan 09 06:15:24 am62xx systemd-networkd[535]: eth0: DHCPv4 address 192.168.1.171/24 via 192.168.1.2
Jan 09 06:15:25 am62xx systemd-networkd[535]: eth0: Gained IPv6LL
Jan 09 06:15:36 am62xx systemd-networkd[535]: eth0: Link DOWN
Jan 09 06:15:36 am62xx systemd-networkd[535]: eth0: Lost carrier
Jan 09 06:15:36 am62xx systemd-networkd[535]: eth0: DHCP lease lost
Jan 09 06:15:36 am62xx systemd-networkd[535]: eth0: Link UP
Jan 09 06:15:39 am62xx systemd-networkd[535]: eth0: Gained carrier
Jan 09 06:15:39 am62xx systemd-networkd[535]: eth0: DHCPv4 address 192.168.1.171/24 via 192.168.1.2
Jan 09 06:15:40 am62xx systemd-networkd[535]: eth0: Gained IPv6LL
Jan 09 06:15:43 am62xx systemd-networkd[535]: can0: Interface name change detected, can0 has been renamed to mcan0.
Jan 09 06:15:43 am62xx systemd-networkd[535]: can1: Interface name change detected, can1 has been renamed to mcan1.
Jan 09 06:15:43 am62xx systemd-networkd[535]: can2: Interface name change detected, can2 has been renamed to mcan2.
```

```bash
root@am62xx:~# journalctl -u systemd-networkd
-- Logs begin at Thu 2025-01-09 06:46:20 UTC, end at Thu 2025-01-09 06:52:48 UTC. --
Jan 09 06:46:21 am62xx systemd[1]: Starting Network Service...
Jan 09 06:46:21 am62xx systemd-networkd[533]: Bus bus-api-network: changing state UNSET → OPENING
Jan 09 06:46:21 am62xx systemd-networkd[533]: Bus bus-api-network: changing state OPENING → AUTHENTICATING
Jan 09 06:46:21 am62xx systemd-networkd[533]: timestamp of '/etc/systemd/network' changed
Jan 09 06:46:21 am62xx systemd-networkd[533]: /etc/systemd/network/01-wlan1-static.network:5: An address '192.168.43.1' is specified without prefix length. The behavior of parsing addresses without prefix length will be changed in>
Jan 09 06:46:21 am62xx systemd-networkd[533]: /etc/systemd/network/01-wlan1-static.network:6: Unknown key name 'Broadcast' in section 'Network', ignoring.
Jan 09 06:46:21 am62xx systemd-networkd[533]: /etc/systemd/network/01-wlan1-static.network:7: Unknown key name 'Netmask' in section 'Network', ignoring.
Jan 09 06:46:22 am62xx systemd-networkd[533]: No virtualization found in DMI
Jan 09 06:46:22 am62xx systemd-networkd[533]: No virtualization found in CPUID
Jan 09 06:46:22 am62xx systemd-networkd[533]: Virtualization XEN not found, /proc/xen does not exist
Jan 09 06:46:22 am62xx systemd-networkd[533]: No virtualization found in /proc/device-tree/*
Jan 09 06:46:22 am62xx systemd-networkd[533]: UML virtualization not found in /proc/cpuinfo.
Jan 09 06:46:22 am62xx systemd-networkd[533]: This platform does not support /proc/sysinfo
Jan 09 06:46:22 am62xx systemd-networkd[533]: Found VM virtualization none
Jan 09 06:46:22 am62xx systemd-networkd[533]: /lib/systemd/network/80-container-host0.network: Conditions in the file do not match the system environment, skipping.
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: New device has no master, continuing without
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: Flags change: +MULTICAST +BROADCAST
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: Link 2 added
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: udev initialized link
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: State changed: pending -> initialized
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: Saved original MTU: 1500
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: New device has no master, continuing without
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Flags change: +LOOPBACK +UP +LOWER_UP +RUNNING
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Link 1 added
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: udev initialized link
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: State changed: pending -> initialized
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Saved original MTU: 65536
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Remembering foreign address: ::1/128 (valid forever)
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Remembering foreign address: 127.0.0.1/8 (valid forever)
Jan 09 06:46:22 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Remembering route: dst: ::1/128, src: n/a, gw: n/a, prefsrc: n/a, scope: global, table: main, proto: kernel, type: unicast
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Remembering route: dst: ::1/128, src: n/a, gw: n/a, prefsrc: n/a, scope: global, table: main, proto: kernel, type: local
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Remembering route: dst: 127.255.255.255/32, src: n/a, gw: n/a, prefsrc: 127.0.0.1, scope: link, table: local, proto: kernel, type: broadcast
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Remembering route: dst: 127.0.0.1/32, src: n/a, gw: n/a, prefsrc: 127.0.0.1, scope: host, table: local, proto: kernel, type: local
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Remembering route: dst: 127.0.0.0/8, src: n/a, gw: n/a, prefsrc: 127.0.0.1, scope: host, table: local, proto: kernel, type: local
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Remembering route: dst: 127.0.0.0/32, src: n/a, gw: n/a, prefsrc: 127.0.0.1, scope: link, table: local, proto: kernel, type: broadcast
Jan 09 06:46:22 am62xx systemd-networkd[533]: FIB Rules are not supported by the kernel. Ignoring.
Jan 09 06:46:22 am62xx systemd-networkd[533]: Enumeration completed
Jan 09 06:46:22 am62xx systemd-networkd[533]: Bus bus-api-network: changing state AUTHENTICATING → HELLO
Jan 09 06:46:22 am62xx systemd-networkd[533]: Sent message type=method_call sender=n/a destination=org.freedesktop.DBus path=/org/freedesktop/DBus interface=org.freedesktop.DBus member=Hello cookie=1 reply_cookie=0 signature=n/a e>
Jan 09 06:46:22 am62xx systemd-networkd[533]: Sent message type=method_call sender=n/a destination=org.freedesktop.DBus path=/org/freedesktop/DBus interface=org.freedesktop.DBus member=RequestName cookie=2 reply_cookie=0 signature>
Jan 09 06:46:22 am62xx systemd-networkd[533]: Sent message type=method_call sender=n/a destination=org.freedesktop.DBus path=/org/freedesktop/DBus interface=org.freedesktop.DBus member=AddMatch cookie=3 reply_cookie=0 signature=s >
Jan 09 06:46:22 am62xx systemd[1]: Started Network Service.
Jan 09 06:46:22 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=4 reply_cookie=0 s>
Jan 09 06:46:22 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_31 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=5 reply_cookie=0 s>
Jan 09 06:46:22 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_31 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=6 reply_cookie=0 s>
Jan 09 06:46:22 am62xx systemd-networkd[533]: Got message type=method_return sender=org.freedesktop.DBus destination=:1.3 path=n/a interface=n/a member=n/a cookie=1 reply_cookie=1 signature=s error-name=n/a error-message=n/a
Jan 09 06:46:22 am62xx systemd-networkd[533]: Bus bus-api-network: changing state HELLO → RUNNING
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: Link state is up-to-date
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: found matching network '/etc/systemd/network/02-eth-dhcp.network'
Jan 09 06:46:22 am62xx systemd-networkd[533]: Setting '/proc/sys/net/ipv6/conf/eth0/disable_ipv6' to '0'
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: IPv6 successfully enabled
Jan 09 06:46:22 am62xx systemd-networkd[533]: Setting '/proc/sys/net/ipv6/conf/eth0/proxy_ndp' to '0'
Jan 09 06:46:22 am62xx systemd-networkd[533]: Setting '/proc/sys/net/ipv6/conf/eth0/use_tempaddr' to '0'
Jan 09 06:46:22 am62xx systemd-networkd[533]: Setting '/proc/sys/net/ipv6/conf/eth0/accept_ra' to '0'
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: Setting address genmode for link
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: State changed: initialized -> configuring
Jan 09 06:46:22 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=7 reply_cookie=0 s>
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: Bringing link up
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: Link state is up-to-date
Jan 09 06:46:22 am62xx systemd-networkd[533]: lo: State changed: initialized -> unmanaged
Jan 09 06:46:22 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_31 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=8 reply_cookie=0 s>
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: Flags change: +UP +RUNNING
Jan 09 06:46:22 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=9 reply_cookie=0 s>
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: Link UP
Jan 09 06:46:22 am62xx systemd-networkd[533]: LLDP: Started LLDP client
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: Started LLDP.
Jan 09 06:46:22 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=10 reply_cookie=0 signature>
Jan 09 06:46:22 am62xx systemd-networkd[533]: eth0: Flags change: -RUNNING
Jan 09 06:46:22 am62xx systemd-networkd[533]: Got message type=signal sender=org.freedesktop.DBus.Local destination=n/a path=/org/freedesktop/DBus/Local interface=org.freedesktop.DBus.Local member=Connected cookie=4294967295 reply>
Jan 09 06:46:22 am62xx systemd-networkd[533]: Got message type=signal sender=org.freedesktop.DBus destination=:1.3 path=/org/freedesktop/DBus interface=org.freedesktop.DBus member=NameAcquired cookie=2 reply_cookie=0 signature=s e>
Jan 09 06:46:22 am62xx systemd-networkd[533]: Got message type=signal sender=org.freedesktop.DBus destination=:1.3 path=/org/freedesktop/DBus interface=org.freedesktop.DBus member=NameAcquired cookie=3 reply_cookie=0 signature=s e>
Jan 09 06:46:22 am62xx systemd-networkd[533]: Got message type=method_return sender=org.freedesktop.DBus destination=:1.3 path=n/a interface=n/a member=n/a cookie=4 reply_cookie=2 signature=u error-name=n/a error-message=n/a
Jan 09 06:46:22 am62xx systemd-networkd[533]: Successfully acquired requested service name.
Jan 09 06:46:22 am62xx systemd-networkd[533]: Got message type=method_return sender=org.freedesktop.DBus destination=:1.3 path=n/a interface=n/a member=n/a cookie=5 reply_cookie=3 signature=n/a error-name=n/a error-message=n/a
Jan 09 06:46:22 am62xx systemd-networkd[533]: Match type='signal',sender='org.freedesktop.login1',path='/org/freedesktop/login1',interface='org.freedesktop.login1.Manager',member='PrepareForSleep' successfully installed.
Jan 09 06:46:26 am62xx systemd-networkd[533]: can0: New device has no master, continuing without
Jan 09 06:46:26 am62xx systemd-networkd[533]: can0: MAC address not found for new device, continuing without
Jan 09 06:46:26 am62xx systemd-networkd[533]: can0: Flags change: +NOARP +ECHO
Jan 09 06:46:26 am62xx systemd-networkd[533]: can0: Link 3 added
Jan 09 06:46:26 am62xx systemd-networkd[533]: can0: link pending udev initialization...
Jan 09 06:46:26 am62xx systemd-networkd[533]: can0: Saved original MTU: 16
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Remembering route: dst: ff00::/8, src: n/a, gw: n/a, prefsrc: n/a, scope: global, table: main, proto: kernel, type: multicast
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Remembering route: dst: fe80::/64, src: n/a, gw: n/a, prefsrc: n/a, scope: global, table: main, proto: kernel, type: unicast
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Flags change: +LOWER_UP +RUNNING
Jan 09 06:46:26 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=11 reply_cookie=0 >
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Gained carrier
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Acquiring DHCPv4 lease
Jan 09 06:46:26 am62xx systemd-networkd[533]: DHCP CLIENT (0x3c2b8452): STARTED on ifindex 2
Jan 09 06:46:26 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=12 reply_cookie=0 signature>
Jan 09 06:46:26 am62xx systemd-networkd[533]: DHCP CLIENT (0x3c2b8452): DISCOVER
Jan 09 06:46:26 am62xx systemd-networkd[533]: DHCP CLIENT (0x3c2b8452): OFFER
Jan 09 06:46:26 am62xx systemd-networkd[533]: DHCP CLIENT (0x3c2b8452): REQUEST (requesting)
Jan 09 06:46:26 am62xx systemd-networkd[533]: can1: New device has no master, continuing without
Jan 09 06:46:26 am62xx systemd-networkd[533]: can1: MAC address not found for new device, continuing without
Jan 09 06:46:26 am62xx systemd-networkd[533]: can1: Flags change: +NOARP +ECHO
Jan 09 06:46:26 am62xx systemd-networkd[533]: can1: Link 4 added
Jan 09 06:46:26 am62xx systemd-networkd[533]: can1: link pending udev initialization...
Jan 09 06:46:26 am62xx systemd-networkd[533]: can1: Saved original MTU: 16
Jan 09 06:46:26 am62xx systemd-networkd[533]: DHCP CLIENT (0x3c2b8452): ACK
Jan 09 06:46:26 am62xx systemd-networkd[533]: DHCP CLIENT (0x3c2b8452): lease expires in 11h 59min 57s
Jan 09 06:46:26 am62xx systemd-networkd[533]: DHCP CLIENT (0x3c2b8452): T2 expires in 10h 29min 57s
Jan 09 06:46:26 am62xx systemd-networkd[533]: DHCP CLIENT (0x3c2b8452): T1 expires in 5h 59min 58s
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: DHCPv4 address 192.168.1.171/24 via 192.168.1.2
Jan 09 06:46:26 am62xx systemd-networkd[533]: Setting transient hostname: 'am62xx'
Jan 09 06:46:26 am62xx systemd-networkd[533]: Sent message type=method_call sender=n/a destination=org.freedesktop.hostname1 path=/org/freedesktop/hostname1 interface=org.freedesktop.hostname1 member=SetHostname cookie=13 reply_co>
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Remembering updated address: 192.168.1.171/24 (valid for 12h)
Jan 09 06:46:26 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=14 reply_cookie=0 >
Jan 09 06:46:26 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=15 reply_cookie=0 signature>
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Remembering route: dst: 192.168.1.171/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: host, table: local, proto: kernel, type: local
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Remembering route: dst: 192.168.1.255/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: local, proto: kernel, type: broadcast
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Remembering route: dst: 192.168.1.0/24, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: main, proto: kernel, type: unicast
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Remembering route: dst: 192.168.1.0/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: local, proto: kernel, type: broadcast
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: DHCP: No routes received from DHCP server: No data available
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Configuring route: dst: 192.168.1.2/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: main, proto: dhcp, type: unicast
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Configuring route: dst: n/a, src: n/a, gw: 192.168.1.2, prefsrc: 192.168.1.171, scope: global, table: main, proto: dhcp, type: unicast
Jan 09 06:46:26 am62xx systemd-networkd[533]: can2: New device has no master, continuing without
Jan 09 06:46:26 am62xx systemd-networkd[533]: can2: MAC address not found for new device, continuing without
Jan 09 06:46:26 am62xx systemd-networkd[533]: can2: Flags change: +NOARP +ECHO
Jan 09 06:46:26 am62xx systemd-networkd[533]: can2: Link 5 added
Jan 09 06:46:26 am62xx systemd-networkd[533]: can2: link pending udev initialization...
Jan 09 06:46:26 am62xx systemd-networkd[533]: can2: Saved original MTU: 16
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Received remembered route: dst: 192.168.1.2/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: main, proto: dhcp, type: unicast
Jan 09 06:46:26 am62xx systemd-networkd[533]: eth0: Received remembered route: dst: n/a, src: n/a, gw: 192.168.1.2, prefsrc: 192.168.1.171, scope: global, table: main, proto: dhcp, type: unicast
Jan 09 06:46:27 am62xx systemd-networkd[533]: Got message type=method_return sender=:1.8 destination=:1.3 path=n/a interface=n/a member=n/a cookie=3 reply_cookie=13 signature=n/a error-name=n/a error-message=n/a
Jan 09 06:46:28 am62xx systemd-networkd[533]: eth0: Remembering foreign address: fe80::2ab5:e8ff:fee2:41d3/64 (valid forever)
Jan 09 06:46:28 am62xx systemd-networkd[533]: eth0: Gained IPv6LL
Jan 09 06:46:28 am62xx systemd-networkd[533]: eth0: Discovering IPv6 routers
Jan 09 06:46:28 am62xx systemd-networkd[533]: NDISC: Started IPv6 Router Solicitation client
Jan 09 06:46:28 am62xx systemd-networkd[533]: eth0: Remembering route: dst: fe80::2ab5:e8ff:fee2:41d3/128, src: n/a, gw: n/a, prefsrc: n/a, scope: global, table: main, proto: kernel, type: local
Jan 09 06:46:29 am62xx systemd-networkd[533]: NDISC: Sent Router Solicitation, next solicitation in 3s
Jan 09 06:46:31 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:33 am62xx systemd-networkd[533]: NDISC: Sent Router Solicitation, next solicitation in 7s
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Flags change: -UP -LOWER_UP -RUNNING
Jan 09 06:46:37 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=16 reply_cookie=0 >
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Link DOWN
Jan 09 06:46:37 am62xx systemd-networkd[533]: LLDP: Stopping LLDP client
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Stopped LLDP.
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Lost carrier
Jan 09 06:46:37 am62xx systemd-networkd[533]: DHCP CLIENT (0x3c2b8452): STOPPED
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: DHCP lease lost
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Removing route: dst: 192.168.1.2/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: main, proto: dhcp, type: unicast
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Removing route: dst: n/a, src: n/a, gw: 192.168.1.2, prefsrc: 192.168.1.171, scope: global, table: main, proto: dhcp, type: unicast
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Removing address 192.168.1.171
Jan 09 06:46:37 am62xx systemd-networkd[533]: Setting transient hostname: 'n/a'
Jan 09 06:46:37 am62xx systemd-networkd[533]: Sent message type=method_call sender=n/a destination=org.freedesktop.hostname1 path=/org/freedesktop/hostname1 interface=org.freedesktop.hostname1 member=SetHostname cookie=17 reply_co>
Jan 09 06:46:37 am62xx systemd-networkd[533]: NDISC: Stopping IPv6 Router Solicitation client
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Removing address 192.168.1.171
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Removing route: dst: 192.168.1.2/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: main, proto: dhcp, type: unicast
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Removing route: dst: n/a, src: n/a, gw: 192.168.1.2, prefsrc: 192.168.1.171, scope: global, table: main, proto: dhcp, type: unicast
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: State is configuring, dropping config
Jan 09 06:46:37 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=18 reply_cookie=0 signature>
Jan 09 06:46:37 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:37 am62xx systemd-networkd[533]: Got message type=method_return sender=:1.8 destination=:1.3 path=n/a interface=n/a member=n/a cookie=4 reply_cookie=17 signature=n/a error-name=n/a error-message=n/a
Jan 09 06:46:37 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:37 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:37 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Forgetting route: dst: fe80::2ab5:e8ff:fee2:41d3/128, src: n/a, gw: n/a, prefsrc: n/a, scope: global, table: main, proto: kernel, type: local
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Forgetting route: dst: fe80::/64, src: n/a, gw: n/a, prefsrc: n/a, scope: global, table: main, proto: kernel, type: unicast
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Forgetting route: dst: ff00::/8, src: n/a, gw: n/a, prefsrc: n/a, scope: global, table: main, proto: kernel, type: multicast
Jan 09 06:46:37 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:37 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:37 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:37 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:37 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Forgetting address: fe80::2ab5:e8ff:fee2:41d3/64 (valid forever)
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Forgetting address: 192.168.1.171/24 (valid for 11h 59min 49s)
Jan 09 06:46:37 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=19 reply_cookie=0 >
Jan 09 06:46:37 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=20 reply_cookie=0 signature>
Jan 09 06:46:37 am62xx systemd-networkd[533]: eth0: Forgetting route: dst: 192.168.1.171/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: host, table: local, proto: kernel, type: local
Jan 09 06:46:38 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:38 am62xx systemd-networkd[533]: eth0: Flags change: +UP
Jan 09 06:46:38 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=21 reply_cookie=0 >
Jan 09 06:46:38 am62xx systemd-networkd[533]: eth0: Link UP
Jan 09 06:46:38 am62xx systemd-networkd[533]: LLDP: Started LLDP client
Jan 09 06:46:38 am62xx systemd-networkd[533]: eth0: Started LLDP.
Jan 09 06:46:38 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=22 reply_cookie=0 signature>
Jan 09 06:46:40 am62xx systemd-networkd[533]: eth0: Remembering route: dst: ff00::/8, src: n/a, gw: n/a, prefsrc: n/a, scope: global, table: main, proto: kernel, type: multicast
Jan 09 06:46:40 am62xx systemd-networkd[533]: eth0: Remembering route: dst: fe80::/64, src: n/a, gw: n/a, prefsrc: n/a, scope: global, table: main, proto: kernel, type: unicast
Jan 09 06:46:40 am62xx systemd-networkd[533]: eth0: Flags change: +LOWER_UP +RUNNING
Jan 09 06:46:40 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=23 reply_cookie=0 >
Jan 09 06:46:40 am62xx systemd-networkd[533]: eth0: Gained carrier
Jan 09 06:46:40 am62xx systemd-networkd[533]: eth0: Acquiring DHCPv4 lease
Jan 09 06:46:40 am62xx systemd-networkd[533]: DHCP CLIENT (0x100aa640): STARTED on ifindex 2
Jan 09 06:46:40 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=24 reply_cookie=0 signature>
Jan 09 06:46:40 am62xx systemd-networkd[533]: DHCP CLIENT (0x100aa640): REQUEST (init-reboot)
Jan 09 06:46:41 am62xx systemd-networkd[533]: eth0: Remembering foreign address: fe80::2ab5:e8ff:fee2:41d3/64 (valid forever)
Jan 09 06:46:41 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=25 reply_cookie=0 >
Jan 09 06:46:41 am62xx systemd-networkd[533]: eth0: Gained IPv6LL
Jan 09 06:46:41 am62xx systemd-networkd[533]: eth0: Discovering IPv6 routers
Jan 09 06:46:41 am62xx systemd-networkd[533]: NDISC: Started IPv6 Router Solicitation client
Jan 09 06:46:41 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=26 reply_cookie=0 signature>
Jan 09 06:46:41 am62xx systemd-networkd[533]: eth0: Remembering route: dst: fe80::2ab5:e8ff:fee2:41d3/128, src: n/a, gw: n/a, prefsrc: n/a, scope: global, table: main, proto: kernel, type: local
Jan 09 06:46:42 am62xx systemd-networkd[533]: NDISC: Sent Router Solicitation, next solicitation in 4s
Jan 09 06:46:42 am62xx systemd-networkd[533]: DHCP CLIENT (0xf51373f8): REBOOTED
Jan 09 06:46:42 am62xx systemd-networkd[533]: DHCP CLIENT (0xf51373f8): DISCOVER
Jan 09 06:46:42 am62xx systemd-networkd[533]: DHCP CLIENT (0xf51373f8): OFFER
Jan 09 06:46:42 am62xx systemd-networkd[533]: DHCP CLIENT (0xf51373f8): REQUEST (requesting)
Jan 09 06:46:42 am62xx systemd-networkd[533]: DHCP CLIENT (0xf51373f8): ACK
Jan 09 06:46:42 am62xx systemd-networkd[533]: DHCP CLIENT (0xf51373f8): lease expires in 11h 59min 58s
Jan 09 06:46:42 am62xx systemd-networkd[533]: DHCP CLIENT (0xf51373f8): T2 expires in 10h 29min 57s
Jan 09 06:46:42 am62xx systemd-networkd[533]: DHCP CLIENT (0xf51373f8): T1 expires in 5h 59min 58s
Jan 09 06:46:42 am62xx systemd-networkd[533]: eth0: DHCPv4 address 192.168.1.171/24 via 192.168.1.2
Jan 09 06:46:42 am62xx systemd-networkd[533]: Setting transient hostname: 'am62xx'
Jan 09 06:46:42 am62xx systemd-networkd[533]: Sent message type=method_call sender=n/a destination=org.freedesktop.hostname1 path=/org/freedesktop/hostname1 interface=org.freedesktop.hostname1 member=SetHostname cookie=27 reply_co>
Jan 09 06:46:42 am62xx systemd-networkd[533]: Got message type=method_return sender=:1.8 destination=:1.3 path=n/a interface=n/a member=n/a cookie=5 reply_cookie=27 signature=n/a error-name=n/a error-message=n/a
Jan 09 06:46:42 am62xx systemd-networkd[533]: eth0: Remembering updated address: 192.168.1.171/24 (valid for 12h)
Jan 09 06:46:42 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=28 reply_cookie=0 >
Jan 09 06:46:42 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=29 reply_cookie=0 signature>
Jan 09 06:46:42 am62xx systemd-networkd[533]: eth0: Remembering route: dst: 192.168.1.171/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: host, table: local, proto: kernel, type: local
Jan 09 06:46:42 am62xx systemd-networkd[533]: eth0: Received remembered route: dst: 192.168.1.255/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: local, proto: kernel, type: broadcast
Jan 09 06:46:42 am62xx systemd-networkd[533]: eth0: Received remembered route: dst: 192.168.1.0/24, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: main, proto: kernel, type: unicast
Jan 09 06:46:42 am62xx systemd-networkd[533]: eth0: Received remembered route: dst: 192.168.1.0/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: local, proto: kernel, type: broadcast
Jan 09 06:46:42 am62xx systemd-networkd[533]: eth0: DHCP: No routes received from DHCP server: No data available
Jan 09 06:46:42 am62xx systemd-networkd[533]: eth0: Configuring route: dst: 192.168.1.2/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: main, proto: dhcp, type: unicast
Jan 09 06:46:42 am62xx systemd-networkd[533]: eth0: Configuring route: dst: n/a, src: n/a, gw: 192.168.1.2, prefsrc: 192.168.1.171, scope: global, table: main, proto: dhcp, type: unicast
Jan 09 06:46:42 am62xx systemd-networkd[533]: eth0: Received remembered route: dst: 192.168.1.2/32, src: n/a, gw: n/a, prefsrc: 192.168.1.171, scope: link, table: main, proto: dhcp, type: unicast
Jan 09 06:46:42 am62xx systemd-networkd[533]: eth0: Received remembered route: dst: n/a, src: n/a, gw: 192.168.1.2, prefsrc: 192.168.1.171, scope: global, table: main, proto: dhcp, type: unicast
Jan 09 06:46:43 am62xx systemd-networkd[533]: rtnl: received non-static neighbor, ignoring.
Jan 09 06:46:44 am62xx systemd-networkd[533]: can1: Interface name change detected, can1 has been renamed to mcan1.
Jan 09 06:46:44 am62xx systemd-networkd[533]: can1: State changed: pending -> linger
Jan 09 06:46:44 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_34 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=30 reply_cookie=0 >
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan1: New device has no master, continuing without
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan1: MAC address not found for new device, continuing without
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan1: Flags change: +NOARP +ECHO
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan1: Link 4 added
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan1: link pending udev initialization...
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan1: Saved original MTU: 16
Jan 09 06:46:44 am62xx systemd-networkd[533]: can0: Interface name change detected, can0 has been renamed to mcan0.
Jan 09 06:46:44 am62xx systemd-networkd[533]: can0: State changed: pending -> linger
Jan 09 06:46:44 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_33 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=31 reply_cookie=0 >
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan0: New device has no master, continuing without
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan0: MAC address not found for new device, continuing without
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan0: Flags change: +NOARP +ECHO
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan0: Link 3 added
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan0: link pending udev initialization...
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan0: Saved original MTU: 16
Jan 09 06:46:44 am62xx systemd-networkd[533]: can2: Interface name change detected, can2 has been renamed to mcan2.
Jan 09 06:46:44 am62xx systemd-networkd[533]: can2: State changed: pending -> linger
Jan 09 06:46:44 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_35 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=32 reply_cookie=0 >
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan2: New device has no master, continuing without
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan2: MAC address not found for new device, continuing without
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan2: Flags change: +NOARP +ECHO
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan2: Link 5 added
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan2: link pending udev initialization...
Jan 09 06:46:44 am62xx systemd-networkd[533]: mcan2: Saved original MTU: 16
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan1: Interface is under renaming, wait for the interface to be renamed: Success
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan0: Interface is under renaming, wait for the interface to be renamed: Success
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan1: udev initialized link
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan1: State changed: pending -> initialized
Jan 09 06:46:45 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_34 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=33 reply_cookie=0 >
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan1: Link state is up-to-date
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan1: State changed: initialized -> unmanaged
Jan 09 06:46:45 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_34 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=34 reply_cookie=0 >
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan0: udev initialized link
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan0: State changed: pending -> initialized
Jan 09 06:46:45 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_33 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=35 reply_cookie=0 >
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan0: Link state is up-to-date
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan0: State changed: initialized -> unmanaged
Jan 09 06:46:45 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_33 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=36 reply_cookie=0 >
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan2: Interface is under renaming, wait for the interface to be renamed: Success
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan2: udev initialized link
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan2: State changed: pending -> initialized
Jan 09 06:46:45 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_35 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=37 reply_cookie=0 >
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan2: Link state is up-to-date
Jan 09 06:46:45 am62xx systemd-networkd[533]: mcan2: State changed: initialized -> unmanaged
Jan 09 06:46:45 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_35 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=38 reply_cookie=0 >
Jan 09 06:46:46 am62xx systemd-networkd[533]: NDISC: Sent Router Solicitation, next solicitation in 8s
Jan 09 06:46:53 am62xx systemd-networkd[533]: NDISC: No RA received before link confirmation timeout
Jan 09 06:46:53 am62xx systemd-networkd[533]: NDISC: Invoking callback for 'timeout' event.
Jan 09 06:46:53 am62xx systemd-networkd[533]: eth0: State changed: configuring -> configured
Jan 09 06:46:53 am62xx systemd-networkd[533]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=39 reply_cookie=0 >
Jan 09 06:46:54 am62xx systemd-networkd[533]: NDISC: Sent Router Solicitation, next solicitation in 16s
Jan 09 06:47:11 am62xx systemd-networkd[533]: NDISC: Sent Router Solicitation, next solicitation in 31s
Jan 09 06:47:42 am62xx systemd-networkd[533]: NDISC: Sent Router Solicitation, next solicitation in 1min 4s
Jan 09 06:48:47 am62xx systemd-networkd[533]: NDISC: Sent Router Solicitation, next solicitation in 2min 6s
Jan 09 06:52:36 am62xx systemd-networkd[533]: Got message type=method_call sender=:1.10 destination=org.freedesktop.network1 path=/org/freedesktop/network1/link/_32 interface=org.freedesktop.DBus.Properties member=Get cookie=2 rep>
Jan 09 06:52:36 am62xx systemd-networkd[533]: Sent message type=method_return sender=n/a destination=:1.10 path=n/a interface=n/a member=n/a cookie=40 reply_cookie=2 signature=v error-name=n/a error-message=n/a
root@am62xx:~#
```

#### systemd-resolved
```bash
root@am62xx:~# journalctl -u systemd-resolved
-- Logs begin at Thu 2025-01-09 06:15:19 UTC, end at Thu 2025-01-09 06:32:27 UTC. --
Jan 09 06:15:21 am62xx systemd[1]: Starting Network Name Resolution...
Jan 09 06:15:22 am62xx systemd-resolved[560]: Positive Trust Anchors:
Jan 09 06:15:22 am62xx systemd-resolved[560]: . IN DS 20326 8 2 e06d44b80b8f1d39a95c0b0d7c65d08458e880409bbc683457104237c7f8ec8d
Jan 09 06:15:22 am62xx systemd-resolved[560]: Negative trust anchors: 10.in-addr.arpa 16.172.in-addr.arpa 17.172.in-addr.arpa 18.172.in-addr.arpa 19.172.in-addr.arpa 20.172.in-addr.arpa 21.172.in-addr.arpa 22.172.in-addr.arpa 23.1>
Jan 09 06:15:22 am62xx systemd-resolved[560]: Using system hostname 'am62xx'.
Jan 09 06:15:22 am62xx systemd[1]: Started Network Name Resolution.
```

```bash
root@am62xx:~# journalctl -u systemd-resolved
-- Logs begin at Thu 2025-01-09 07:00:08 UTC, end at Thu 2025-01-09 07:01:57 UTC. --
Jan 09 07:00:10 am62xx systemd[1]: Starting Network Name Resolution...
Jan 09 07:00:11 am62xx systemd-resolved[557]: Positive Trust Anchors:
Jan 09 07:00:11 am62xx systemd-resolved[557]: . IN DS 20326 8 2 e06d44b80b8f1d39a95c0b0d7c65d08458e880409bbc683457104237c7f8ec8d
Jan 09 07:00:11 am62xx systemd-resolved[557]: Negative trust anchors: 10.in-addr.arpa 16.172.in-addr.arpa 17.172.in-addr.arpa 18.172.in-addr.arpa 19.172.in-addr.arpa 20.172.in-addr.arpa 21.172.in-addr.arpa 22.172.in-addr.arpa 23.1>
Jan 09 07:00:11 am62xx systemd-resolved[557]: Using system hostname 'am62xx'.
Jan 09 07:00:11 am62xx systemd-resolved[557]: New scope on link *, protocol dns, family *
Jan 09 07:00:11 am62xx systemd-resolved[557]: Found new link 2/eth0
Jan 09 07:00:11 am62xx systemd-resolved[557]: Found new link 1/lo
Jan 09 07:00:11 am62xx systemd-resolved[557]: Bus bus-api-resolve: changing state UNSET → OPENING
Jan 09 07:00:11 am62xx systemd-resolved[557]: Bus bus-api-resolve: changing state OPENING → AUTHENTICATING
Jan 09 07:00:11 am62xx systemd-resolved[557]: Creating stub listener using UDP/TCP.
Jan 09 07:00:11 am62xx systemd[1]: Started Network Name Resolution.
Jan 09 07:00:11 am62xx systemd-resolved[557]: Bus bus-api-resolve: changing state AUTHENTICATING → HELLO
Jan 09 07:00:11 am62xx systemd-resolved[557]: Sent message type=method_call sender=n/a destination=org.freedesktop.DBus path=/org/freedesktop/DBus interface=org.freedesktop.DBus member=Hello cookie=1 reply_cookie=0 signature=n/a e>
Jan 09 07:00:11 am62xx systemd-resolved[557]: Sent message type=method_call sender=n/a destination=org.freedesktop.DBus path=/org/freedesktop/DBus interface=org.freedesktop.DBus member=RequestName cookie=2 reply_cookie=0 signature>
Jan 09 07:00:11 am62xx systemd-resolved[557]: Sent message type=method_call sender=n/a destination=org.freedesktop.DBus path=/org/freedesktop/DBus interface=org.freedesktop.DBus member=AddMatch cookie=3 reply_cookie=0 signature=s >
Jan 09 07:00:11 am62xx systemd-resolved[557]: Got message type=method_return sender=org.freedesktop.DBus destination=:1.4 path=n/a interface=n/a member=n/a cookie=1 reply_cookie=1 signature=s error-name=n/a error-message=n/a
Jan 09 07:00:11 am62xx systemd-resolved[557]: Bus bus-api-resolve: changing state HELLO → RUNNING
Jan 09 07:00:11 am62xx systemd-resolved[557]: Got message type=signal sender=org.freedesktop.DBus.Local destination=n/a path=/org/freedesktop/DBus/Local interface=org.freedesktop.DBus.Local member=Connected cookie=4294967295 reply>
Jan 09 07:00:11 am62xx systemd-resolved[557]: Got message type=signal sender=org.freedesktop.DBus destination=:1.4 path=/org/freedesktop/DBus interface=org.freedesktop.DBus member=NameAcquired cookie=2 reply_cookie=0 signature=s e>
Jan 09 07:00:11 am62xx systemd-resolved[557]: Got message type=signal sender=org.freedesktop.DBus destination=:1.4 path=/org/freedesktop/DBus interface=org.freedesktop.DBus member=NameAcquired cookie=3 reply_cookie=0 signature=s e>
Jan 09 07:00:11 am62xx systemd-resolved[557]: Got message type=method_return sender=org.freedesktop.DBus destination=:1.4 path=n/a interface=n/a member=n/a cookie=4 reply_cookie=2 signature=u error-name=n/a error-message=n/a
Jan 09 07:00:11 am62xx systemd-resolved[557]: Successfully acquired requested service name.
Jan 09 07:00:11 am62xx systemd-resolved[557]: Got message type=method_return sender=org.freedesktop.DBus destination=:1.4 path=n/a interface=n/a member=n/a cookie=5 reply_cookie=3 signature=n/a error-name=n/a error-message=n/a
Jan 09 07:00:11 am62xx systemd-resolved[557]: Match type='signal',sender='org.freedesktop.login1',path='/org/freedesktop/login1',interface='org.freedesktop.login1.Manager',member='PrepareForSleep' successfully installed.
Jan 09 07:00:13 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=4 reply_cookie=0 signature=>
Jan 09 07:00:13 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=5 reply_cookie=0 signature=>
Jan 09 07:00:13 am62xx systemd-resolved[557]: New scope on link eth0, protocol dns, family *
Jan 09 07:00:13 am62xx systemd-resolved[557]: New scope on link eth0, protocol llmnr, family AF_INET
Jan 09 07:00:13 am62xx systemd-resolved[557]: Transaction 374 for <am62xx IN ANY> scope llmnr on eth0/INET.
Jan 09 07:00:13 am62xx systemd-resolved[557]: Delaying llmnr transaction for 50114us.
Jan 09 07:00:13 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=6 reply_cookie=0 signature=>
Jan 09 07:00:13 am62xx systemd-resolved[557]: Timeout reached on transaction 374.
Jan 09 07:00:13 am62xx systemd-resolved[557]: Retrying transaction 374.
Jan 09 07:00:13 am62xx systemd-resolved[557]: Transaction 374 for <am62xx IN ANY> scope llmnr on eth0/INET.
Jan 09 07:00:13 am62xx systemd-resolved[557]: Sending query packet with id 374 on interface 2/AF_INET.
Jan 09 07:00:13 am62xx systemd-resolved[557]: Timeout reached on transaction 374.
Jan 09 07:00:13 am62xx systemd-resolved[557]: Retrying transaction 374.
Jan 09 07:00:13 am62xx systemd-resolved[557]: Transaction 374 for <am62xx IN ANY> scope llmnr on eth0/INET.
Jan 09 07:00:13 am62xx systemd-resolved[557]: Sending query packet with id 374 on interface 2/AF_INET.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Timeout reached on transaction 374.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Retrying transaction 374.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Transaction 374 for <am62xx IN ANY> scope llmnr on eth0/INET.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Sending query packet with id 374 on interface 2/AF_INET.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Timeout reached on transaction 374.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Retrying transaction 374.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Transaction 374 for <am62xx IN ANY> on scope llmnr on eth0/INET now complete with <attempts-max-reached> from none (unsigned).
Jan 09 07:00:14 am62xx systemd-resolved[557]: Record am62xx IN A 192.168.1.171 successfully probed.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Freeing transaction 374.
Jan 09 07:00:14 am62xx systemd-resolved[557]: New scope on link eth0, protocol llmnr, family AF_INET6
Jan 09 07:00:14 am62xx systemd-resolved[557]: Transaction 26026 for <am62xx IN ANY> scope llmnr on eth0/INET6.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Delaying llmnr transaction for 8068us.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Timeout reached on transaction 26026.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Retrying transaction 26026.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Transaction 26026 for <am62xx IN ANY> scope llmnr on eth0/INET6.
Jan 09 07:00:14 am62xx systemd-resolved[557]: Sending query packet with id 26026 on interface 2/AF_INET6.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Timeout reached on transaction 26026.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Retrying transaction 26026.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Transaction 26026 for <am62xx IN ANY> scope llmnr on eth0/INET6.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Sending query packet with id 26026 on interface 2/AF_INET6.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Timeout reached on transaction 26026.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Retrying transaction 26026.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Transaction 26026 for <am62xx IN ANY> scope llmnr on eth0/INET6.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Sending query packet with id 26026 on interface 2/AF_INET6.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Found new link 3/can0
Jan 09 07:00:15 am62xx systemd-resolved[557]: Timeout reached on transaction 26026.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Retrying transaction 26026.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Transaction 26026 for <am62xx IN ANY> on scope llmnr on eth0/INET6 now complete with <attempts-max-reached> from none (unsigned).
Jan 09 07:00:15 am62xx systemd-resolved[557]: Record am62xx IN AAAA fe80::2ab5:e8ff:fee2:41d3 successfully probed.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Freeing transaction 26026.
Jan 09 07:00:15 am62xx systemd-resolved[557]: Found new link 4/can1
Jan 09 07:00:15 am62xx systemd-resolved[557]: Found new link 5/can2
Jan 09 07:00:25 am62xx systemd-resolved[557]: Removing scope on link eth0, protocol dns, family *
Jan 09 07:00:25 am62xx systemd-resolved[557]: Removing scope on link eth0, protocol llmnr, family AF_INET
Jan 09 07:00:25 am62xx systemd-resolved[557]: Removing scope on link eth0, protocol llmnr, family AF_INET6
Jan 09 07:00:25 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=7 reply_cookie=0 signature=>
Jan 09 07:00:25 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=8 reply_cookie=0 signature=>
Jan 09 07:00:25 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=9 reply_cookie=0 signature=>
Jan 09 07:00:27 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=10 reply_cookie=0 signature>
Jan 09 07:00:29 am62xx systemd-resolved[557]: New scope on link eth0, protocol llmnr, family AF_INET6
Jan 09 07:00:29 am62xx systemd-resolved[557]: Transaction 49358 for <am62xx IN ANY> scope llmnr on eth0/INET6.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Delaying llmnr transaction for 4154us.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=11 reply_cookie=0 signature>
Jan 09 07:00:29 am62xx systemd-resolved[557]: Timeout reached on transaction 49358.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Retrying transaction 49358.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Transaction 49358 for <am62xx IN ANY> scope llmnr on eth0/INET6.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Sending query packet with id 49358 on interface 2/AF_INET6.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Timeout reached on transaction 49358.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Retrying transaction 49358.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Transaction 49358 for <am62xx IN ANY> scope llmnr on eth0/INET6.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Sending query packet with id 49358 on interface 2/AF_INET6.
Jan 09 07:00:29 am62xx systemd-resolved[557]: New scope on link eth0, protocol llmnr, family AF_INET
Jan 09 07:00:29 am62xx systemd-resolved[557]: Transaction 10784 for <am62xx IN ANY> scope llmnr on eth0/INET.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Delaying llmnr transaction for 18248us.
Jan 09 07:00:29 am62xx systemd-resolved[557]: New scope on link eth0, protocol dns, family *
Jan 09 07:00:29 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=12 reply_cookie=0 signature>
Jan 09 07:00:29 am62xx systemd-resolved[557]: Timeout reached on transaction 49358.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Retrying transaction 49358.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Transaction 49358 for <am62xx IN ANY> scope llmnr on eth0/INET6.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Sending query packet with id 49358 on interface 2/AF_INET6.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Timeout reached on transaction 10784.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Retrying transaction 10784.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Transaction 10784 for <am62xx IN ANY> scope llmnr on eth0/INET.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Sending query packet with id 10784 on interface 2/AF_INET.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Timeout reached on transaction 49358.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Retrying transaction 49358.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Transaction 49358 for <am62xx IN ANY> on scope llmnr on eth0/INET6 now complete with <attempts-max-reached> from none (unsigned).
Jan 09 07:00:29 am62xx systemd-resolved[557]: Record am62xx IN AAAA fe80::2ab5:e8ff:fee2:41d3 successfully probed.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Freeing transaction 49358.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Timeout reached on transaction 10784.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Retrying transaction 10784.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Transaction 10784 for <am62xx IN ANY> scope llmnr on eth0/INET.
Jan 09 07:00:29 am62xx systemd-resolved[557]: Sending query packet with id 10784 on interface 2/AF_INET.
Jan 09 07:00:30 am62xx systemd-resolved[557]: Timeout reached on transaction 10784.
Jan 09 07:00:30 am62xx systemd-resolved[557]: Retrying transaction 10784.
Jan 09 07:00:30 am62xx systemd-resolved[557]: Transaction 10784 for <am62xx IN ANY> scope llmnr on eth0/INET.
Jan 09 07:00:30 am62xx systemd-resolved[557]: Sending query packet with id 10784 on interface 2/AF_INET.
Jan 09 07:00:30 am62xx systemd-resolved[557]: Timeout reached on transaction 10784.
Jan 09 07:00:30 am62xx systemd-resolved[557]: Retrying transaction 10784.
Jan 09 07:00:30 am62xx systemd-resolved[557]: Transaction 10784 for <am62xx IN ANY> on scope llmnr on eth0/INET now complete with <attempts-max-reached> from none (unsigned).
Jan 09 07:00:30 am62xx systemd-resolved[557]: Record am62xx IN A 192.168.1.171 successfully probed.
Jan 09 07:00:30 am62xx systemd-resolved[557]: Freeing transaction 10784.
Jan 09 07:00:33 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=13 reply_cookie=0 signature>
Jan 09 07:00:33 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=14 reply_cookie=0 signature>
Jan 09 07:00:33 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=15 reply_cookie=0 signature>
Jan 09 07:00:41 am62xx systemd-resolved[557]: Sent message type=signal sender=n/a destination=n/a path=/org/freedesktop/resolve1 interface=org.freedesktop.DBus.Properties member=PropertiesChanged cookie=16 reply_cookie=0 signature>
```

#### resolvectl
```bash
root@am62xx:~# resolvectl status
Global
       LLMNR setting: yes
MulticastDNS setting: yes
  DNSOverTLS setting: no
      DNSSEC setting: no
    DNSSEC supported: no
Fallback DNS Servers: 1.1.1.1
                      8.8.8.8
                      1.0.0.1
                      8.8.4.4
                      2606:4700:4700::1111
                      2001:4860:4860::8888
                      2606:4700:4700::1001
                      2001:4860:4860::8844
          DNSSEC NTA: 10.in-addr.arpa
                      16.172.in-addr.arpa
                      168.192.in-addr.arpa
                      17.172.in-addr.arpa
                      18.172.in-addr.arpa
                      19.172.in-addr.arpa
                      20.172.in-addr.arpa
                      21.172.in-addr.arpa
                      22.172.in-addr.arpa
                      23.172.in-addr.arpa
                      24.172.in-addr.arpa
                      25.172.in-addr.arpa
                      26.172.in-addr.arpa
                      27.172.in-addr.arpa
                      28.172.in-addr.arpa
                      29.172.in-addr.arpa
                      30.172.in-addr.arpa
                      31.172.in-addr.arpa
                      corp
                      d.f.ip6.arpa
                      home
                      internal
                      intranet
                      lan
                      local
                      private
                      test

Link 5 (mcan2)
      Current Scopes: none
DefaultRoute setting: no
       LLMNR setting: yes
MulticastDNS setting: no
  DNSOverTLS setting: no
      DNSSEC setting: no
    DNSSEC supported: no

Link 4 (mcan1)
      Current Scopes: none
DefaultRoute setting: no
       LLMNR setting: yes
MulticastDNS setting: no
  DNSOverTLS setting: no
      DNSSEC setting: no
    DNSSEC supported: no

Link 3 (mcan0)
      Current Scopes: none
DefaultRoute setting: no
       LLMNR setting: yes
MulticastDNS setting: no
  DNSOverTLS setting: no
      DNSSEC setting: no
    DNSSEC supported: no

Link 2 (eth0)
      Current Scopes: DNS LLMNR/IPv4 LLMNR/IPv6
DefaultRoute setting: yes
       LLMNR setting: yes
MulticastDNS setting: no
  DNSOverTLS setting: no
      DNSSEC setting: no
    DNSSEC supported: no
         DNS Servers: 192.168.1.2


root@am62xx:~# resolvectl query www.baidu.com
www.baidu.com: 183.2.172.42                    -- link: eth0
               183.2.172.185                   -- link: eth0
               (www.a.shifen.com)

-- Information acquired via protocol DNS in 19.8ms.
-- Data is authenticated: no


root@am62xx:~# ping www.baidu.com
PING www.baidu.com (183.2.172.185): 56 data bytes
64 bytes from 183.2.172.185: seq=0 ttl=50 time=15.281 ms
64 bytes from 183.2.172.185: seq=1 ttl=50 time=13.363 ms
64 bytes from 183.2.172.185: seq=2 ttl=50 time=15.538 ms
^C
--- www.baidu.com ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 13.363/14.727/15.538 ms

root@am62xx:~# ping 192.168.1.2
PING 192.168.1.2 (192.168.1.2): 56 data bytes
64 bytes from 192.168.1.2: seq=0 ttl=64 time=0.797 ms
64 bytes from 192.168.1.2: seq=1 ttl=64 time=0.515 ms
^C
--- 192.168.1.2 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.515/0.656/0.797 ms


root@am62xx:~# ping 183.2.172.42
PING 183.2.172.42 (183.2.172.42): 56 data bytes
64 bytes from 183.2.172.42: seq=0 ttl=50 time=16.598 ms
64 bytes from 183.2.172.42: seq=1 ttl=50 time=24.877 ms
^C
--- 183.2.172.42 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 16.598/20.737/24.877 ms


root@am62xx:~# ping 183.2.172.185
PING 183.2.172.185 (183.2.172.185): 56 data bytes
64 bytes from 183.2.172.185: seq=0 ttl=50 time=21.060 ms
64 bytes from 183.2.172.185: seq=1 ttl=50 time=17.058 ms
^C
--- 183.2.172.185 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 17.058/19.059/21.060 ms
```

#### networkctl
```bash
root@am62xx:~# networkctl list
IDX LINK  TYPE     OPERATIONAL SETUP
  1 lo    loopback carrier     unmanaged
  2 eth0  ether    routable    configured
  3 mcan0 can      off         unmanaged
  4 mcan1 can      off         unmanaged
  5 mcan2 can      off         unmanaged

5 links listed.


root@am62xx:~# networkctl status
●   State: routable
  Address: 192.168.1.171 on eth0
           fe80::2ab5:e8ff:fee2:41d3 on eth0
  Gateway: 192.168.1.2 on eth0
      DNS: 192.168.1.2

root@am62xx:~# tcpdump -i eth0 port 67 or port 68 -v
[ 1173.182635] device eth0 entered promiscuous mode
tcpdump: listening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes
^C
0 packets captured
10 packets received by filter
0 packets dropped by kernel
[ 1175.713404] device eth0 left promiscuous mode
```

#### udhcpc
```bash
root@am62xx:~# udhcpc -i eth0 -f -v
udhcpc: started, v1.31.1
udhcpc: sending discover
udhcpc: sending select for 192.168.1.172
udhcpc: lease of 192.168.1.172 obtained, lease time 43200
/etc/udhcpc.d/50default: Adding DNS 192.168.1.2

root@am62xx:~# ll /etc/udhcpc.d/50default
-rwxr-xr-x    1 root     root          2652 Jun 16  2023 /etc/udhcpc.d/50default
```

