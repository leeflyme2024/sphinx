# U-Boot

# Kernel
# 光标位置异常
## 问题描述

```bash
root@AM62x:~# bluetoothctl
hci0 new_settings: powered connectable discoverable bondable ssp br/edr le secure-conn
Agent registered
[CHG] Controller 64:1C:10:28:D5:C6 Pairable: yes
[bluetooth]#
为什么执行bluetoothctl命令后，光标是在[bluetooth]#的最左边而不是最右边？
```

```bash
root@AM62x:/usr/sbin# resize
COLUMNS=231;LINES=60;export COLUMNS LINES;
root@AM62x:/usr/sbin#
root@AM62x:/usr/sbin# echo $LANG
zh_CN.UTF-8
root@AM62x:/usr/sbin# bluetoothctl
hci0 new_settings: powered bondable ssp br/edr le secure-conn
Agent registered
[CHG] Controller 64:1C:10:28:D5:C6 Pairable: yes
hci0 64:1C:10:28:D6:30 type BR/EDR connected eir_len 0
[CHG] Device 64:1C:10:28:D6:30 Connected: yes
[ZY-AM62x-BT-01]#
只有执行bluetoothctl后光标才到最左边，没有执行之前，如root@AM62x:/usr/sbin#，都是在最右边的
```

```bash

root@AM62x:~# cat transcript.log
Script started on 2023-10-22 01:55:05+08:00 [COMMAND="bluetoothctl" TERM="vt100" TTY="/dev/ttyS2" COLUMNS="231" LINES="60"]
hci0 new_settings: powered bondable ssp br/edr le secure-conn
Agent registered
[CHG] Controller 64:1C:10:28:D5:C6 Pairable: yes
helpetooth]#
Menu main:
Available commands:
-------------------
advertise                                         Advertise Options Submenu
monitor                                           Advertisement Monitor Options Submenu
scan                                              Scan Options Submenu
gatt                                              Generic Attribute Submenu
admin                                             Admin Policy Submenu
player                                            Media Player Submenu
endpoint                                          Media Endpoint Submenu
transport                                         Media Transport Submenu
mgmt                                              Management Submenu
monitor                                           Advertisement Monitor Submenu
list                                              List available controllers
show [ctrl]                                       Controller information
select <ctrl>                                     Select default controller
devices [Paired/Bonded/Trusted/Connected]         List available devices, with an optional property as the filter
system-alias <name>                               Set controller alias
reset-alias                                       Reset controller alias
power <on/off>                                    Set controller power
pairable <on/off>                                 Set controller pairable mode
discoverable <on/off>                             Set controller discoverable mode
discoverable-timeout [value]                      Set discoverable timeout
agent <on/off/auto/capability>                    Enable/disable agent with given capability
default-agent                                     Set agent as the default one
advertise <on/off/type>                           Enable/disable advertising with given type
set-alias <alias>                                 Set device alias
scan <on/off/bredr/le>                            Scan for devices
info [dev/set]                                    Device/Set information
pair [dev]                                        Pair with device
cancel-pairing [dev]                              Cancel pairing with device
trust [dev]                                       Trust device
untrust [dev]                                     Untrust device
block [dev]                                       Block device
unblock [dev]                                     Unblock device
remove <dev>                                      Remove device
connect <dev>                                     Connect device
disconnect [dev]                                  Disconnect device
menu <name>                                       Select submenu
version                                           Display version
quit                                              Quit program
exit                                              Quit program
help                                              Display help about this program
export                                            Print environment variables
script <filename>                                 Run script
exitetooth]#

Script done on 2023-10-22 01:55:10+08:00 [COMMAND_EXIT_CODE="0"]
```

```bash
# 执行vim后，按向下翻页和向上翻页字母会变大写也是由这个导致的
```

## 解决方法
```
# 修改TERM的值
export TERM=xterm
```

```bash
script -c "bluetoothctl" transcript.log
export TERM=xterm
tisdk-yocto-image-am62xx-evm/overlay/lib/systemd/system/serial-getty@.service:32:Environment="TERM=xterm"
tisdk-yocto-image-am62xx-evm/overlay/etc/profile:6:[ "$TERM" ] || TERM="vt100"  # Basic terminal capab. For screen etc.
tisdk-buildroot-2022.02/src/overlay/configs/zy_am62x_defconfig:443:BR2_TARGET_GENERIC_GETTY_TERM="vt100"
```

