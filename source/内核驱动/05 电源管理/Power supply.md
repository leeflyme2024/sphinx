# 相关文件
```Makefile
power_supply-y                          := power_supply_core.o
power_supply-$(CONFIG_SYSFS)            += power_supply_sysfs.o
power_supply-$(CONFIG_LEDS_TRIGGERS)    += power_supply_leds.o

obj-$(CONFIG_POWER_SUPPLY)      += power_supply.o
obj-$(CONFIG_POWER_SUPPLY_HWMON) += power_supply_hwmon.o

obj-$(CONFIG_CHARGER_GPIO)      += gpio-charger.o
obj-$(CONFIG_CHARGER_MANAGER)   += charger-manager.o

power/charger-manager.rst
power/power_supply_class.rst
ABI/testing/sysfs-class-power
devicetree/bindings/power/supply/battery.yaml
devicetree/bindings/power/supply/gpio-charger.yaml
devicetree/bindings/power/supply/charger-manager.txt
```


# DFT
```bash
root@AM62x:/# cat /proc/interrupts | grep charger
349:          0          0      GPIO  42 Edge    -davinci_gpio  gpio-charger
```


```bash
root@AM62x:/# ll /sys/class/power_supply/gpio-charger/
total 0
lrwxrwxrwx 1 root root    0  8月 11 18:03 device -> ../../../gpio-charger
drwxr-xr-x 3 root root    0  8月 11 18:03 hwmon0
-r--r--r-- 1 root root 4096  8月 11 18:03 online
drwxr-xr-x 2 root root    0  8月 11 18:03 power
lrwxrwxrwx 1 root root    0  8月 11 18:03 subsystem -> ../../../../../class/power_supply
-r--r--r-- 1 root root 4096  8月 11 18:03 type
-rw-r--r-- 1 root root 4096  8月 11 18:03 uevent


root@AM62x:/# ll /sys/class/power_supply/gpio-charger/hwmon0/
total 0
lrwxrwxrwx 1 root root    0  8月 11 18:04 device -> ../../gpio-charger
-r--r--r-- 1 root root 4096  8月 11 18:04 name
drwxr-xr-x 2 root root    0  8月 11 18:04 power
lrwxrwxrwx 1 root root    0  8月 11 18:04 subsystem -> ../../../../../../class/hwmon
-rw-r--r-- 1 root root 4096  8月 11 18:04 uevent

root@AM62x:/# cat /sys/class/power_supply/gpio-charger/hwmon0/name
gpio_charger

root@AM62x:/# ll /sys/class/power_supply/gpio-charger/hwmon0/power/
total 0
-rw-r--r-- 1 root root 4096  8月 11 18:04 autosuspend_delay_ms
-rw-r--r-- 1 root root 4096  8月 11 18:04 control
-r--r--r-- 1 root root 4096  8月 11 18:04 runtime_active_time
-r--r--r-- 1 root root 4096  8月 11 18:04 runtime_status
-r--r--r-- 1 root root 4096  8月 11 18:04 runtime_suspended_time

root@AM62x:/# cat /sys/class/power_supply/gpio-charger/hwmon0/power/control
auto
root@AM62x:/# cat /sys/class/power_supply/gpio-charger/hwmon0/power/runtime_active_time
0
root@AM62x:/# cat /sys/class/power_supply/gpio-charger/hwmon0/power/runtime_status
unsupported
root@AM62x:/# cat /sys/class/power_supply/gpio-charger/hwmon0/power/runtime_suspended_time
0


root@AM62x:/# ll ./sys/class/power_supply/gpio-charger/power/
total 0
-rw-r--r-- 1 root root 4096  8月 11 18:04 autosuspend_delay_ms
-rw-r--r-- 1 root root 4096  8月 11 18:04 control
-r--r--r-- 1 root root 4096  8月 11 18:04 runtime_active_time
-r--r--r-- 1 root root 4096  8月 11 18:04 runtime_status
-r--r--r-- 1 root root 4096  8月 11 18:04 runtime_suspended_time


root@AM62x:/# cat /sys/class/power_supply/gpio-charger/type
Mains


root@AM62x:/# cat /sys/class/power_supply/gpio-charger/online
1
```

在 Linux 系统中，`/sys/class/power_supply/gpio-charger` 目录是用来管理通过 GPIO 与系统进行通信的电源或充电器设备的。在这个目录下，有一系列文件和子目录，它们提供了有关电源（如电池、充电器、UPS 等）的信息。以下是各个文件和目录的详细解释：

### 1. `/sys/class/power_supply/gpio-charger/`

这是一个表示电源设备的虚拟文件系统节点，系统会将所有关于电源设备的信息通过这个节点暴露出来。

