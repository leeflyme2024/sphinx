# U-Boot

# Kernel
```bash
# show info
journalctl | grep snd
journalctl | grep sound

lsmod | grep snd

ls -l /proc/asound/
cat /proc/asound/cards

ls -l /dev/snd/

arecord -l
aplay -l

cat /sys/kernel/debug/clk/clk_summary | grep tlv320_mclk

# test i2c communication
i2cdetect -r -y 2
i2cdump -f -y 2 0x18 b

#dapm
ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/tlv320aic3x-codec.2-0018/dapm
ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/dapm
```

```bash
# 列出所有声卡和音频设备
arecord -l
aplay -l

# 列出所有PCM设备
arecord -L
aplay -L

# 查看声卡硬件参数
arecord -D hw:0,0 --dump-hw-params

# 检查当前音量和开关状态
amixer -c 0
amixer -c 0 contents     # 显示所有控制器的详细信息


# 确保录音输入没有静音
amixer -c 0 sset 'Capture' unmute
amixer -c 0 sset 'ADC' unmute

# 调整录音增益
amixer -c 0 sset 'Capture' 80%




ll /proc/asound
cat /proc/asound/version
cat /proc/asound/cards
cat /proc/asound/devices
cat /proc/asound/pcm
cat /proc/asound/timers


ll /proc/asound/card0/
ll /proc/asound/card0/pcm0c
ll /proc/asound/card0/pcm0c/sub0/
ll /proc/asound/card0/pcm0p
ll /proc/asound/card0/pcm0p/sub0/

cat /proc/asound/card0/id

cat /proc/asound/card0/pcm0c/info
cat /proc/asound/card0/pcm0c/sub0/status
cat /proc/asound/card0/pcm0c/sub0/prealloc
cat /proc/asound/card0/pcm0c/sub0/prealloc_max
cat /proc/asound/card0/pcm0c/sub0/hw_params
cat /proc/asound/card0/pcm0c/sub0/sw_params


cat /proc/asound/card0/pcm0p/info
cat /proc/asound/card0/pcm0p/sub0/status
cat /proc/asound/card0/pcm0p/sub0/prealloc
cat /proc/asound/card0/pcm0p/sub0/prealloc_max
cat /proc/asound/card0/pcm0p/sub0/hw_params
cat /proc/asound/card0/pcm0p/sub0/sw_params


ll /sys/kernel/debug/asoc/
cat /sys/kernel/debug/asoc/dais
cat /sys/kernel/debug/asoc/components

ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE
ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/2b10000.audio-controller
ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/2b10000.audio-controller/dapm/
ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/dapm
ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/dma:2b10000.audio-controller
ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/dma:2b10000.audio-controller/dapm
ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/tlv320aic3x.2-0018
ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/tlv320aic3x.2-0018/dapm/
cat /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/dapm_pop_time




arecord -f S16_LE -c 2 -r 22050 -d 10 -t wav record.wav

# 尝试使用更常见的采样率，如48000Hz
arecord -D hw:0,0 -f S16_LE -c 2 -r 48000 -d 10 -t wav record.wav
# 尝试使用单声道录音
arecord -D hw:0,0 -f S16_LE -c 1 -r 48000 -d 10 -t wav record.wav

# 录音
arecord -D hw:0,0 -f S16_LE -c 2 -r 48000 -d 10 -t wav record_48k.wav
# 播放
aplay -D hw:0,0 record_48k.wav



# 录音时指定更多参数
arecord -D hw:0,0 -f S16_LE -c 2 -r 48000 -d 10 -t wav --buffer-size=16384 --period-size=4096 record_48k.wav

# 播放时也使用相同配置
aplay -D hw:0,0 -f S16_LE -c 2 -r 48000 --buffer-size=16384 --period-size=4096 record_48k.wav




root@AM62x:~# arecord -D hw:0,0 -f S16_LE -c 2 -r 48000 -d 10 -t wav record.wav
Recording WAVE 'record.wav' : Signed 16 bit Little Endian, Rate 48000 Hz, Stereo
arecord: pcm_read:2221: read error: Input/output error
root@AM62x:~# arecord -D hw:0,0 -f S16_LE -c 2 -r 44100 -d 10 -t wav record.wav
Recording WAVE 'record.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo


arecord -D hw:0,0 -f S16_LE -c 2 -r 44100 -d 10 -v -v -v record_44k.wav
arecord -D hw:0,0 -f S16_LE -c 2 -r 44100 -d 10 --dump-hw-params record_44k.wav
arecord -D hw:0,0 -f S16_LE -c 2 -r 44100 -d 10 --fatal-errors -v -t wav record_44k.wav


arecord -D hw:0,0 -f S16_LE -c 2 -r 48000 -d 10 -v -v -v record_48k.wav
arecord -D hw:0,0 -f S16_LE -c 2 -r 48000 -d 10 --dump-hw-params record_48k.wav
arecord -D hw:0,0 -f S16_LE -c 2 -r 48000 -d 10 --fatal-errors -v -t wav record_48k.wav


# 使用48KHz采样率录音
arecord -D hw:0,0 -f S16_LE -c 2 -r 48000 --period-size=1024 --buffer-size=4096 -d 10 -t wav record_48k.wav

# 播放检查
aplay -D hw:0,0 record_48k.wav


# 44.1KHz录音
arecord -D hw:0,0 -f S16_LE -c 2 -r 44100 --period-size=1024 --buffer-size=4096 -d 10 -t wav record_44k.wav





arecord -D hw:0,0 -f S16_LE -c 2 -r 44100 -d 10 -t wav record_44k.wav
aplay -D hw:0,0 -f S16_LE -c 2 -r 44100 -v -v -v record_44k.wav



arecord -D hw:0,0 -f S16_LE -c 2 -r 44100 -d 10 -t wav -v -v -v SDK5.10_record_44k.wav
aplay -D hw:0,0 -f S16_LE -c 2 -r 44100 -t wav -v -v -v SDK5.10_record_44k.wav



arecord -D hw:0,0 -f S16_LE -c 2 -r 22050 -d 10 -t wav -v -v -v SDK5.10_record_22k.wav
aplay -D hw:0,0 -f S16_LE -c 2 -r 22050 -t wav -v -v -v SDK5.10_record_22k.wav




# buffer_size 设为 period_size 的4倍
arecord -D plughw:0,0 -f S16_LE -c 2 -r 44100 -d 10 --period-size=4096 --buffer-size=16384 -t wav  -v -v -v record_test.wav
aplay -D plughw:0,0 -f S16_LE -c 2 -r 44100 -t wav -v -v -v record_test.wav

arecord -D plughw:0,0 -f S16_LE -c 2 -r 44100 -d 10 --period-size=8192 --buffer-size=32768 -t wav  -v -v -v record_test.wav
aplay -D plughw:0,0 -f S16_LE -c 2 -r 44100 -t wav -v -v -v record_test.wav
```

## 声卡
```bash
root@AM62x:~# dmesg | grep snd

root@AM62x:~# dmesg | grep sound
[    0.734549]   No soundcards found.
[    5.336987] asoc-simple-card sound: using DT '/sound' for 'simple-audio-card,hp-det' GPIO lookup
[    5.338739] of_get_named_gpiod_flags: can't parse 'simple-audio-card,hp-det-gpios' property of node '/sound[0]'
[    5.338795] of_get_named_gpiod_flags: can't parse 'simple-audio-card,hp-det-gpio' property of node '/sound[0]'
[    5.338811] asoc-simple-card sound: using lookup tables for GPIO lookup
[    5.338820] asoc-simple-card sound: No GPIO consumer simple-audio-card,hp-det found
[    5.338830] asoc-simple-card sound: using DT '/sound' for 'simple-audio-card,mic-det' GPIO lookup
[    5.338847] of_get_named_gpiod_flags: can't parse 'simple-audio-card,mic-det-gpios' property of node '/sound[0]'
[    5.338863] of_get_named_gpiod_flags: can't parse 'simple-audio-card,mic-det-gpio' property of node '/sound[0]'
[    5.338872] asoc-simple-card sound: using lookup tables for GPIO lookup
[    5.338876] asoc-simple-card sound: No GPIO consumer simple-audio-card,mic-det found

```