```bash
root@AM62x:/# stty -a
speed 115200 baud; rows 60; columns 231; line = 0;
intr = ^C; quit = ^\; erase = ^?; kill = ^U; eof = ^D; eol = <undef>; eol2 = <undef>; swtch = <undef>; start = ^Q; stop = ^S; susp = ^Z; rprnt = ^R; werase = ^W; lnext = ^V; discard = ^O; min = 1; time = 0;
-parenb -parodd -cmspar cs8 hupcl -cstopb cread clocal -crtscts
-ignbrk brkint -ignpar -parmrk -inpck -istrip -inlcr -igncr icrnl ixon -ixoff -iuclc -ixany imaxbel -iutf8
opost -olcuc -ocrnl onlcr -onocr -onlret -ofill -ofdel nl0 cr0 tab0 bs0 vt0 ff0
isig icanon iexten echo echoe echok -echonl -noflsh -xcase -tostop -echoprt echoctl echoke -flusho -extproc
```

## 原因
### Key Points
- The cursor appears on the left side of the `[bluetooth]#` prompt in `bluetoothctl` due to terminal configuration issues, likely related to the serial terminal setup.
- This is unusual because the cursor should normally be on the right side, ready for input.

### Why This Happens
When you run `bluetoothctl`, it enters an interactive shell with the prompt `[bluetooth]#`, and the cursor should be after the `#` for you to type commands. If the cursor is on the left, it suggests the terminal isn't handling the prompt correctly. This is likely because:
- You're using a serial terminal (`/dev/ttyS2`) with `TERM=vt100`, which may not support all modern terminal features.
- The terminal might be in raw mode, causing input and output to mix up, making the cursor appear before the prompt.

### What to Do
To fix this, try these simple steps:
1. Reset your terminal by typing `reset` or `stty sane` before running `bluetoothctl`.
2. Check your terminal settings with `stty -a` to ensure "icanon" and "echo" are enabled.
3. If possible, test in a graphical terminal like GNOME Terminal to see if the issue persists.

### Surprising Detail
It's surprising that the prompt changes to `[ZY-AM62x-BT-01]#` in later runs, which might be due to device-specific configurations affecting how `bluetoothctl` displays the prompt.

---

### Investigation Report: Cursor Position Issue in `bluetoothctl` on Serial Terminal

This report investigates why, when running the `bluetoothctl` command on a serial terminal, the cursor appears on the left side of the prompt `[bluetooth]#` instead of the right side, where it should be for input. The user is operating on a system with `TERM=vt100` and `TTY=/dev/ttyS2`, and has provided a transcript log for analysis. The investigation covers potential causes, troubleshooting steps, and detailed analysis of the terminal and `bluetoothctl` behavior.

#### Background and Context
The user runs `bluetoothctl` and observes that the cursor is positioned at the beginning of the line, before the prompt `[bluetooth]#`, rather than after it, which is the standard behavior for interactive command-line tools. The transcript log shows:

```
Script started on 2023-10-22 01:55:05+08:00 [COMMAND="bluetoothctl" TERM="vt100" TTY="/dev/ttyS2" COLUMNS="231" LINES="60"]
hci0 new_settings: powered bondable ssp br/edr le secure-conn
Agent registered
[CHG] Controller 64:1C:10:28:D5:C6 Pairable: yes
helpetooth]#
Menu main:
Available commands:
-------------------
...
exitetooth]#
```

The transcript also indicates the user tried inputting commands like "help" and "exit," with the prompt appearing concatenated as "helpetooth]#" and "exitetooth]#", suggesting potential issues with terminal input/output handling.

