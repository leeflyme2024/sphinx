```bash
# touchscreen
i2cdetect -l
i2cdetect -F 1
i2cdetect -F 2
i2cdetect -r -y 1
i2cdetect -r -y 2
i2cdump -f -y 1 0x14 b
i2cdump -f -y 2 0x14 b

ls -l /dev/input/
cat /proc/bus/input/devices


cd /opt/tslib/bin
export TSLIB_TSDEVICE=/dev/input/event1
./ts_calibrate
./ts_test
./ts_test_mt
./ts_print
./ts_print_raw
./ts_print_mt
./ts_finddev /dev/input/event1 15

evtest /dev/input/event1
hexdump /dev/input/event1
hexdump /dev/input/touchscreen0
```

# 总线信息
```bash
root@AM62x:~# i2cdetect -l
i2c-0   i2c             OMAP I2C adapter                        I2C adapter
i2c-1   i2c             OMAP I2C adapter                        I2C adapter
i2c-2   i2c             OMAP I2C adapter                        I2C adapter
i2c-4   i2c             OMAP I2C adapter                        I2C adapter
i2c-5   i2c             OMAP I2C adapter                        I2C adapter


root@AM62x:~# i2cdetect -F 1
Functionalities implemented by /dev/i2c-1:
I2C                              yes
SMBus Quick Command              no
SMBus Send Byte                  yes
SMBus Receive Byte               yes
SMBus Write Byte                 yes
SMBus Read Byte                  yes
SMBus Write Word                 yes
SMBus Read Word                  yes
SMBus Process Call               yes
SMBus Block Write                yes
SMBus Block Read                 no
SMBus Block Process Call         no
SMBus PEC                        yes
I2C Block Write                  yes
I2C Block Read                   yes


root@AM62x:~# i2cdetect -F 2
Functionalities implemented by /dev/i2c-2:
I2C                              yes
SMBus Quick Command              no
SMBus Send Byte                  yes
SMBus Receive Byte               yes
SMBus Write Byte                 yes
SMBus Read Byte                  yes
SMBus Write Word                 yes
SMBus Read Word                  yes
SMBus Process Call               yes
SMBus Block Write                yes
SMBus Block Read                 no
SMBus Block Process Call         no
SMBus PEC                        yes
I2C Block Write                  yes
I2C Block Read                   yes

root@AM62x:~# i2cdetect -r -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- --
10: -- -- -- -- UU -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: 30 -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- UU -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --

root@AM62x:~# i2cdetect -r -y 2
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- --
10: -- -- -- -- UU -- -- -- UU -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --

root@AM62x:~# i2cdump -f -y 1 0x14 b
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f    0123456789abcdef
00: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
10: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
20: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
30: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
40: 00 00 03 00 00 00 00 00 00 00 00 00 00 00 00 00    ..?.............
50: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
60: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
70: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
80: 00 00 03 3a 03 00 00 00 00 00 00 00 03 35 03 00    ..?:?.......?5?.
90: 00 00 00 00 00 63 08 f4 09 ad 00 00 00 00 00 00    .....c????......
a0: 00 00 00 00 00 00 00 00 00 ff 00 00 00 03 39 00    .............?9.
b0: 00 00 00 00 00 00 01 00 00 00 00 00 00 02 00 00    ......?......?..
c0: 00 00 00 00 00 00 00 00 00 ff 00 00 00 00 05 29    ..............?)
d0: 00 1d 19 18 00 1d 19 18 00 1d 19 18 00 1d 19 18    .???.???.???.???
e0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
f0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................

root@AM62x:~# i2cdump -f -y 2 0x14 b
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f    0123456789abcdef
00: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
10: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
20: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
30: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
40: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
50: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
60: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
70: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
80: 00 00 0f 03 96 00 00 00 00 00 00 00 f6 03 00 00    ..???.......??..
90: 00 00 00 00 00 08 ca 04 5b 00 00 00 00 00 00 00    .....???[.......
a0: 00 00 00 00 00 00 00 00 00 ff 00 02 17 00 00 03    ...........??..?
b0: a7 00 00 00 00 00 ff 00 00 00 00 00 00 00 00 00    ?...............
c0: 00 00 00 00 00 00 00 00 01 ff 00 00 00 00 00 f2    ........?......?
d0: 42 08 49 82 40 08 49 82 40 08 49 82 40 08 49 02    B?I?@?I?@?I?@?I?
e0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
f0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00    ................
```

# 触摸信息
```bash
root@AM62x:~# cat /proc/bus/input/devices
I: Bus=0019 Vendor=001f Product=0001 Version=0100
N: Name="pwm-beeper"
P: Phys=pwm/input0
S: Sysfs=/devices/platform/buzzer/input/input0
U: Uniq=
H: Handlers=kbd event0
B: PROP=0
B: EV=40001
B: SND=6

I: Bus=0018 Vendor=dead Product=beef Version=28bb
N: Name="goodix-ts"
P: Phys=input/ts
S: Sysfs=/devices/virtual/input/input1
U: Uniq=
H: Handlers=event1
B: PROP=2
B: EV=b
B: KEY=0
B: ABS=265800000000000

I: Bus=0018 Vendor=dead Product=beef Version=28bb
N: Name="goodix-ts"
P: Phys=input/ts
S: Sysfs=/devices/virtual/input/input2
U: Uniq=
H: Handlers=event2
B: PROP=2
B: EV=b
B: KEY=0
B: ABS=265800000000000


root@AM62x:~# ll /dev/input/
total 0
drwxr-xr-x 2 root root      60 10月 28 04:58 by-path
crw-rw---- 1 root input 13, 64 10月 28 04:58 event0
crw-rw---- 1 root input 13, 65 10月 28 04:58 event1
crw-rw---- 1 root input 13, 66 10月 28 04:58 event2
lrwxrwxrwx 1 root root       6 10月 28 04:58 touchscreen0 -> event1
```