```bash
root@AM62x:~# ls -l /proc/asound/
total 0
lrwxrwxrwx 1 root root 5 10月 21 23:20 AM62xSKEVMSLAVE -> card0
dr-xr-xr-x 5 root root 0 10月 21 23:20 card0
-r--r--r-- 1 root root 0 10月 21 23:20 cards
-r--r--r-- 1 root root 0 10月 21 23:20 devices
-r--r--r-- 1 root root 0 10月 21 23:20 pcm
-r--r--r-- 1 root root 0 10月 21 23:20 timers
-r--r--r-- 1 root root 0 10月 21 23:20 version

root@AM62x:~# cat /proc/asound/cards
 0 [AM62xSKEVMSLAVE]: simple-card - AM62x-SKEVM-SLAVE
                      AM62x-SKEVM-SLAVE

root@AM62x:~# ls -l /dev/snd/
total 0
drwxr-xr-x 2 root root       60 10月 21 23:12 by-path
crw-rw---- 1 root audio 116,  4 10月 21 23:12 controlC0
crw-rw---- 1 root audio 116,  3 10月 21 23:12 pcmC0D0c
crw-rw---- 1 root audio 116,  2 10月 21 23:12 pcmC0D0p
crw-rw---- 1 root audio 116, 33 10月 21 23:12 timer
```

```bash
root@AM62x:~# arecord -l
**** List of CAPTURE Hardware Devices ****
card 0: AM62xSKEVMSLAVE [AM62x-SKEVM-SLAVE], device 0: 2b10000.audio-controller-tlv320aic3x-hifi tlv320aic3x-hifi-0 [2b10000.audio-controller-tlv320aic3x-hifi tlv320aic3x-hifi-0]
  Subdevices: 1/1
  Subdevice #0: subdevice #0

root@AM62x:~# aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: AM62xSKEVMSLAVE [AM62x-SKEVM-SLAVE], device 0: 2b10000.audio-controller-tlv320aic3x-hifi tlv320aic3x-hifi-0 [2b10000.audio-controller-tlv320aic3x-hifi tlv320aic3x-hifi-0]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```

## 驱动
```bash
root@AM62x:~# lsmod | grep snd
snd_soc_simple_card          16384  0
snd_soc_simple_card_utils    20480  1 snd_soc_simple_card
snd_soc_davinci_mcasp        28672  2
snd_soc_ti_udma              12288  1 snd_soc_davinci_mcasp
snd_soc_ti_edma              12288  1 snd_soc_davinci_mcasp
snd_soc_ti_sdma              12288  1 snd_soc_davinci_mcasp
snd_soc_tlv320aic3x_i2c      12288  1
snd_soc_tlv320aic3x          65536  1 snd_soc_tlv320aic3x_i2c
```

```bash
root@AM62x:/# ll /sys/module/snd*
/sys/module/snd:
total 0
drwxr-xr-x 2 root root    0 10月 23 01:01 parameters
--w------- 1 root root 4096 10月 23 00:11 uevent

/sys/module/snd_pcm:
total 0
drwxr-xr-x 2 root root    0 10月 23 01:01 parameters
--w------- 1 root root 4096 10月 23 00:11 uevent

/sys/module/snd_soc_core:
total 0
drwxr-xr-x 2 root root    0 10月 23 01:01 parameters
--w------- 1 root root 4096 10月 23 00:11 uevent

/sys/module/snd_soc_davinci_mcasp:
total 0
-r--r--r-- 1 root root 4096 10月 23 00:11 coresize
drwxr-xr-x 2 root root    0 10月 23 01:01 drivers
drwxr-xr-x 2 root root    0 10月 23 00:11 holders
-r--r--r-- 1 root root 4096 10月 23 01:01 initsize
-r--r--r-- 1 root root 4096 10月 23 00:11 initstate
-r--r--r-- 1 root root 4096 10月 23 00:11 refcnt
-r--r--r-- 1 root root 4096 10月 23 01:01 taint
--w------- 1 root root 4096 10月 23 00:11 uevent

/sys/module/snd_soc_simple_card:
total 0
-r--r--r-- 1 root root 4096 10月 23 00:11 coresize
drwxr-xr-x 2 root root    0 10月 23 01:01 drivers
drwxr-xr-x 2 root root    0 10月 23 00:11 holders
-r--r--r-- 1 root root 4096 10月 23 01:01 initsize
-r--r--r-- 1 root root 4096 10月 23 00:11 initstate
-r--r--r-- 1 root root 4096 10月 23 00:11 refcnt
-r--r--r-- 1 root root 4096 10月 23 01:01 taint
--w------- 1 root root 4096 10月 23 00:11 uevent

/sys/module/snd_soc_simple_card_utils:
total 0
-r--r--r-- 1 root root 4096 10月 23 00:11 coresize
drwxr-xr-x 2 root root    0 10月 23 00:11 holders
-r--r--r-- 1 root root 4096 10月 23 01:01 initsize
-r--r--r-- 1 root root 4096 10月 23 01:01 initstate
-r--r--r-- 1 root root 4096 10月 23 00:11 refcnt
-r--r--r-- 1 root root 4096 10月 23 01:01 taint
--w------- 1 root root 4096 10月 23 00:11 uevent

/sys/module/snd_soc_ti_edma:
total 0
-r--r--r-- 1 root root 4096 10月 23 00:11 coresize
drwxr-xr-x 2 root root    0 10月 23 00:11 holders
-r--r--r-- 1 root root 4096 10月 23 01:01 initsize
-r--r--r-- 1 root root 4096 10月 23 01:01 initstate
-r--r--r-- 1 root root 4096 10月 23 00:11 refcnt
-r--r--r-- 1 root root 4096 10月 23 01:01 taint
--w------- 1 root root 4096 10月 23 00:11 uevent

/sys/module/snd_soc_ti_sdma:
total 0
-r--r--r-- 1 root root 4096 10月 23 00:11 coresize
drwxr-xr-x 2 root root    0 10月 23 00:11 holders
-r--r--r-- 1 root root 4096 10月 23 01:01 initsize
-r--r--r-- 1 root root 4096 10月 23 01:01 initstate
-r--r--r-- 1 root root 4096 10月 23 00:11 refcnt
-r--r--r-- 1 root root 4096 10月 23 01:01 taint
--w------- 1 root root 4096 10月 23 00:11 uevent

/sys/module/snd_soc_ti_udma:
total 0
-r--r--r-- 1 root root 4096 10月 23 00:11 coresize
drwxr-xr-x 2 root root    0 10月 23 00:11 holders
-r--r--r-- 1 root root 4096 10月 23 01:01 initsize
-r--r--r-- 1 root root 4096 10月 23 01:01 initstate
-r--r--r-- 1 root root 4096 10月 23 00:11 refcnt
-r--r--r-- 1 root root 4096 10月 23 01:01 taint
--w------- 1 root root 4096 10月 23 00:11 uevent

/sys/module/snd_soc_tlv320aic3x:
total 0
-r--r--r-- 1 root root 4096 10月 23 00:11 coresize
drwxr-xr-x 2 root root    0 10月 23 00:11 holders
-r--r--r-- 1 root root 4096 10月 23 01:01 initsize
-r--r--r-- 1 root root 4096 10月 23 01:01 initstate
-r--r--r-- 1 root root 4096 10月 23 00:11 refcnt
-r--r--r-- 1 root root 4096 10月 23 01:01 taint
--w------- 1 root root 4096 10月 23 00:11 uevent

/sys/module/snd_soc_tlv320aic3x_i2c:
total 0
-r--r--r-- 1 root root 4096 10月 23 00:11 coresize
drwxr-xr-x 2 root root    0 10月 23 01:01 drivers
drwxr-xr-x 2 root root    0 10月 23 00:11 holders
-r--r--r-- 1 root root 4096 10月 23 01:01 initsize
-r--r--r-- 1 root root 4096 10月 23 00:11 initstate
-r--r--r-- 1 root root 4096 10月 23 00:11 refcnt
-r--r--r-- 1 root root 4096 10月 23 01:01 taint
--w------- 1 root root 4096 10月 23 00:11 uevent

/sys/module/snd_timer:
total 0
drwxr-xr-x 2 root root    0 10月 23 01:01 parameters
--w------- 1 root root 4096 10月 23 00:11 uevent



root@AM62x:/# ll /sys/module/snd*/parameters
/sys/module/snd/parameters:
total 0
-r--r--r-- 1 root root 4096 10月 23 01:01 cards_limit
-rw-r--r-- 1 root root 4096 10月 23 01:01 debug
-r--r--r-- 1 root root 4096 10月 23 01:01 major
-r--r--r-- 1 root root 4096 10月 23 01:01 max_user_ctl_alloc_size
-r--r--r-- 1 root root 4096 10月 23 01:01 slots

/sys/module/snd_pcm/parameters:
total 0
-rw-r--r-- 1 root root 4096 10月 23 01:01 max_alloc_per_card
-r--r--r-- 1 root root 4096 10月 23 01:01 maximum_substreams
-r--r--r-- 1 root root 4096 10月 23 01:01 preallocate_dma

/sys/module/snd_soc_core/parameters:
total 0
-r--r--r-- 1 root root 4096 10月 23 01:01 prealloc_buffer_size_kbytes

/sys/module/snd_timer/parameters:
total 0
-r--r--r-- 1 root root 4096 10月 23 01:01 timer_limit
-r--r--r-- 1 root root 4096 10月 23 01:01 timer_tstamp_monotonic
```