#### Expected Behavior
In a standard setup, `bluetoothctl` uses the `readline` library to manage interactive input, printing the prompt `[bluetooth]#` and positioning the cursor at the end for user input. This is consistent with documentation such as [bluetoothctl: Bluetooth Control Command Line Tool](https://www.mankier.com/1/bluetoothctl), which shows the prompt followed by cursor-ready input.

#### Possible Causes
Several factors could lead to the cursor appearing on the left side:

1. **Terminal Mode and Settings**:
   - The terminal might be in raw mode, where input is not buffered, and echo is disabled. This can cause user input to appear before the prompt, mixing with the output.
   - The user can check this with `stty -a`, looking for "icanon" (canonical mode, cooked mode) and "echo" settings. If disabled, the terminal may not handle prompts correctly.

2. **Terminal Type (TERM=vt100)**:
   - `TERM=vt100` is a basic terminal type supporting ANSI escape sequences, but it may lack support for some modern features used by `bluetoothctl`. This could lead to incorrect cursor positioning if escape sequences are misinterpreted.
   - Modern graphical terminals like xterm or GNOME Terminal typically handle these sequences better, suggesting a potential compatibility issue with serial terminals.

3. **Serial Terminal Configuration**:
   - The TTY is `/dev/ttyS2`, indicating a serial terminal, which may have specific settings like baud rate, parity, or flow control affecting input/output. Incorrect settings could disrupt prompt display and cursor positioning.

4. **Locale and Encoding**:
   - The user's `LANG=zh_CN.UTF-8` suggests UTF-8 encoding, which should handle ASCII prompts like `[bluetooth]#` correctly. However, if the terminal misinterprets character widths, it could affect cursor positioning, though unlikely given the prompt's ASCII nature.

5. **Transcript Log Interpretation**:
   - The transcript shows "helpetooth]#" and "exitetooth]#", which may indicate the user's input ("help" or "exit") is concatenated with the prompt without proper newline separation. This suggests the terminal is not handling input/output buffering correctly, possibly due to raw mode or timing issues on the serial connection.

6. **Prompt Changes**:
   - The user mentioned in previous interactions that the prompt later changed to `[ZY-AM62x-BT-01]#`, which could be device-specific and might affect how `bluetoothctl` manages the prompt, potentially impacting cursor positioning.

#### Detailed Analysis
The transcript log provides critical insight. The line "helpetooth]#" suggests the user typed "help" and the prompt "[bluetooth]#" was printed, but without proper separation, indicating a terminal issue. This is not standard behavior, as typically, the command output should appear on a new line, followed by the prompt with the cursor at the end.

Given the serial terminal setup, several factors could contribute:
- **Raw Mode**: If the terminal is in raw mode (no "icanon"), input is sent immediately, and the prompt might not be positioned correctly. Running `stty icanon` and `stty echo` should enable cooked mode and echo, respectively.
- **Escape Sequence Handling**: `bluetoothctl` likely uses ANSI escape sequences to position the cursor, but vt100 might not interpret them correctly, especially on a serial connection with potential latency or buffering issues.
- **Terminal Size**: The user set `COLUMNS=231` and `LINES=60` via `resize`, which should be sufficient, but the serial terminal might not report these correctly to `bluetoothctl`, affecting prompt display.

#### Troubleshooting Steps
To resolve the issue, the user should follow these steps:

1. **Check Terminal Settings**:
   - Run `stty -a` to verify settings. Ensure "icanon" and "echo" are enabled. If not, run:
     ```
     stty icanon
     stty echo
     ```
   - This ensures the terminal is in cooked mode, buffering input until enter is pressed, and echoing user input.

2. **Reset the Terminal**:
   - Run `reset` or `stty sane` to restore default terminal settings, which can fix mode and cursor positioning issues.

3. **Test with Different TERM**:
   - Set `TERM=xterm` temporarily and run `bluetoothctl` to see if the issue persists. For example:
     ```
     export TERM=xterm
     bluetoothctl
     ```
   - This tests if the terminal type is the cause, as xterm supports more features than vt100.

4. **Try Graphical Terminal**:
   - If possible, run `bluetoothctl` in a graphical terminal like GNOME Terminal or xterm to compare behavior. If the cursor positions correctly, the issue is specific to the serial terminal setup.

5. **Verify Terminal Size**:
   - Run `tput cols` and `tput lines` to ensure the terminal reports the correct size (231 columns, 60 lines). If not, adjust with `resize` and re-run `bluetoothctl`.

6. **Analyze Input/Output**:
   - Use `script -c "bluetoothctl" new_transcript.log` again, and review the log for any unusual escape sequences or prompt behavior. Look for missing newlines or concatenated input/output.

7. **Check Serial Terminal Configuration**:
   - Ensure the serial terminal (`/dev/ttyS2`) is configured correctly, including baud rate, parity, and flow control. Use `setserial` or system documentation to verify.

#### Surprising Observations
- The prompt changing to `[ZY-AM62x-BT-01]#` in later runs is unexpected and may indicate device-specific configurations affecting `bluetoothctl`'s prompt, potentially impacting cursor positioning.
- The concatenation of user input and prompt in the transcript ("helpetooth]#") is unusual and suggests a terminal buffering or mode issue, particularly on a serial connection.

#### Table: Possible Issues and Solutions

| **Possible Issue**                     | **Solution**                                      |
|---------------------------------------|--------------------------------------------------|
| Terminal in raw mode (no "icanon")     | Run `stty icanon` and `stty echo` to enable cooked mode. |
| Incorrect TERM type (vt100)            | Try setting `TERM=xterm` and re-run `bluetoothctl`. |
| Serial terminal configuration          | Verify baud rate, parity, and flow control settings. |
| Terminal size not reported correctly   | Run `resize` and verify with `tput cols` and `tput lines`. |
| Prompt handling issues                 | Reset terminal with `reset` or `stty sane`.       |

#### Conclusion
The cursor position issue in `bluetoothctl` is likely due to the terminal being in raw mode or misconfigured, particularly given the serial terminal setup with `TERM=vt100`. By checking and adjusting terminal settings, resetting the terminal, and testing with different configurations, the user should resolve the problem. If the issue persists, further investigation into the serial terminal setup or seeking system administrator support is recommended.

#### Key Citations
- [bluetoothctl: Bluetooth Control Command Line Tool](https://www.mankier.com/1/bluetoothctl)
- [How to Manage Bluetooth Devices on Linux Using bluetoothctl](https://www.makeuseof.com/manage-bluetooth-linux-with-bluetoothctl/)
- [Toggling Your Bluetooth Connection using bluetoothctl on the Command-Line](https://www.jvt.me/post/2020/03/27/toggle-bluetooth-connection/)


## 拓展知识
```bash
root@AM62x:~# tree /usr/share/terminfo
/usr/share/terminfo
├── a
│   └── ansi
├── d
│   └── dumb
├── l
│   └── linux
├── p
│   ├── putty
│   ├── putty-256color
│   └── putty-vt100
├── s
│   ├── screen
│   └── screen-256color
├── v
│   ├── vt100
│   ├── vt100-putty
│   ├── vt102
│   ├── vt200
│   └── vt220
└── x
    ├── xterm
    ├── xterm+256color
    ├── xterm-256color
    ├── xterm-color
    └── xterm-xfree86
```

这两条命令的区别在于它们告诉程序你的终端（terminal）的类型不同，从而使程序使用相应的控制码和特性。

### export TERM=xterm 与 export TERM=vt100 的区别

- **xterm**  
  - 代表的是 X 窗口系统下常见的终端模拟器。  
  - 支持丰富的特性，如 256 色（通常用 xterm-256color）、鼠标事件、窗口大小调整、更多键盘功能等。  
  - 现代的 Linux 桌面环境下常用，能充分利用图形界面下的高级功能。  

- **vt100**  
  - vt100 是 DEC 公司早期推出的一种物理终端型号。  
  - 它只支持基本的 ANSI 转义序列，功能较为简单，颜色支持也有限。  
  - 通常用于需要严格兼容旧式终端环境的场景，或在条件受限（比如远程连接某些老旧设备）的情况下使用。  

### TERM 可选的配置值有哪些？

TERM 环境变量的值非常多，因为它们对应着各种不同的终端类型和模拟器。常见的有：

- **xterm** 系列：  
  - `xterm`  
  - `xterm-color`  
  - `xterm-256color`  
- **VT 系列：**  
  - `vt100`  
  - `vt220`  
  - `vt102`  
- **Linux 控制台：**  
  - `linux`
- **Screen/Tmux：**  
  - `screen`  
  - `screen-256color`  
- **其他终端模拟器：**  
  - 如 `putty`、`konsole`、`rxvt`、`gnome`、`terminator` 等，根据不同终端软件会有所区别。  
- **dumb：**  
  - 适用于非常简陋的终端或脚本环境，表示不支持复杂控制序列。  


实际上，你可以通过查找系统的 terminfo 数据库（如在 `/usr/share/terminfo` 或使用 `toe` 命令）来查看系统支持哪些终端类型。

---

总之，选择哪个值取决于你使用的终端的实际功能。设置正确的终端类型可以确保程序能正确地显示和交互。