# 测试
```bash
root@AM62x:~# evtest /dev/input/event1
Input driver version is 1.0.1
Input device ID: bus 0x18 vendor 0xdead product 0xbeef version 0x28bb
Input device name: "goodix-ts"
Supported events:
  Event type 0 (EV_SYN)
  Event type 1 (EV_KEY)
  Event type 3 (EV_ABS)
    Event code 47 (ABS_MT_SLOT)
      Value      0
      Min        0
      Max       15
    Event code 48 (ABS_MT_TOUCH_MAJOR)
      Value      0
      Min        0
      Max      255
    Event code 50 (ABS_MT_WIDTH_MAJOR)
      Value      0
      Min        0
      Max      255
    Event code 53 (ABS_MT_POSITION_X)
      Value      0
      Min        0
      Max     1280
    Event code 54 (ABS_MT_POSITION_Y)
      Value      0
      Min        0
      Max      800
    Event code 57 (ABS_MT_TRACKING_ID)
      Value      0
      Min        0
      Max      255
Properties:
  Property type 1 (INPUT_PROP_DIRECT)
Testing ... (interrupt to exit)
Event: time 1698440627.041117, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value 0
Event: time 1698440627.041117, type 3 (EV_ABS), code 53 (ABS_MT_POSITION_X), value 853
Event: time 1698440627.041117, type 3 (EV_ABS), code 54 (ABS_MT_POSITION_Y), value 498
Event: time 1698440627.041117, type 3 (EV_ABS), code 48 (ABS_MT_TOUCH_MAJOR), value 11
Event: time 1698440627.041117, type 3 (EV_ABS), code 50 (ABS_MT_WIDTH_MAJOR), value 11
Event: time 1698440627.041117, -------------- SYN_REPORT ------------
Event: time 1698440627.137177, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value -1
Event: time 1698440627.137177, -------------- SYN_REPORT ------------
Event: time 1698440627.233268, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value 0
Event: time 1698440627.233268, type 3 (EV_ABS), code 53 (ABS_MT_POSITION_X), value 847
Event: time 1698440627.233268, type 3 (EV_ABS), code 54 (ABS_MT_POSITION_Y), value 462
Event: time 1698440627.233268, type 3 (EV_ABS), code 48 (ABS_MT_TOUCH_MAJOR), value 12
Event: time 1698440627.233268, type 3 (EV_ABS), code 50 (ABS_MT_WIDTH_MAJOR), value 12
Event: time 1698440627.233268, -------------- SYN_REPORT ------------
Event: time 1698440627.297326, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value -1
Event: time 1698440627.297326, -------------- SYN_REPORT ------------
Event: time 1698440627.393377, type 3 (EV_ABS), code 57 (ABS_MT_TRACKING_ID), value 0
Event: time 1698440627.393377, type 3 (EV_ABS), code 53 (ABS_MT_POSITION_X), value 858
Event: time 1698440627.393377, type 3 (EV_ABS), code 54 (ABS_MT_POSITION_Y), value 466
Event: time 1698440627.393377, type 3 (EV_ABS), code 48 (ABS_MT_TOUCH_MAJOR), value 11
Event: time 1698440627.393377, type 3 (EV_ABS), code 50 (ABS_MT_WIDTH_MAJOR), value 11

root@AM62x:~# hexdump /dev/input/touchscreen0
0000000 25fe 653c 0000 0000 6b1d 000e 0000 0000
0000010 0003 0039 0000 0000 25fe 653c 0000 0000
0000020 6b1d 000e 0000 0000 0003 0035 0397 0000
0000030 25fe 653c 0000 0000 6b1d 000e 0000 0000
0000040 0003 0036 0222 0000 25fe 653c 0000 0000
0000050 6b1d 000e 0000 0000 0000 0000 0000 0000
0000060 25ff 653c 0000 0000 a021 0000 0000 0000
0000070 0003 0039 ffff ffff 25ff 653c 0000 0000
0000080 a021 0000 0000 0000 0000 0000 0000 0000
0000090 25ff 653c 0000 0000 59fb 000c 0000 0000
00000a0 0003 0039 0000 0000 25ff 653c 0000 0000
00000b0 59fb 000c 0000 0000 0003 0035 03b9 0000
00000c0 25ff 653c 0000 0000 59fb 000c 0000 0000
00000d0 0003 0036 022d 0000 25ff 653c 0000 0000
00000e0 59fb 000c 0000 0000 0003 0030 000c 0000
00000f0 25ff 653c 0000 0000 59fb 000c 0000 0000
0000100 0003 0032 000c 0000 25ff 653c 0000 0000

```