## 时钟
```bash
root@AM62x:~# cat /sys/kernel/debug/clk/clk_summary | grep tlv320_mclk
 tlv320_mclk                         0       0        0        12288000    0          0     50000      Y   simple-audio-card,codec         no_connection_id
```


## i2c
```bash
root@AM62x:~# i2cdetect -r -y 2
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- --
10: -- -- -- -- 14 -- -- -- UU -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --

root@AM62x:~# i2cdump -f -y 2 0x18 b
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f    0123456789abcdef
00: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
10: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
20: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
30: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
40: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
50: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
60: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
70: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
80: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
90: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
a0: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
b0: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
c0: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
d0: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
e0: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
f0: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
```


## dapm
```bash
root@AM62x:~# ll /proc/asound/*
lrwxrwxrwx 1 root root 5 10月 23 03:04 /proc/asound/AM62xSKEVMSLAVE -> card0
-r--r--r-- 1 root root 0 10月 23 03:04 /proc/asound/cards
-r--r--r-- 1 root root 0 10月 23 03:04 /proc/asound/devices
-r--r--r-- 1 root root 0 10月 23 03:04 /proc/asound/pcm
-r--r--r-- 1 root root 0 10月 23 03:04 /proc/asound/timers
-r--r--r-- 1 root root 0 10月 23 03:04 /proc/asound/version

root@AM62x:~# ll /proc/asound/card0/*
-r--r--r-- 1 root root 0 10月 23 03:04 /proc/asound/card0/id

/proc/asound/card0/pcm0c:
total 0
-r--r--r-- 1 root root 0 10月 23 03:05 info
dr-xr-xr-x 9 root root 0 10月 23 03:05 sub0
-rw-r--r-- 1 root root 0 10月 23 03:05 xrun_debug

/proc/asound/card0/pcm0p:
total 0
-r--r--r-- 1 root root 0 10月 23 03:05 info
dr-xr-xr-x 9 root root 0 10月 23 03:05 sub0
-rw-r--r-- 1 root root 0 10月 23 03:05 xrun_debug


root@AM62x:~# ll /proc/asound/card0/pcm0c/*
-r--r--r-- 1 root root 0 10月 23 03:05 /proc/asound/card0/pcm0c/info
-rw-r--r-- 1 root root 0 10月 23 03:05 /proc/asound/card0/pcm0c/xrun_debug

/proc/asound/card0/pcm0c/sub0:
total 0
-r--r--r-- 1 root root 0 10月 23 03:07 hw_params
-r--r--r-- 1 root root 0 10月 23 03:07 info
-rw-r--r-- 1 root root 0 10月 23 03:07 prealloc
-r--r--r-- 1 root root 0 10月 23 03:07 prealloc_max
-r--r--r-- 1 root root 0 10月 23 03:07 status
-r--r--r-- 1 root root 0 10月 23 03:07 sw_params
--w------- 1 root root 0 10月 23 03:07 xrun_injection


root@AM62x:~# ll /proc/asound/card0/pcm0p/*
-r--r--r-- 1 root root 0 10月 23 03:05 /proc/asound/card0/pcm0p/info
-rw-r--r-- 1 root root 0 10月 23 03:05 /proc/asound/card0/pcm0p/xrun_debug

/proc/asound/card0/pcm0p/sub0:
total 0
-r--r--r-- 1 root root 0 10月 23 03:07 hw_params
-r--r--r-- 1 root root 0 10月 23 03:07 info
-rw-r--r-- 1 root root 0 10月 23 03:07 prealloc
-r--r--r-- 1 root root 0 10月 23 03:07 prealloc_max
-r--r--r-- 1 root root 0 10月 23 03:07 status
-r--r--r-- 1 root root 0 10月 23 03:07 sw_params
--w------- 1 root root 0 10月 23 03:07 xrun_injection
```


```bash
root@AM62x:~# ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/tlv320aic3x.2-0018/dapm/
total 0
-r--r--r-- 1 root root 0 10月 21 23:12  bias_level
-r--r--r-- 1 root root 0 10月 21 23:12  Capture
-r--r--r-- 1 root root 0 10月 21 23:12  Detection
-r--r--r-- 1 root root 0 10月 21 23:12 'DMic Rate 128'
-r--r--r-- 1 root root 0 10月 21 23:12 'DMic Rate 32'
-r--r--r-- 1 root root 0 10月 21 23:12 'DMic Rate 64'
-r--r--r-- 1 root root 0 10月 21 23:12 'GPIO1 dmic modclk'
-r--r--r-- 1 root root 0 10月 21 23:12  HPLCOM
-r--r--r-- 1 root root 0 10月 21 23:12  HPLOUT
-r--r--r-- 1 root root 0 10月 21 23:12  HPRCOM
-r--r--r-- 1 root root 0 10月 21 23:12  HPROUT
-r--r--r-- 1 root root 0 10月 21 23:12 'Left ADC'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left DAC'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left DAC Mux'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left HP Com'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left HPCOM Mixer'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left HPCOM Mux'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left HP Mixer'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left HP Out'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left Line1L Mux'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left Line1R Mux'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left Line2L Mux'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left Line Mixer'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left Line Out'
-r--r--r-- 1 root root 0 10月 21 23:12 'Left PGA Mixer'
-r--r--r-- 1 root root 0 10月 21 23:12  LINE1L
-r--r--r-- 1 root root 0 10月 21 23:12  LINE1R
-r--r--r-- 1 root root 0 10月 21 23:12  LINE2L
-r--r--r-- 1 root root 0 10月 21 23:12  LINE2R
-r--r--r-- 1 root root 0 10月 21 23:12  LLOUT
-r--r--r-- 1 root root 0 10月 21 23:12  MIC3L
-r--r--r-- 1 root root 0 10月 21 23:12  MIC3R
-r--r--r-- 1 root root 0 10月 21 23:12 'Mic Bias'
-r--r--r-- 1 root root 0 10月 21 23:12  MONO_LOUT
-r--r--r-- 1 root root 0 10月 21 23:12 'Mono Mixer'
-r--r--r-- 1 root root 0 10月 21 23:12 'Mono Out'
-r--r--r-- 1 root root 0 10月 21 23:12  Playback
-r--r--r-- 1 root root 0 10月 21 23:12 'Right ADC'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right DAC'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right DAC Mux'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right HP Com'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right HPCOM Mixer'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right HPCOM Mux'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right HP Mixer'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right HP Out'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right Line1L Mux'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right Line1R Mux'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right Line2R Mux'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right Line Mixer'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right Line Out'
-r--r--r-- 1 root root 0 10月 21 23:12 'Right PGA Mixer'
-r--r--r-- 1 root root 0 10月 21 23:12  RLOUT
```