- `device -> ../../../gpio-charger`: 该符号链接指向电源设备的实际硬件设备。
- `hwmon0`: 这是一个子目录，表示与硬件监控相关的部分。通常，它用于存储电源相关的统计数据，如电池电量、功耗等。
- `online`: 这是一个文件，表示设备是否在线。在你的输出中，`1` 表示电源设备已连接并处于正常工作状态。
- `subsystem -> ../../../../../class/power_supply`: 这是指向 `power_supply` 子系统的符号链接，表示它是 `power_supply` 类的一部分。
- `type`: 这是一个文件，表示充电器的类型。例如，`Mains` 表示它是通过墙壁电源连接的充电器。
- `uevent`: 提供设备的 uevent 信息，通常用于 udev 设备管理系统。

### 2. `/sys/class/power_supply/gpio-charger/hwmon0/`

这个目录用于硬件监控相关的设置，主要用于提供与电源相关的统计信息和控制。

- `device -> ../../gpio-charger`: 该符号链接指向设备的实际硬件设备。
- `name`: 文件内容为 `gpio_charger`，表示设备的名称或类型。
- `power`: 这个子目录包含与设备电源管理相关的参数。

### 3. `/sys/class/power_supply/gpio-charger/hwmon0/power/`

这个目录包含了与电源管理和能耗控制相关的信息。

- `autosuspend_delay_ms`: 该文件控制自动挂起的延迟时间（以毫秒为单位）。如果系统在一定时间内没有活动，它会自动将设备置于挂起状态。
- `control`: 该文件可以用来控制电源的状态，例如启动或停止设备。
- `runtime_active_time`: 表示设备在活动状态下的时间（单位：毫秒）。
- `runtime_status`: 显示设备当前的运行状态（例如 `active` 或 `suspended`）。
- `runtime_suspended_time`: 设备处于挂起状态的时间（单位：毫秒）。

### 4. `/sys/class/power_supply/gpio-charger/online`

- 该文件的内容显示设备是否在线。在你的输出中，`1` 表示设备已连接并处于正常工作状态。如果显示为 `0`，则表示设备已掉电或断开。

### 5. `/sys/class/power_supply/gpio-charger/type`

- 该文件显示了电源设备的类型。输出 `Mains` 表示它是一个接入了墙壁电源的充电器，通常用于向电池充电。

### 6. `/sys/class/power_supply/gpio-charger/power/`

该目录与 `hwmon0/power` 目录内容相似，包含了设备的电源管理相关设置。

### 总结

- **`/sys/class/power_supply/gpio-charger/online`**: 文件内容为 `1` 表示电源设备在线，处于正常工作状态；如果为 `0`，表示设备掉电或未连接。
- **`/sys/class/power_supply/gpio-charger/type`**: 显示电源设备的类型，比如 `Mains` 表示它是通过墙壁电源连接的充电器。
- **`/sys/class/power_supply/gpio-charger/hwmon0/power/`**: 包含与电源管理相关的统计信息，例如设备的活动时间、挂起时间等。

这些文件和目录提供了丰富的电源设备状态信息和电源管理控制接口，方便通过用户空间或内核驱动来控制电源管理。



`/sys/class/power_supply/gpio-charger/online` 是一个 Linux 系统中与电源管理相关的文件，通常与电池、充电器或电源适配器的状态监测有关。它属于 `power_supply` 子系统，在这个子系统中，设备和驱动程序通过标准的接口提供电源信息。

### `/sys/class/power_supply/gpio-charger/online` 详解

- **路径含义**：
    
    - `/sys/class/power_supply/`：这个路径下包含了与电源供应相关的信息，通常是电池、电源适配器、充电器等设备的信息。
    - `gpio-charger`：表示这是一个通过 GPIO（通用输入输出）接口监控的充电器。它可能是一个外部电源管理设备，监控充电状态或检测外部电源连接状态。
    - `online`：这是一个文件，表示当前设备的“在线”状态，即电源是否连接。通常是一个表示充电器状态的文件，值为 1 时表示设备在线（充电器接入），值为 0 时表示设备离线（充电器未接入）。
- **`/sys/class/power_supply/gpio-charger/online` 文件内容**：
    
    - `0`：表示充电器未连接或电源处于断开状态。
    - `1`：表示充电器已连接或电源处于在线状态。
    
    这个文件可以通过读取或写入来监控和控制电源状态。例如，当充电器连接到设备时，该文件的值为 `1`，如果充电器断开，该值变为 `0`。
    

### 掉电检测功能实现