```bash
root@AM62x:~# ll /sys/kernel/debug/asoc/AM62x-SKEVM-SLAVE/dapm
total 0
-r--r--r-- 1 root root 0 10月 21 23:12  bias_level
-r--r--r-- 1 root root 0 10月 21 23:12 'Headphone Jack'
-r--r--r-- 1 root root 0 10月 21 23:12 'Microphone Jack'
```

## 内核选项

```bash
root@AM62x:~# zcat /proc/config.gz | grep CONFIG_SND_VERBOSE_PRINTK
CONFIG_SND_VERBOSE_PRINTK=y
root@AM62x:~# zcat /proc/config.gz | grep CONFIG_SND_DEBUG
CONFIG_SND_DEBUG=y
CONFIG_SND_DEBUG_VERBOSE=y
root@AM62x:~# zcat /proc/config.gz | grep CONFIG_SND_TEST_COMPONENT
CONFIG_SND_TEST_COMPONENT=y
```


### CONFIG_SND_PROC_FS

### CONFIG_SND_VERBOSE_PROCFS

### CONFIG_SND_VERBOSE_PRINTK
要在 Linux 内核中启用 `CONFIG_SND_VERBOSE_PRINTK` 以调试 ALSA（高级 Linux 声音体系结构）相关问题，您需要按照以下步骤进行：
1. **配置内核选项：**
    - 在内核源码目录中，运行 `make menuconfig` 命令以进入内核配置界面。
    - 导航至以下路径：`Device Drivers` → `Sound card support` → `Advanced Linux Sound Architecture`
    - 找到 `Verbose printk` 选项（对应于 `CONFIG_SND_VERBOSE_PRINTK`），将其设置为 `Y`（启用）。
2. **重新编译内核：**
    - 保存配置并退出配置界面。
    - 按照您的系统环境，重新编译并安装内核。
3. **设置内核日志级别：**
    - 在系统运行时，内核的日志级别决定了哪些消息会输出到控制台。
    - `printk` 函数有 8 个日志级别，定义在 `<linux/kernel.h>` 中。
    - 要查看当前的日志级别，运行：
        `cat /proc/sys/kernel/printk`
    - 输出示例：`4 4 1 7`。第一个数字表示当前的控制台日志级别。
    - 要调整日志级别，例如设置为 8（显示所有级别的日志），运行：
        `echo 8 > /proc/sys/kernel/printk`
    - 这将确保所有级别的日志消息都输出到控制台。

通过以上步骤，您可以启用详细的 ALSA 日志消息，有助于调试声音相关的问题。

### CONFIG_SND_DEBUG
启用 `CONFIG_SND_DEBUG` 选项会在内核中激活 ALSA（高级Linux声音体系结构）的调试代码。这将使内核能够记录更多的调试信息，有助于开发人员诊断和解决与声音子系统相关的问题。

启用该选项后，您可以通过以下方式进行调试：

1. **内核日志消息**：ALSA 的调试宏（如 `snd_printd`）会在内核日志中生成调试信息。这些消息可以使用 `dmesg` 命令或查看 `/var/log/kern.log` 文件来访问。请注意，只有在启用了 `CONFIG_SND_DEBUG` 时，这些调试消息才会被记录。
    [kernel.org](https://www.kernel.org/doc/html/v4.15/driver-api/sound.html?utm_source=chatgpt.com)
2. **调试宏**：ALSA 提供了多个调试宏，例如：
    
    - `snd_BUG()`：在启用 `CONFIG_SND_DEBUG` 时调用 `WARN()`，否则忽略。
    - `snd_BUG_ON(cond)`：在启用 `CONFIG_SND_DEBUG` 时，如果条件 `cond` 为真，则行为类似于 `WARN_ON`，否则仅评估条件并返回其值。
    - `snd_printd(fmt, ...)`：用于调试的打印函数，行为类似于 `snd_printk()`，但仅在启用了 `CONFIG_SND_DEBUG` 时有效。
    - `snd_printdd(fmt, ...)`：用于更详细的调试打印，仅在启用了 `CONFIG_SND_DEBUG_VERBOSE` 时有效。
这些宏在调试过程中非常有用，可以帮助开发人员识别和解决问题。
[kernel.org](https://www.kernel.org/doc/html/v4.15/driver-api/sound.html?utm_source=chatgpt.com)
请注意，启用 `CONFIG_SND_DEBUG` 可能会增加内核的内存占用和代码大小，因此在生产环境中应谨慎使用。在调试完成后，建议将其禁用以优化性能。

### CONFIG_SND_DEBUG_VERBOSE

### CONFIG_SND_TEST_COMPONENT
启用 `CONFIG_SND_TEST_COMPONENT` 选项后，您可以使用 ALSA 提供的测试组件驱动程序来协助调试和测试音频子系统。该选项主要用于 ALSA 开发者或驱动程序开发人员，以验证音频框架的功能和性能。

**启用方法：**

1. **配置内核：** 在内核配置菜单中，导航至以下路径：
    
    - `Device Drivers` → `Sound card support` → `Advanced Linux Sound Architecture` → `ALSA for SoC audio support` → `ASoC Test component sound support`
    - 将 `ASoC Test component sound support`（对应于 `CONFIG_SND_TEST_COMPONENT`）设置为编译为内核模块（`M`）或内置（`Y`）。
2. **编译内核：** 保存配置并重新编译内核，以包含测试组件支持。
    
3. **加载模块：** 如果您将其配置为模块，使用以下命令加载：
    
    `modprobe snd-soc-test-component`

**使用方法：**

启用测试组件后，您可以通过以下方式进行调试：

- **创建测试音频路径：** 使用测试组件，您可以在不依赖实际音频硬件的情况下创建虚拟音频路径。这对于验证音频路由和处理非常有用。
    
- **调试音频流：** 通过将音频数据路由到测试组件，您可以捕获和分析音频流，帮助识别和解决潜在的问题。
    

请注意，`CONFIG_SND_TEST_COMPONENT` 主要用于开发和调试目的。在生产环境中，不建议启用此选项。

如果您需要更详细的指导，建议查阅 ALSA 官方文档或相关的开发者资源，以获取关于测试组件的深入信息。




# 5.10
```bash
root@AM62x:~# arecord -D hw:0,0 --dump-hw-params
Warning: Some sources (like microphones) may produce inaudiable results
         with 8-bit sampling. Use '-f' argument to increase resolution
         e.g. '-f S16_LE'.
Recording WAVE 'stdin' : Unsigned 8 bit, Rate 8000 Hz, Mono
HW Params of device "hw:0,0":
--------------------
ACCESS:  MMAP_INTERLEAVED RW_INTERLEAVED
FORMAT:  S16_LE S24_LE S32_LE S24_3LE
SUBFORMAT:  STD
SAMPLE_BITS: [16 32]
FRAME_BITS: [32 64]
CHANNELS: 2
RATE: [8000 96000]
PERIOD_TIME: (666 2048000]
PERIOD_SIZE: [64 16384]
PERIOD_BYTES: [256 65536]
PERIODS: [2 2048]
BUFFER_TIME: (1333 16384000]
BUFFER_SIZE: [128 131072]
BUFFER_BYTES: [512 524288]
TICK_TIME: ALL
--------------------
arecord: set_params:1352: Sample format non available
Available formats:
- S16_LE
- S24_LE
- S32_LE
- S24_3LE
```

```bash
root@AM62x:~# arecord -D hw:0,0 -f S16_LE -c 2 -r 22050 -d 10 -t wav -v -v -v SDK5.10_record_44k.wav
Recording WAVE 'SDK5.10_record_44k.wav' : Signed 16 bit Little Endian, Rate 22050 Hz, Stereo
Hardware PCM card 0 'AM62x-SKEVM-SLAVE' device 0 subdevice 0
Its setup is:
  stream       : CAPTURE
  access       : RW_INTERLEAVED
  format       : S16_LE
  subformat    : STD
  channels     : 2
  rate         : 22050
  exact rate   : 22050 (22050/1)
  msbits       : 16
  buffer_size  : 11024
  period_size  : 2756
  period_time  : 124988
  tstamp_mode  : NONE
  tstamp_type  : MONOTONIC
  period_step  : 1
  avail_min    : 2756
  period_event : 0
  start_threshold  : 1
  stop_threshold   : 11024
  silence_threshold: 0
  silence_size : 0
  boundary     : 6205960286516543488
  appl_ptr     : 0
  hw_ptr       : 0
Max peak (5512 samples): 0x00004e7c #############        61%
Max peak (5512 samples): 0x00001054 ###                  12%
Max peak (5512 samples): 0x00000832 ##                   6%
Max peak (5512 samples): 0x00000624 #                    4%
Max peak (5512 samples): 0x000003ef #                    3%
Max peak (5512 samples): 0x00000387 #                    2%
Max peak (5512 samples): 0x0000028f #                    1%
Max peak (5512 samples): 0x00000383 #                    2%
Max peak (5512 samples): 0x000006f9 ##                   5%
Max peak (5512 samples): 0x0000070e ##                   5%
Max peak (5512 samples): 0x0000048d #                    3%
Max peak (5512 samples): 0x00000451 #                    3%
Max peak (5512 samples): 0x00000625 #                    4%
Max peak (5512 samples): 0x000003be #                    2%
Max peak (5512 samples): 0x00000965 ##                   7%
Max peak (5512 samples): 0x00000904 ##                   7%
Max peak (5512 samples): 0x0000067a ##                   5%
Max peak (5512 samples): 0x000004df #                    3%
Max peak (5512 samples): 0x00000608 #                    4%
Max peak (5512 samples): 0x00000579 #                    4%
Max peak (5512 samples): 0x00000f76 ###                  12%
Max peak (5512 samples): 0x00000a1a ##                   7%
Max peak (5512 samples): 0x000004df #                    3%
Max peak (5512 samples): 0x000003f9 #                    3%
Max peak (5512 samples): 0x0000144d ####                 15%
Max peak (5512 samples): 0x00000932 ##                   7%
Max peak (5512 samples): 0x0000064d #                    4%
Max peak (5512 samples): 0x000008a1 ##                   6%
Max peak (5512 samples): 0x00003a51 ##########           45%
Max peak (5512 samples): 0x000005e8 #                    4%
Max peak (5512 samples): 0x00001a66 #####                20%
Max peak (5512 samples): 0x00000d92 ###                  10%
Max peak (5512 samples): 0x00000dfe ###                  10%
Max peak (5512 samples): 0x000008b8 ##                   6%
Max peak (5512 samples): 0x000011da ###                  13%
Max peak (5512 samples): 0x00000d29 ###                  10%
Max peak (5512 samples): 0x00001752 ####                 18%
Max peak (5512 samples): 0x00003514 #########            41%
Max peak (5512 samples): 0x000030c9 ########             38%
Max peak (5512 samples): 0x00000c7f ##                   9%
Max peak (5512 samples): 0x00001ca2 #####                22%
Max peak (5512 samples): 0x00000d61 ###                  10%
Max peak (5512 samples): 0x000008fc ##                   7%
Max peak (5512 samples): 0x00001d34 #####                22%
Max peak (5512 samples): 0x0000358e #########            41%
Max peak (5512 samples): 0x000016a7 ####                 17%
Max peak (5512 samples): 0x00001ac3 #####                20%
Max peak (5512 samples): 0x00003ead ##########           48%
Max peak (5512 samples): 0x00001e8e #####                23%
Max peak (5512 samples): 0x00001624 ####                 17%
Max peak (5512 samples): 0x00001cd9 #####                22%
Max peak (5512 samples): 0x00001985 ####                 19%
Max peak (5512 samples): 0x000016ba ####                 17%
Max peak (5512 samples): 0x00001a97 #####                20%
Max peak (5512 samples): 0x0000120e ###                  14%
Max peak (5512 samples): 0x00000cfe ###                  10%
Max peak (5512 samples): 0x0000103e ###                  12%
Max peak (5512 samples): 0x00002d6c ########             35%
Max peak (5512 samples): 0x00000faf ###                  12%
Max peak (5512 samples): 0x00000964 ##                   7%
Max peak (5512 samples): 0x000008e8 ##                   6%
Max peak (5512 samples): 0x00000a45 ##                   8%
Max peak (5512 samples): 0x0000078b ##                   5%
Max peak (5512 samples): 0x00001d2b #####                22%
Max peak (5512 samples): 0x000013a3 ####                 15%
Max peak (5512 samples): 0x00000e20 ###                  11%
Max peak (5512 samples): 0x0000106e ###                  12%
Max peak (5512 samples): 0x0000180a ####                 18%
Max peak (5512 samples): 0x00001547 ####                 16%
Max peak (5512 samples): 0x000016b9 ####                 17%
Max peak (5512 samples): 0x00001002 ###                  12%
Max peak (5512 samples): 0x00000e10 ###                  10%
Max peak (5512 samples): 0x000011db ###                  13%
Max peak (5512 samples): 0x00001b92 #####                21%
Max peak (5512 samples): 0x000013f0 ####                 15%
Max peak (5512 samples): 0x00000a14 ##                   7%
Max peak (5512 samples): 0x00000864 ##                   6%
Max peak (5512 samples): 0x00000ea1 ###                  11%
Max peak (5512 samples): 0x00000ef5 ###                  11%
Max peak (5512 samples): 0x0000152d ####                 16%
Max peak (5512 samples): 0x000011fd ###                  14%
```

```bash
root@AM62x:~# arecord -D hw:0,0 -f S16_LE -c 2 -r 44100 -d 10 -t wav -v -v -v SDK5.10_record_44k.wav
Recording WAVE 'SDK5.10_record_44k.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
Hardware PCM card 0 'AM62x-SKEVM-SLAVE' device 0 subdevice 0
Its setup is:
  stream       : CAPTURE
  access       : RW_INTERLEAVED
  format       : S16_LE
  subformat    : STD
  channels     : 2
  rate         : 44100
  exact rate   : 44100 (44100/1)
  msbits       : 16
  buffer_size  : 22052
  period_size  : 5513
  period_time  : 125011
  tstamp_mode  : NONE
  tstamp_type  : MONOTONIC
  period_step  : 1
  avail_min    : 5513
  period_event : 0
  start_threshold  : 1
  stop_threshold   : 22052
  silence_threshold: 0
  silence_size : 0
  boundary     : 6207086186423386112
  appl_ptr     : 0
  hw_ptr       : 0
Max peak (11026 samples): 0x00000418 #                    3%
Max peak (11026 samples): 0x00000f7a ###                  12%
Max peak (11026 samples): 0x00000df7 ###                  10%
Max peak (11026 samples): 0x00002e72 ########             36%
Max peak (11026 samples): 0x0000110d ###                  13%
Max peak (11026 samples): 0x000012f6 ###                  14%
Max peak (11026 samples): 0x00000fd8 ###                  12%
Max peak (11026 samples): 0x00007b41 #################### 96%
Max peak (11026 samples): 0x00001286 ###                  14%
Max peak (11026 samples): 0x00001660 ####                 17%
Max peak (11026 samples): 0x000029ea #######              32%
Max peak (11026 samples): 0x00000869 ##                   6%
Max peak (11026 samples): 0x000008d0 ##                   6%
Max peak (11026 samples): 0x000016e5 ####                 17%
Max peak (11026 samples): 0x00001097 ###                  12%
Max peak (11026 samples): 0x0000103b ###                  12%
Max peak (11026 samples): 0x00001245 ###                  14%
Max peak (11026 samples): 0x00006c6f #################    84%
Max peak (11026 samples): 0x000023f2 ######               28%
Max peak (11026 samples): 0x00002433 ######               28%
Max peak (11026 samples): 0x0000209e ######               25%
Max peak (11026 samples): 0x00001f47 #####                24%
Max peak (11026 samples): 0x00002420 ######               28%
Max peak (11026 samples): 0x00001a5a #####                20%
Max peak (11026 samples): 0x000010c3 ###                  13%
Max peak (11026 samples): 0x00002b84 #######              33%
Max peak (11026 samples): 0x0000714d ##################   88%
Max peak (11026 samples): 0x00001a7f #####                20%
Max peak (11026 samples): 0x00001b59 #####                21%
Max peak (11026 samples): 0x00003b2b ##########           46%
Max peak (11026 samples): 0x00006af0 #################    83%
Max peak (11026 samples): 0x000034ca #########            41%
Max peak (11026 samples): 0x00001cb7 #####                22%
Max peak (11026 samples): 0x000016de ####                 17%
Max peak (11026 samples): 0x000017a7 ####                 18%
Max peak (11026 samples): 0x00003664 #########            42%
Max peak (11026 samples): 0x00001990 ####                 19%
Max peak (11026 samples): 0x00006430 ################     78%
Max peak (11026 samples): 0x00002ccf ########             35%
Max peak (11026 samples): 0x00002d8e ########             35%
Max peak (11026 samples): 0x0000362f #########            42%
Max peak (11026 samples): 0x00001c31 #####                22%
Max peak (11026 samples): 0x000028c8 #######              31%
Max peak (11026 samples): 0x00002936 #######              32%
Max peak (11026 samples): 0x00001caa #####                22%
Max peak (11026 samples): 0x0000672c #################    80%
Max peak (11026 samples): 0x000013ea ####                 15%
Max peak (11026 samples): 0x00001a85 #####                20%
Max peak (11026 samples): 0x00001a31 #####                20%
Max peak (11026 samples): 0x000024c9 ######               28%
Max peak (11026 samples): 0x00000b2b ##                   8%
Max peak (11026 samples): 0x000013a6 ####                 15%
Max peak (11026 samples): 0x00001dde #####                23%
Max peak (11026 samples): 0x000020bc ######               25%
Max peak (11026 samples): 0x0000144a ####                 15%
Max peak (11026 samples): 0x00000d3a ###                  10%
Max peak (11026 samples): 0x000073d2 ###################  90%
Max peak (11026 samples): 0x00001c29 #####                22%
Max peak (11026 samples): 0x000019d6 #####                20%
Max peak (11026 samples): 0x00001cd2 #####                22%
Max peak (11026 samples): 0x00001429 ####                 15%
Max peak (11026 samples): 0x00002b9d #######              34%
Max peak (11026 samples): 0x00001814 ####                 18%
Max peak (11026 samples): 0x000019ba #####                20%
Max peak (11026 samples): 0x000023df ######               28%
Max peak (11026 samples): 0x0000709f ##################   87%
Max peak (11026 samples): 0x00001b92 #####                21%
Max peak (11026 samples): 0x00001562 ####                 16%
Max peak (11026 samples): 0x00002b42 #######              33%
Max peak (11026 samples): 0x00001dad #####                23%
Max peak (11026 samples): 0x0000233e ######               27%
Max peak (11026 samples): 0x00002f8c ########             37%
Max peak (11026 samples): 0x00001344 ####                 15%
Max peak (11026 samples): 0x0000142b ####                 15%
Max peak (11026 samples): 0x00001400 ####                 15%
Max peak (11026 samples): 0x0000154f ####                 16%
Max peak (11026 samples): 0x00006fcc ##################   87%
Max peak (11026 samples): 0x00002235 ######               26%
Max peak (11026 samples): 0x00002726 #######              30%
Max peak (11026 samples): 0x000026bf #######              30%
```

```bash
root@AM62x:~# arecord -D plughw:0,0 -f S16_LE -c 2 -r 44100 -d 10 --period-size=4096 --buffer-size=16384 -t wav  -v -v -v record_test.wav
Recording WAVE 'record_test.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
Plug PCM: Hardware PCM card 0 'AM62x-SKEVM-SLAVE' device 0 subdevice 0
Its setup is:
  stream       : CAPTURE
  access       : RW_INTERLEAVED
  format       : S16_LE
  subformat    : STD
  channels     : 2
  rate         : 44100
  exact rate   : 44100 (44100/1)
  msbits       : 16
  buffer_size  : 16384
  period_size  : 4096
  period_time  : 92879
  tstamp_mode  : NONE
  tstamp_type  : MONOTONIC
  period_step  : 1
  avail_min    : 4096
  period_event : 0
  start_threshold  : 1
  stop_threshold   : 16384
  silence_threshold: 0
  silence_size : 0
  boundary     : 4611686018427387904
  appl_ptr     : 0
  hw_ptr       : 0
Max peak (8192 samples): 0x00000260 #                    1%
Max peak (8192 samples): 0x000006d8 ##                   5%
Max peak (8192 samples): 0x00000f44 ###                  11%
Max peak (8192 samples): 0x00000ffc ###                  12%
Max peak (8192 samples): 0x00001350 ####                 15%
Max peak (8192 samples): 0x00000da1 ###                  10%
Max peak (8192 samples): 0x00000fc7 ###                  12%
Max peak (8192 samples): 0x00000e1f ###                  11%
Max peak (8192 samples): 0x00000a15 ##                   7%
Max peak (8192 samples): 0x000008fa ##                   7%
Max peak (8192 samples): 0x00000a2f ##                   7%
Max peak (8192 samples): 0x000009a0 ##                   7%
Max peak (8192 samples): 0x000013be ####                 15%
Max peak (8192 samples): 0x00001314 ###                  14%
Max peak (8192 samples): 0x000010c0 ###                  13%
Max peak (8192 samples): 0x0000153c ####                 16%
Max peak (8192 samples): 0x000015ee ####                 17%
Max peak (8192 samples): 0x0000112d ###                  13%
Max peak (8192 samples): 0x00001065 ###                  12%
Max peak (8192 samples): 0x0000100e ###                  12%
Max peak (8192 samples): 0x0000095d ##                   7%
Max peak (8192 samples): 0x00000593 #                    4%
Max peak (8192 samples): 0x000002c4 #                    2%
Max peak (8192 samples): 0x00000212 #                    1%
Max peak (8192 samples): 0x000002c1 #                    2%
Max peak (8192 samples): 0x00000861 ##                   6%
Max peak (8192 samples): 0x000007f3 ##                   6%
Max peak (8192 samples): 0x00000318 #                    2%
Max peak (8192 samples): 0x00001676 ####                 17%
Max peak (8192 samples): 0x00000921 ##                   7%
Max peak (8192 samples): 0x00000667 ##                   5%
Max peak (8192 samples): 0x00000932 ##                   7%
Max peak (8192 samples): 0x00002fd6 ########             37%
Max peak (8192 samples): 0x0000475d ############         55%
Max peak (8192 samples): 0x000006b9 ##                   5%
Max peak (8192 samples): 0x00000a78 ##                   8%
Max peak (8192 samples): 0x0000088c ##                   6%
Max peak (8192 samples): 0x00001672 ####                 17%
Max peak (8192 samples): 0x00000680 ##                   5%
Max peak (8192 samples): 0x00000437 #                    3%
Max peak (8192 samples): 0x00000434 #                    3%
Max peak (8192 samples): 0x00000c38 ##                   9%
Max peak (8192 samples): 0x00000a6b ##                   8%
Max peak (8192 samples): 0x000005ad #                    4%
Max peak (8192 samples): 0x000007e9 ##                   6%
Max peak (8192 samples): 0x0000083e ##                   6%
Max peak (8192 samples): 0x0000075f ##                   5%
Max peak (8192 samples): 0x00003ac7 ##########           45%
Max peak (8192 samples): 0x00000d86 ###                  10%
Max peak (8192 samples): 0x00001063 ###                  12%
Max peak (8192 samples): 0x00000d36 ###                  10%
Max peak (8192 samples): 0x00000de5 ###                  10%
Max peak (8192 samples): 0x00000c1e ##                   9%
Max peak (8192 samples): 0x00000ac0 ##                   8%
Max peak (8192 samples): 0x00000cb2 ##                   9%
Max peak (8192 samples): 0x00000439 #                    3%
Max peak (8192 samples): 0x00000667 ##                   5%
Max peak (8192 samples): 0x00001305 ###                  14%
Max peak (8192 samples): 0x000010cd ###                  13%
Max peak (8192 samples): 0x000039ea ##########           45%
Max peak (8192 samples): 0x00000b47 ##                   8%
Max peak (8192 samples): 0x00000b62 ##                   8%
Max peak (8192 samples): 0x000005f5 #                    4%
Max peak (8192 samples): 0x00002e68 ########             36%
Max peak (8192 samples): 0x00000e1a ###                  11%
Max peak (8192 samples): 0x000010cf ###                  13%
Max peak (8192 samples): 0x00001556 ####                 16%
Max peak (8192 samples): 0x00000bea ##                   9%
Max peak (8192 samples): 0x0000090b ##                   7%
Max peak (8192 samples): 0x000005bd #                    4%
Max peak (8192 samples): 0x00001832 ####                 18%
Max peak (8192 samples): 0x0000104f ###                  12%
Max peak (8192 samples): 0x00000a28 ##                   7%
Max peak (8192 samples): 0x00002c75 #######              34%
Max peak (8192 samples): 0x0000095d ##                   7%
Max peak (8192 samples): 0x00001227 ###                  14%
Max peak (8192 samples): 0x00001040 ###                  12%
Max peak (8192 samples): 0x00001632 ####                 17%
Max peak (8192 samples): 0x00000e9c ###                  11%
Max peak (8192 samples): 0x000009b7 ##                   7%
Max peak (8192 samples): 0x00000e3b ###                  11%
Max peak (8192 samples): 0x000009a8 ##                   7%
Max peak (8192 samples): 0x00000d83 ###                  10%
Max peak (8192 samples): 0x00000913 ##                   7%
Max peak (8192 samples): 0x00000912 ##                   7%
Max peak (8192 samples): 0x00002a02 #######              32%
Max peak (8192 samples): 0x00000578 #                    4%
Max peak (8192 samples): 0x00000904 ##                   7%
Max peak (8192 samples): 0x00000825 ##                   6%
Max peak (8192 samples): 0x00000802 ##                   6%
Max peak (8192 samples): 0x00000cb6 ##                   9%
Max peak (8192 samples): 0x000002b6 #                    2%
Max peak (8192 samples): 0x000002c7 #                    2%
Max peak (8192 samples): 0x00000a6d ##                   8%
Max peak (8192 samples): 0x000009b5 ##                   7%
Max peak (8192 samples): 0x00000b77 ##                   8%
Max peak (8192 samples): 0x000006c8 ##                   5%
Max peak (8192 samples): 0x00000664 #                    4%
Max peak (8192 samples): 0x0000048a #                    3%
Max peak (8192 samples): 0x00002c26 #######              34%
Max peak (8192 samples): 0x0000079e ##                   5%
Max peak (8192 samples): 0x00000968 ##                   7%
Max peak (8192 samples): 0x00000b4a ##                   8%
Max peak (8192 samples): 0x00000995 ##                   7%
Max peak (8192 samples): 0x0000080c ##                   6%
Max peak (8192 samples): 0x00000564 #                    4%
Max peak (8192 samples): 0x00000e39 ###                  11%
Max peak (8192 samples): 0x000006b2 ##                   5%
```

```bash
root@AM62x:~# arecord -D plughw:0,0 -f S16_LE -c 2 -r 44100 -d 10 --period-size=8192 --buffer-size=32768 -t wav  -v -v -v record_test.wav
Recording WAVE 'record_test.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
Plug PCM: Hardware PCM card 0 'AM62x-SKEVM-SLAVE' device 0 subdevice 0
Its setup is:
  stream       : CAPTURE
  access       : RW_INTERLEAVED
  format       : S16_LE
  subformat    : STD
  channels     : 2
  rate         : 44100
  exact rate   : 44100 (44100/1)
  msbits       : 16
  buffer_size  : 32768
  period_size  : 8192
  period_time  : 185759
  tstamp_mode  : NONE
  tstamp_type  : MONOTONIC
  period_step  : 1
  avail_min    : 8192
  period_event : 0
  start_threshold  : 1
  stop_threshold   : 32768
  silence_threshold: 0
  silence_size : 0
  boundary     : 4611686018427387904
  appl_ptr     : 0
  hw_ptr       : 0
Max peak (16384 samples): 0x000005e7 #                    4%
Max peak (16384 samples): 0x00004206 ###########          51%
Max peak (16384 samples): 0x00000d3f ###                  10%
Max peak (16384 samples): 0x00002d99 ########             35%
Max peak (16384 samples): 0x00000dbc ###                  10%
Max peak (16384 samples): 0x0000143d ####                 15%
Max peak (16384 samples): 0x00001642 ####                 17%
Max peak (16384 samples): 0x00001884 ####                 19%
Max peak (16384 samples): 0x000036d0 #########            42%
Max peak (16384 samples): 0x0000122c ###                  14%
Max peak (16384 samples): 0x000011da ###                  13%
Max peak (16384 samples): 0x00000f7d ###                  12%
Max peak (16384 samples): 0x000012c2 ###                  14%
Max peak (16384 samples): 0x0000121d ###                  14%
Max peak (16384 samples): 0x000033a4 #########            40%
Max peak (16384 samples): 0x00000a24 ##                   7%
Max peak (16384 samples): 0x00000fcf ###                  12%
Max peak (16384 samples): 0x000005a6 #                    4%
Max peak (16384 samples): 0x00000e80 ###                  11%
Max peak (16384 samples): 0x00000af2 ##                   8%
Max peak (16384 samples): 0x0000084d ##                   6%
Max peak (16384 samples): 0x00003439 #########            40%
Max peak (16384 samples): 0x000009c9 ##                   7%
Max peak (16384 samples): 0x00000935 ##                   7%
Max peak (16384 samples): 0x00001f8d #####                24%
Max peak (16384 samples): 0x0000110e ###                  13%
Max peak (16384 samples): 0x00000d89 ###                  10%
Max peak (16384 samples): 0x0000327f ########             39%
Max peak (16384 samples): 0x00000f6f ###                  12%
Max peak (16384 samples): 0x000018ff ####                 19%
Max peak (16384 samples): 0x00000c4f ##                   9%
Max peak (16384 samples): 0x00000bd1 ##                   9%
Max peak (16384 samples): 0x0000127b ###                  14%
Max peak (16384 samples): 0x000034e0 #########            41%
Max peak (16384 samples): 0x00003943 #########            44%
Max peak (16384 samples): 0x0000211c ######               25%
Max peak (16384 samples): 0x00001def #####                23%
Max peak (16384 samples): 0x0000134c ####                 15%
Max peak (16384 samples): 0x0000102e ###                  12%
Max peak (16384 samples): 0x00000c46 ##                   9%
Max peak (16384 samples): 0x00003660 #########            42%
Max peak (16384 samples): 0x0000172d ####                 18%
Max peak (16384 samples): 0x00001bc1 #####                21%
Max peak (16384 samples): 0x00001128 ###                  13%
Max peak (16384 samples): 0x0000180e ####                 18%
Max peak (16384 samples): 0x00001d7b #####                23%
Max peak (16384 samples): 0x00000fb3 ###                  12%
Max peak (16384 samples): 0x000028e3 #######              31%
Max peak (16384 samples): 0x00001c29 #####                22%
Max peak (16384 samples): 0x0000127d ###                  14%
Max peak (16384 samples): 0x000013dc ####                 15%
Max peak (16384 samples): 0x00000e56 ###                  11%
Max peak (16384 samples): 0x00000fa7 ###                  12%
Max peak (16384 samples): 0x0000354b #########            41%
```

# 6.6
```bash
root@AM62x:~# arecord -D hw:0,0 --dump-hw-params
Warning: Some sources (like microphones) may produce inaudiable results
         with 8-bit sampling. Use '-f' argument to increase resolution
         e.g. '-f S16_LE'.
Recording WAVE 'stdin' : Unsigned 8 bit, Rate 8000 Hz, Mono
HW Params of device "hw:0,0":
--------------------
ACCESS:  MMAP_INTERLEAVED RW_INTERLEAVED
FORMAT:  S16_LE S24_LE S32_LE S24_3LE
SUBFORMAT:  STD
SAMPLE_BITS: [16 32]
FRAME_BITS: [32 64]
CHANNELS: 2
RATE: [8000 96000]
PERIOD_TIME: (333 2048000]
PERIOD_SIZE: [32 16384]
PERIOD_BYTES: [128 65536]
PERIODS: [2 4096]
BUFFER_TIME: (666 16384000]
BUFFER_SIZE: [64 131072]
BUFFER_BYTES: [256 524288]
TICK_TIME: ALL
--------------------
arecord: set_params:1352: Sample format non available
Available formats:
- S16_LE
- S24_LE
- S32_LE
- S24_3LE
```

```bash
root@AM62x:~# arecord -D hw:0,0 -f S16_LE -c 2 -r 22050 -d 10 -t wav -v -v -v SDK5.10_record_44k.wav
Recording WAVE 'SDK5.10_record_44k.wav' : Signed 16 bit Little Endian, Rate 22050 Hz, Stereo
Hardware PCM card 0 'AM62x-SKEVM-SLAVE' device 0 subdevice 0
Its setup is:
  stream       : CAPTURE
  access       : RW_INTERLEAVED
  format       : S16_LE
  subformat    : STD
  channels     : 2
  rate         : 22050
  exact rate   : 22050 (22050/1)
  msbits       : 16
  buffer_size  : 11024
  period_size  : 2756
  period_time  : 124988
  tstamp_mode  : NONE
  tstamp_type  : MONOTONIC
  period_step  : 1
  avail_min    : 2756
  period_event : 0
  start_threshold  : 1
  stop_threshold   : 11024
  silence_threshold: 0
  silence_size : 0
  boundary     : 6205960286516543488
  appl_ptr     : 0
  hw_ptr       : 0
Max peak (5512 samples): 0x0000148f ####                 16%
arecord: pcm_read:2221: read error: Input/output error
```

```bash
root@AM62x:~# arecord -D hw:0,0 -f S16_LE -c 2 -r 44100 -d 10 -t wav -v -v -v SDK5.10_record_44k.wav
Recording WAVE 'SDK5.10_record_44k.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
Hardware PCM card 0 'AM62x-SKEVM-SLAVE' device 0 subdevice 0
Its setup is:
  stream       : CAPTURE
  access       : RW_INTERLEAVED
  format       : S16_LE
  subformat    : STD
  channels     : 2
  rate         : 44100
  exact rate   : 44100 (44100/1)
  msbits       : 16
  buffer_size  : 22052
  period_size  : 5513
  period_time  : 125011
  tstamp_mode  : NONE
  tstamp_type  : MONOTONIC
  period_step  : 1
  avail_min    : 5513
  period_event : 0
  start_threshold  : 1
  stop_threshold   : 22052
  silence_threshold: 0
  silence_size : 0
  boundary     : 6207086186423386112
  appl_ptr     : 0
  hw_ptr       : 0
Max peak (11026 samples): 0x00000f89 ###                  12%
Max peak (11026 samples): 0x00000ea9 ###                  11%
Max peak (11026 samples): 0x00000f15 ###                  11%
Max peak (11026 samples): 0x00000ee0 ###                  11%
Max peak (11026 samples): 0x00001040 ###                  12%
Max peak (11026 samples): 0x00001017 ###                  12%
Max peak (11026 samples): 0x00000f5e ###                  12%
Max peak (11026 samples): 0x00000f30 ###                  11%
Max peak (11026 samples): 0x00001032 ###                  12%
Max peak (11026 samples): 0x00001067 ###                  12%
Max peak (11026 samples): 0x00000fe9 ###                  12%
Max peak (11026 samples): 0x00000f16 ###                  11%
Max peak (11026 samples): 0x000010c0 ###                  13%
Max peak (11026 samples): 0x000012ce ###                  14%
Max peak (11026 samples): 0x00000fee ###                  12%
Max peak (11026 samples): 0x00000f2b ###                  11%
Max peak (11026 samples): 0x00000f63 ###                  12%
Max peak (11026 samples): 0x000010c2 ###                  13%
Max peak (11026 samples): 0x00000fd2 ###                  12%
Max peak (11026 samples): 0x00000f94 ###                  12%
Max peak (11026 samples): 0x00000ee7 ###                  11%
Max peak (11026 samples): 0x00000f72 ###                  12%
Max peak (11026 samples): 0x0000100f ###                  12%
Max peak (11026 samples): 0x00000f33 ###                  11%
Max peak (11026 samples): 0x00000ffb ###                  12%
Max peak (11026 samples): 0x000010bf ###                  13%
Max peak (11026 samples): 0x00001052 ###                  12%
Max peak (11026 samples): 0x000010a2 ###                  12%
Max peak (11026 samples): 0x00000e8b ###                  11%
Max peak (11026 samples): 0x0000103b ###                  12%
Max peak (11026 samples): 0x00001063 ###                  12%
Max peak (11026 samples): 0x00000fd8 ###                  12%
Max peak (11026 samples): 0x0000105e ###                  12%
Max peak (11026 samples): 0x000010bf ###                  13%
Max peak (11026 samples): 0x00001006 ###                  12%
Max peak (11026 samples): 0x00001105 ###                  13%
Max peak (11026 samples): 0x00001048 ###                  12%
Max peak (11026 samples): 0x00001143 ###                  13%
Max peak (11026 samples): 0x00000e27 ###                  11%
Max peak (11026 samples): 0x0000107c ###                  12%
Max peak (11026 samples): 0x00000f4d ###                  11%
Max peak (11026 samples): 0x00001004 ###                  12%
Max peak (11026 samples): 0x00000f9d ###                  12%
Max peak (11026 samples): 0x0000109b ###                  12%
Max peak (11026 samples): 0x000011c0 ###                  13%
Max peak (11026 samples): 0x00000ec0 ###                  11%
Max peak (11026 samples): 0x00000e54 ###                  11%
Max peak (11026 samples): 0x00000f5a ###                  11%
Max peak (11026 samples): 0x0000113f ###                  13%
Max peak (11026 samples): 0x00000eed ###                  11%
Max peak (11026 samples): 0x00000ef5 ###                  11%
Max peak (11026 samples): 0x000010a1 ###                  12%
Max peak (11026 samples): 0x00000fb9 ###                  12%
Max peak (11026 samples): 0x0000110b ###                  13%
Max peak (11026 samples): 0x00000eff ###                  11%
Max peak (11026 samples): 0x00000f49 ###                  11%
Max peak (11026 samples): 0x00000eb9 ###                  11%
Max peak (11026 samples): 0x000012a6 ###                  14%
Max peak (11026 samples): 0x00000f7d ###                  12%
Max peak (11026 samples): 0x00001325 ###                  14%
Max peak (11026 samples): 0x00001023 ###                  12%
Max peak (11026 samples): 0x00000f60 ###                  12%
Max peak (11026 samples): 0x000012b4 ###                  14%
Max peak (11026 samples): 0x0000116f ###                  13%
Max peak (11026 samples): 0x00000ed4 ###                  11%
Max peak (11026 samples): 0x00000fe8 ###                  12%
Max peak (11026 samples): 0x00001042 ###                  12%
Max peak (11026 samples): 0x00001046 ###                  12%
Max peak (11026 samples): 0x000010eb ###                  13%
Max peak (11026 samples): 0x0000100d ###                  12%
Max peak (11026 samples): 0x00000f46 ###                  11%
Max peak (11026 samples): 0x00000fd0 ###                  12%
Max peak (11026 samples): 0x000010d5 ###                  13%
Max peak (11026 samples): 0x00001203 ###                  14%
Max peak (11026 samples): 0x00000fdc ###                  12%
Max peak (11026 samples): 0x00000f8b ###                  12%
Max peak (11026 samples): 0x00001067 ###                  12%
Max peak (11026 samples): 0x00001009 ###                  12%
Max peak (11026 samples): 0x00001125 ###                  13%
Max peak (11026 samples): 0x00000ee7 ###                  11%
```