在 Linux 系统中，实现掉电检测功能的常见方式是通过电源管理子系统（如 `power_supply`）和 GPIO 中断来检测电源的变化，或者通过其他硬件监测机制（如 UPS 或外部电源管理芯片）。掉电检测的核心思想是检测电源的变化并在电源断开时采取适当的操作（例如，关机、报警或保存数据）。

#### 1. **通过读取 `/sys/class/power_supply/gpio-charger/online` 文件进行轮询**

如果你不需要立即响应掉电事件，而是可以周期性地检查电源状态，可以通过轮询 `/sys/class/power_supply/gpio-charger/online` 来监控掉电状态。

- **示例：使用 shell 脚本轮询检测掉电**：

```bash
#!/bin/bash

while true; do
    status=$(cat /sys/class/power_supply/gpio-charger/online)
    if [ "$status" -eq 0 ]; then
        echo "Power is off. Taking action..."
        # 在此处执行掉电处理逻辑，如关机或数据保存
    fi
    sleep 1  # 每秒检测一次
done
```

这个脚本会每秒钟检测一次电源状态，一旦检测到电源断开（值为 `0`），就会执行相应的操作（如关机、保存数据等）。

#### 2. **通过 GPIO 中断进行实时响应**

如果你需要即时响应电源状态变化（比如电源断开时立即进行掉电处理），你可以通过 GPIO 中断来实现实时检测。这种方式要求硬件支持 GPIO 中断，并且你需要在内核中配置相应的驱动或工具来监听 GPIO 状态变化。

- **示例：通过 GPIO 中断检测掉电（需要内核支持）**：
    
    1. **配置 GPIO 中断**：确保内核启用了 GPIO 驱动，并且电源适配器（如充电器）连接到正确的 GPIO 引脚。
    2. **使用 `sysfs` 或其他工具监听 GPIO 状态变化**：
        - 可以通过 `sysfs` 接口读取 GPIO 的状态。通过 `/sys/class/gpio/export` 和 `/sys/class/gpio/gpioX/value` 读取和写入 GPIO 状态。
            
        - 监听 GPIO 中断并执行相应的动作。
            
    
    - **配置和监听 GPIO 中断**：
    
    ```bash
    echo "X" > /sys/class/gpio/export   # "X" 是 GPIO 引脚号
    echo "falling" > /sys/class/gpio/gpioX/edge  # 设置中断触发类型为下降沿
    cat /sys/class/gpio/gpioX/value  # 读取 GPIO 引脚的状态
    ```
    
    这段代码通过 sysfs 设置 GPIO 引脚为输入，并设置为在电源断开时触发中断（例如电压下降）。你可以使用 shell 脚本或其他工具在检测到 GPIO 引脚状态变化时进行掉电处理。
    

#### 3. **通过系统事件（ACPI 或 UDEV）进行响应**

另一种方式是通过 `udev` 规则或 ACPI（高级配置和电源接口）来响应电源事件。例如，当电源电池电量过低时，`udev` 可以触发相关的处理逻辑（如自动关机）。

- **通过 UDEV 规则**： `udev` 是一个设备管理守护进程，可以根据硬件事件自动触发操作。例如，可以设置一个 udev 规则来监听电源适配器状态变化。
    
    - **示例：设置 udev 规则来监听电源适配器事件**：
        
        创建或编辑 `/etc/udev/rules.d/90-power-supply.rules` 文件，添加如下规则：
        
        ```bash
        ACTION=="change", SUBSYSTEM=="power_supply", KERNEL=="gpio-charger", ENV{POWER_SUPPLY_ONLINE}=="0", RUN+="/usr/bin/shutdown -h now"
        ```
        
        这条规则会在电源适配器断开时（`POWER_SUPPLY_ONLINE == 0`）触发关机命令。
        

#### 4. **系统电池管理和 UPS**

对于更复杂的电源管理，可以使用 UPS（不间断电源）管理工具，如 `nut`（Network UPS Tools），来监控电源供应状态。UPS 系统会在电源中断时发送事件，通知系统进行处理。

- **安装和配置 UPS 工具**：

```bash
sudo apt-get install nut
```

安装后，配置 NUT 来监控 UPS 状态，当 UPS 断电时，系统会收到警报，并可以根据设置执行关机或其他处理。

### 总结

`/sys/class/power_supply/gpio-charger/online` 文件提供了一个简单的接口来监测电源状态，值为 `1` 表示电源在线（充电器接入），值为 `0` 表示电源离线。实现掉电检测功能的方法有多种，包括：

1. 轮询该文件获取电源状态。
2. 通过 GPIO 中断实现实时掉电响应。
3. 使用 `udev` 或 ACPI 事件自动化处理。
4. 使用 UPS 管理工具（如 `nut`）实现更复杂的电源监控。

根据具体的需求，可以选择合适的方法来实现掉电检测功能。