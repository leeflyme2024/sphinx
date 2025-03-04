# 测试命令
```bash
dmesg | grep rproc
dmesg | grep remoteproc
dmesg | grep rpmsg
dmesg | grep reserved
dmesg | grep mailbox
dmesg | grep virtio

lsmod | grep rpmsg
lsmod | grep remoteproc
lsmod | grep rproc

modprobe rpmsg_client_sample count=10

ll /sys/module/*_remoteproc
ll /sys/bus/platform/drivers/*-rproc

ll /lib/firmware
ll /lib/firmware/pdk-ipc/

ll /dev/rpmsg*
ll /sys/class/rpmsg
ll /sys/bus/rpmsg/devices

ll /sys/bus/virtio/devices

ll /sys/class/remoteproc
head /sys/class/remoteproc/remoteproc*/name
head /sys/class/remoteproc/remoteproc*/state
head /sys/class/remoteproc/remoteproc*/firmware
head /sys/class/remoteproc/remoteproc*/coredump
head /sys/class/remoteproc/remoteproc*/recovery
ll /sys/class/remoteproc/remoteproc*/remoteproc*#vdev0buffer/virtio*/
ll /sys/class/remoteproc/remoteproc*/rproc-virtio.*.auto/virtio*/

echo stop > /sys/class/remoteproc/remoteproc0/state
echo detach > /sys/class/remoteproc/remoteproc1/state
echo start > /sys/class/remoteproc/remoteproc0/state
echo start > /sys/class/remoteproc/remoteproc1/state

ll /sys/kernel/debug/remoteproc/*
head /sys/kernel/debug/remoteproc/remoteproc*/name
head /sys/kernel/debug/remoteproc/remoteproc*/trace0
head /sys/kernel/debug/remoteproc/remoteproc*/carveout_memories -n 100
head /sys/kernel/debug/remoteproc/remoteproc*/coredump
head /sys/kernel/debug/remoteproc/remoteproc*/recovery
head /sys/kernel/debug/remoteproc/remoteproc*/resource_table -n 500

rpmsg_char_simple -r 9 -n 10
rpmsg_char_simple -r 15 -n 10
```

# 测试日志
```bash


root@AM62x:~# dmesg | grep rproc
[    8.405353] k3-m4-rproc 5000000.m4fss: assigned reserved memory node m4f-dma-memory@9cb00000
[    8.430081] k3-m4-rproc 5000000.m4fss: configured M4 for remoteproc mode
[    8.430182] k3-m4-rproc 5000000.m4fss: local reset is deasserted for device
[    8.453577] rproc-virtio rproc-virtio.2.auto: assigned reserved memory node m4f-dma-memory@9cb00000
[    8.457472] rproc-virtio rproc-virtio.2.auto: registered virtio0 (type 7)
[    8.573813] rproc-virtio rproc-virtio.3.auto: assigned reserved memory node r5f-dma-memory@9da00000
[    8.588202] rproc-virtio rproc-virtio.3.auto: registered virtio1 (type 7)




root@AM62x:~# dmesg | grep remoteproc
[    8.430081] k3-m4-rproc 5000000.m4fss: configured M4 for remoteproc mode
[    8.431917] remoteproc remoteproc0: 5000000.m4fss is available
[    8.441031] remoteproc remoteproc0: powering up 5000000.m4fss
[    8.441178] remoteproc remoteproc0: Booting fw image am62-mcu-m4f0_0-fw, size 55100
[    8.457481] remoteproc remoteproc0: remote processor 5000000.m4fss is now up
[    8.573179] remoteproc remoteproc1: 78000000.r5f is available
[    8.573273] remoteproc remoteproc1: attaching to 78000000.r5f
[    8.588211] remoteproc remoteproc1: remote processor 78000000.r5f is now attached
[   11.560232] remoteproc remoteproc2: 30074000.pru is available
[   11.569405] remoteproc remoteproc3: 30078000.pru is available


root@AM62x:~# dmesg | grep rpmsg
[    8.457392] virtio_rpmsg_bus virtio0: rpmsg host is online
[    8.459461] virtio_rpmsg_bus virtio0: creating channel ti.ipc4.ping-pong addr 0xd
[    8.459660] virtio_rpmsg_bus virtio0: creating channel rpmsg_chrdev addr 0xe
[    8.577099] virtio_rpmsg_bus virtio1: creating channel ti.ipc4.ping-pong addr 0xd
[    8.577223] virtio_rpmsg_bus virtio1: creating channel rpmsg_chrdev addr 0xe
[    8.588078] virtio_rpmsg_bus virtio1: rpmsg host is online
[   11.587630] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: new channel: 0x401 -> 0xd!
[   11.587827] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 1 (src: 0xd)
[   11.587970] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 2 (src: 0xd)
[   11.588086] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 3 (src: 0xd)
[   11.588155] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 4 (src: 0xd)
[   11.588251] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 5 (src: 0xd)
[   11.588342] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 6 (src: 0xd)
[   11.588436] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 7 (src: 0xd)
[   11.588527] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 8 (src: 0xd)
[   11.588628] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 9 (src: 0xd)
[   11.588724] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 10 (src: 0xd)
[   11.588797] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 11 (src: 0xd)
[   11.588900] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 12 (src: 0xd)
[   11.588991] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 13 (src: 0xd)
[   11.589081] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 14 (src: 0xd)
[   11.589172] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 15 (src: 0xd)
[   11.589266] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 16 (src: 0xd)
[   11.589396] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 17 (src: 0xd)
[   11.589490] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 18 (src: 0xd)
[   11.589581] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 19 (src: 0xd)
[   11.589673] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 20 (src: 0xd)
[   11.589755] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 21 (src: 0xd)
[   11.589843] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 22 (src: 0xd)
[   11.589943] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 23 (src: 0xd)
[   11.590033] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 24 (src: 0xd)
[   11.590121] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 25 (src: 0xd)
[   11.590214] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 26 (src: 0xd)
[   11.590306] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 27 (src: 0xd)
[   11.590395] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 28 (src: 0xd)
[   11.590489] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 29 (src: 0xd)
[   11.590602] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 30 (src: 0xd)
[   11.590720] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 31 (src: 0xd)
[   11.590789] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 32 (src: 0xd)
[   11.590874] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 33 (src: 0xd)
[   11.590981] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 34 (src: 0xd)
[   11.591075] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 35 (src: 0xd)
[   11.591162] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 36 (src: 0xd)
[   11.591253] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 37 (src: 0xd)
[   11.591344] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 38 (src: 0xd)
[   11.591434] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 39 (src: 0xd)
[   11.591524] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 40 (src: 0xd)
[   11.591597] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 41 (src: 0xd)
[   11.591713] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 42 (src: 0xd)
[   11.591778] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 43 (src: 0xd)
[   11.591881] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 44 (src: 0xd)
[   11.591939] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 45 (src: 0xd)
[   11.592045] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 46 (src: 0xd)
[   11.592125] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 47 (src: 0xd)
[   11.592224] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 48 (src: 0xd)
[   11.592319] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 49 (src: 0xd)
[   11.592409] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 50 (src: 0xd)
[   11.592503] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 51 (src: 0xd)
[   11.592599] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 52 (src: 0xd)
[   11.592690] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 53 (src: 0xd)
[   11.592789] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 54 (src: 0xd)
[   11.592882] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 55 (src: 0xd)
[   11.592972] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 56 (src: 0xd)
[   11.593075] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 57 (src: 0xd)
[   11.593152] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 58 (src: 0xd)
[   11.593245] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 59 (src: 0xd)
[   11.593334] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 60 (src: 0xd)
[   11.593425] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 61 (src: 0xd)
[   11.593515] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 62 (src: 0xd)
[   11.593607] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 63 (src: 0xd)
[   11.593697] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 64 (src: 0xd)
[   11.593786] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 65 (src: 0xd)
[   11.593878] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 66 (src: 0xd)
[   11.593968] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 67 (src: 0xd)
[   11.594069] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 68 (src: 0xd)
[   11.594149] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 69 (src: 0xd)
[   11.594248] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 70 (src: 0xd)
[   11.594342] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 71 (src: 0xd)
[   11.594479] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 72 (src: 0xd)
[   11.594558] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 73 (src: 0xd)
[   11.594656] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 74 (src: 0xd)
[   11.594747] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 75 (src: 0xd)
[   11.594838] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 76 (src: 0xd)
[   11.594929] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 77 (src: 0xd)
[   11.595020] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 78 (src: 0xd)
[   11.595113] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 79 (src: 0xd)
[   11.595253] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 80 (src: 0xd)
[   11.595381] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 81 (src: 0xd)
[   11.595522] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 82 (src: 0xd)
[   11.595659] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 83 (src: 0xd)
[   11.595736] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 84 (src: 0xd)
[   11.595834] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 85 (src: 0xd)
[   11.595949] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 86 (src: 0xd)
[   11.596019] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 87 (src: 0xd)
[   11.596124] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 88 (src: 0xd)
[   11.596220] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 89 (src: 0xd)
[   11.596311] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 90 (src: 0xd)
[   11.596401] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 91 (src: 0xd)
[   11.596492] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 92 (src: 0xd)
[   11.596593] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 93 (src: 0xd)
[   11.596686] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 94 (src: 0xd)
[   11.596777] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 95 (src: 0xd)
[   11.596869] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 96 (src: 0xd)
[   11.596961] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 97 (src: 0xd)
[   11.597063] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 98 (src: 0xd)
[   11.597143] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 99 (src: 0xd)
[   11.597245] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 100 (src: 0xd)
[   11.597252] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: goodbye!
[   11.598282] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: new channel: 0x401 -> 0xd!
[   11.598421] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 1 (src: 0xd)
[   11.598472] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 2 (src: 0xd)
[   11.598522] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 3 (src: 0xd)
[   11.598590] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 4 (src: 0xd)
[   11.598663] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 5 (src: 0xd)
[   11.598740] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 6 (src: 0xd)
[   11.598806] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 7 (src: 0xd)
[   11.598872] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 8 (src: 0xd)
[   11.598941] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 9 (src: 0xd)
[   11.599010] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 10 (src: 0xd)
[   11.599088] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 11 (src: 0xd)
[   11.599142] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 12 (src: 0xd)
[   11.599217] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 13 (src: 0xd)
[   11.599297] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 14 (src: 0xd)
[   11.599360] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 15 (src: 0xd)
[   11.599433] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 16 (src: 0xd)
[   11.599506] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 17 (src: 0xd)
[   11.599576] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 18 (src: 0xd)
[   11.599641] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 19 (src: 0xd)
[   11.599711] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 20 (src: 0xd)
[   11.599784] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 21 (src: 0xd)
[   11.599851] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 22 (src: 0xd)
[   11.599926] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 23 (src: 0xd)
[   11.599989] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 24 (src: 0xd)
[   11.600066] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 25 (src: 0xd)
[   11.600123] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 26 (src: 0xd)
[   11.600197] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 27 (src: 0xd)
[   11.600263] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 28 (src: 0xd)
[   11.600331] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 29 (src: 0xd)
[   11.600396] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 30 (src: 0xd)
[   11.600468] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 31 (src: 0xd)
[   11.600533] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 32 (src: 0xd)
[   11.600598] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 33 (src: 0xd)
[   11.600676] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 34 (src: 0xd)
[   11.600737] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 35 (src: 0xd)
[   11.600799] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 36 (src: 0xd)
[   11.600863] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 37 (src: 0xd)
[   11.600939] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 38 (src: 0xd)
[   11.600994] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 39 (src: 0xd)
[   11.601067] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 40 (src: 0xd)
[   11.601124] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 41 (src: 0xd)
[   11.601191] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 42 (src: 0xd)
[   11.601262] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 43 (src: 0xd)
[   11.601329] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 44 (src: 0xd)
[   11.601395] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 45 (src: 0xd)
[   11.601462] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 46 (src: 0xd)
[   11.601533] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 47 (src: 0xd)
[   11.601601] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 48 (src: 0xd)
[   11.601668] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 49 (src: 0xd)
[   11.601733] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 50 (src: 0xd)
[   11.601802] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 51 (src: 0xd)
[   11.601872] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 52 (src: 0xd)
[   11.601940] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 53 (src: 0xd)
[   11.602004] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 54 (src: 0xd)
[   11.602072] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 55 (src: 0xd)
[   11.602139] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 56 (src: 0xd)
[   11.602211] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 57 (src: 0xd)
[   11.602284] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 58 (src: 0xd)
[   11.602348] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 59 (src: 0xd)
[   11.602418] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 60 (src: 0xd)
[   11.602483] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 61 (src: 0xd)
[   11.602553] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 62 (src: 0xd)
[   11.602619] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 63 (src: 0xd)
[   11.602688] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 64 (src: 0xd)
[   11.602754] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 65 (src: 0xd)
[   11.602820] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 66 (src: 0xd)
[   11.602889] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 67 (src: 0xd)
[   11.602958] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 68 (src: 0xd)
[   11.603026] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 69 (src: 0xd)
[   11.603098] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 70 (src: 0xd)
[   11.603168] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 71 (src: 0xd)
[   11.603230] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 72 (src: 0xd)
[   11.603298] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 73 (src: 0xd)
[   11.603363] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 74 (src: 0xd)
[   11.603516] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 75 (src: 0xd)
[   11.603562] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 76 (src: 0xd)
[   11.603633] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 77 (src: 0xd)
[   11.603699] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 78 (src: 0xd)
[   11.603768] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 79 (src: 0xd)
[   11.603847] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 80 (src: 0xd)
[   11.603920] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 81 (src: 0xd)
[   11.603994] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 82 (src: 0xd)
[   11.604068] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 83 (src: 0xd)
[   11.604128] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 84 (src: 0xd)
[   11.604213] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 85 (src: 0xd)
[   11.604268] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 86 (src: 0xd)
[   11.604338] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 87 (src: 0xd)
[   11.604411] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 88 (src: 0xd)
[   11.604473] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 89 (src: 0xd)
[   11.604548] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 90 (src: 0xd)
[   11.604613] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 91 (src: 0xd)
[   11.604682] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 92 (src: 0xd)
[   11.604754] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 93 (src: 0xd)
[   11.604820] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 94 (src: 0xd)
[   11.604928] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 95 (src: 0xd)
[   11.605004] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 96 (src: 0xd)
[   11.605073] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 97 (src: 0xd)
[   11.605140] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 98 (src: 0xd)
[   11.605209] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 99 (src: 0xd)
[   11.605279] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 100 (src: 0xd)
[   11.605286] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: goodbye!


root@AM62x:~# dmesg | grep reserved
[    0.000000] OF: reserved mem: initialized node linux,cma, compatible id shared-dma-pool
[    0.000000] OF: reserved mem: 0x00000000b8000000..0x00000000bfffffff (131072 KiB) map reusable linux,cma
[    0.000000] OF: reserved mem: 0x000000009c700000..0x000000009c7fffff (1024 KiB) map non-reusable ramoops@9c700000
[    0.000000] OF: reserved mem: initialized node ipc-memories@9c800000, compatible id shared-dma-pool
[    0.000000] OF: reserved mem: 0x000000009c800000..0x000000009cafffff (3072 KiB) nomap non-reusable ipc-memories@9c800000
[    0.000000] OF: reserved mem: initialized node m4f-dma-memory@9cb00000, compatible id shared-dma-pool
[    0.000000] OF: reserved mem: 0x000000009cb00000..0x000000009cbfffff (1024 KiB) nomap non-reusable m4f-dma-memory@9cb00000
[    0.000000] OF: reserved mem: initialized node m4f-memory@9cc00000, compatible id shared-dma-pool
[    0.000000] OF: reserved mem: 0x000000009cc00000..0x000000009d9fffff (14336 KiB) nomap non-reusable m4f-memory@9cc00000
[    0.000000] OF: reserved mem: initialized node r5f-dma-memory@9da00000, compatible id shared-dma-pool
[    0.000000] OF: reserved mem: 0x000000009da00000..0x000000009dafffff (1024 KiB) nomap non-reusable r5f-dma-memory@9da00000
[    0.000000] OF: reserved mem: initialized node r5f-memory@9db00000, compatible id shared-dma-pool
[    0.000000] OF: reserved mem: 0x000000009db00000..0x000000009e6fffff (12288 KiB) nomap non-reusable r5f-memory@9db00000
[    0.000000] OF: reserved mem: 0x000000009e780000..0x000000009e7fffff (512 KiB) nomap non-reusable tfa@9e780000
[    0.000000] OF: reserved mem: 0x000000009e800000..0x000000009fffffff (24576 KiB) nomap non-reusable optee@9e800000
[    0.000000] Memory: 749120K/1048576K available (10880K kernel code, 1220K rwdata, 2616K rodata, 2176K init, 427K bss, 168384K reserved, 131072K cma-reserved)
[    8.405353] k3-m4-rproc 5000000.m4fss: assigned reserved memory node m4f-dma-memory@9cb00000
[    8.453577] rproc-virtio rproc-virtio.2.auto: assigned reserved memory node m4f-dma-memory@9cb00000
[    8.567540] platform 78000000.r5f: assigned reserved memory node r5f-dma-memory@9da00000
[    8.573813] rproc-virtio rproc-virtio.3.auto: assigned reserved memory node r5f-dma-memory@9da00000




root@AM62x:~# dmesg | grep mailbox
[    0.165397] omap-mailbox 29000000.mailbox: omap mailbox rev 0x66fc9100



root@AM62x:~# dmesg | grep virtio
[    8.453577] rproc-virtio rproc-virtio.2.auto: assigned reserved memory node m4f-dma-memory@9cb00000
[    8.457392] virtio_rpmsg_bus virtio0: rpmsg host is online
[    8.457472] rproc-virtio rproc-virtio.2.auto: registered virtio0 (type 7)
[    8.459461] virtio_rpmsg_bus virtio0: creating channel ti.ipc4.ping-pong addr 0xd
[    8.459660] virtio_rpmsg_bus virtio0: creating channel rpmsg_chrdev addr 0xe
[    8.573813] rproc-virtio rproc-virtio.3.auto: assigned reserved memory node r5f-dma-memory@9da00000
[    8.577099] virtio_rpmsg_bus virtio1: creating channel ti.ipc4.ping-pong addr 0xd
[    8.577223] virtio_rpmsg_bus virtio1: creating channel rpmsg_chrdev addr 0xe
[    8.588078] virtio_rpmsg_bus virtio1: rpmsg host is online
[    8.588202] rproc-virtio rproc-virtio.3.auto: registered virtio1 (type 7)
[   11.587630] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: new channel: 0x401 -> 0xd!
[   11.587827] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 1 (src: 0xd)
[   11.587970] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 2 (src: 0xd)
[   11.588086] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 3 (src: 0xd)
[   11.588155] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 4 (src: 0xd)
[   11.588251] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 5 (src: 0xd)
[   11.588342] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 6 (src: 0xd)
[   11.588436] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 7 (src: 0xd)
[   11.588527] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 8 (src: 0xd)
[   11.588628] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 9 (src: 0xd)
[   11.588724] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 10 (src: 0xd)
[   11.588797] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 11 (src: 0xd)
[   11.588900] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 12 (src: 0xd)
[   11.588991] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 13 (src: 0xd)
[   11.589081] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 14 (src: 0xd)
[   11.589172] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 15 (src: 0xd)
[   11.589266] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 16 (src: 0xd)
[   11.589396] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 17 (src: 0xd)
[   11.589490] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 18 (src: 0xd)
[   11.589581] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 19 (src: 0xd)
[   11.589673] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 20 (src: 0xd)
[   11.589755] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 21 (src: 0xd)
[   11.589843] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 22 (src: 0xd)
[   11.589943] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 23 (src: 0xd)
[   11.590033] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 24 (src: 0xd)
[   11.590121] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 25 (src: 0xd)
[   11.590214] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 26 (src: 0xd)
[   11.590306] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 27 (src: 0xd)
[   11.590395] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 28 (src: 0xd)
[   11.590489] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 29 (src: 0xd)
[   11.590602] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 30 (src: 0xd)
[   11.590720] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 31 (src: 0xd)
[   11.590789] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 32 (src: 0xd)
[   11.590874] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 33 (src: 0xd)
[   11.590981] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 34 (src: 0xd)
[   11.591075] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 35 (src: 0xd)
[   11.591162] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 36 (src: 0xd)
[   11.591253] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 37 (src: 0xd)
[   11.591344] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 38 (src: 0xd)
[   11.591434] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 39 (src: 0xd)
[   11.591524] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 40 (src: 0xd)
[   11.591597] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 41 (src: 0xd)
[   11.591713] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 42 (src: 0xd)
[   11.591778] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 43 (src: 0xd)
[   11.591881] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 44 (src: 0xd)
[   11.591939] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 45 (src: 0xd)
[   11.592045] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 46 (src: 0xd)
[   11.592125] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 47 (src: 0xd)
[   11.592224] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 48 (src: 0xd)
[   11.592319] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 49 (src: 0xd)
[   11.592409] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 50 (src: 0xd)
[   11.592503] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 51 (src: 0xd)
[   11.592599] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 52 (src: 0xd)
[   11.592690] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 53 (src: 0xd)
[   11.592789] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 54 (src: 0xd)
[   11.592882] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 55 (src: 0xd)
[   11.592972] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 56 (src: 0xd)
[   11.593075] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 57 (src: 0xd)
[   11.593152] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 58 (src: 0xd)
[   11.593245] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 59 (src: 0xd)
[   11.593334] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 60 (src: 0xd)
[   11.593425] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 61 (src: 0xd)
[   11.593515] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 62 (src: 0xd)
[   11.593607] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 63 (src: 0xd)
[   11.593697] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 64 (src: 0xd)
[   11.593786] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 65 (src: 0xd)
[   11.593878] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 66 (src: 0xd)
[   11.593968] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 67 (src: 0xd)
[   11.594069] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 68 (src: 0xd)
[   11.594149] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 69 (src: 0xd)
[   11.594248] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 70 (src: 0xd)
[   11.594342] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 71 (src: 0xd)
[   11.594479] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 72 (src: 0xd)
[   11.594558] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 73 (src: 0xd)
[   11.594656] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 74 (src: 0xd)
[   11.594747] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 75 (src: 0xd)
[   11.594838] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 76 (src: 0xd)
[   11.594929] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 77 (src: 0xd)
[   11.595020] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 78 (src: 0xd)
[   11.595113] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 79 (src: 0xd)
[   11.595253] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 80 (src: 0xd)
[   11.595381] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 81 (src: 0xd)
[   11.595522] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 82 (src: 0xd)
[   11.595659] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 83 (src: 0xd)
[   11.595736] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 84 (src: 0xd)
[   11.595834] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 85 (src: 0xd)
[   11.595949] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 86 (src: 0xd)
[   11.596019] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 87 (src: 0xd)
[   11.596124] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 88 (src: 0xd)
[   11.596220] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 89 (src: 0xd)
[   11.596311] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 90 (src: 0xd)
[   11.596401] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 91 (src: 0xd)
[   11.596492] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 92 (src: 0xd)
[   11.596593] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 93 (src: 0xd)
[   11.596686] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 94 (src: 0xd)
[   11.596777] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 95 (src: 0xd)
[   11.596869] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 96 (src: 0xd)
[   11.596961] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 97 (src: 0xd)
[   11.597063] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 98 (src: 0xd)
[   11.597143] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 99 (src: 0xd)
[   11.597245] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 100 (src: 0xd)
[   11.597252] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: goodbye!
[   11.598282] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: new channel: 0x401 -> 0xd!
[   11.598421] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 1 (src: 0xd)
[   11.598472] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 2 (src: 0xd)
[   11.598522] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 3 (src: 0xd)
[   11.598590] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 4 (src: 0xd)
[   11.598663] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 5 (src: 0xd)
[   11.598740] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 6 (src: 0xd)
[   11.598806] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 7 (src: 0xd)
[   11.598872] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 8 (src: 0xd)
[   11.598941] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 9 (src: 0xd)
[   11.599010] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 10 (src: 0xd)
[   11.599088] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 11 (src: 0xd)
[   11.599142] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 12 (src: 0xd)
[   11.599217] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 13 (src: 0xd)
[   11.599297] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 14 (src: 0xd)
[   11.599360] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 15 (src: 0xd)
[   11.599433] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 16 (src: 0xd)
[   11.599506] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 17 (src: 0xd)
[   11.599576] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 18 (src: 0xd)
[   11.599641] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 19 (src: 0xd)
[   11.599711] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 20 (src: 0xd)
[   11.599784] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 21 (src: 0xd)
[   11.599851] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 22 (src: 0xd)
[   11.599926] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 23 (src: 0xd)
[   11.599989] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 24 (src: 0xd)
[   11.600066] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 25 (src: 0xd)
[   11.600123] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 26 (src: 0xd)
[   11.600197] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 27 (src: 0xd)
[   11.600263] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 28 (src: 0xd)
[   11.600331] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 29 (src: 0xd)
[   11.600396] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 30 (src: 0xd)
[   11.600468] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 31 (src: 0xd)
[   11.600533] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 32 (src: 0xd)
[   11.600598] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 33 (src: 0xd)
[   11.600676] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 34 (src: 0xd)
[   11.600737] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 35 (src: 0xd)
[   11.600799] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 36 (src: 0xd)
[   11.600863] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 37 (src: 0xd)
[   11.600939] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 38 (src: 0xd)
[   11.600994] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 39 (src: 0xd)
[   11.601067] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 40 (src: 0xd)
[   11.601124] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 41 (src: 0xd)
[   11.601191] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 42 (src: 0xd)
[   11.601262] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 43 (src: 0xd)
[   11.601329] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 44 (src: 0xd)
[   11.601395] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 45 (src: 0xd)
[   11.601462] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 46 (src: 0xd)
[   11.601533] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 47 (src: 0xd)
[   11.601601] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 48 (src: 0xd)
[   11.601668] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 49 (src: 0xd)
[   11.601733] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 50 (src: 0xd)
[   11.601802] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 51 (src: 0xd)
[   11.601872] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 52 (src: 0xd)
[   11.601940] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 53 (src: 0xd)
[   11.602004] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 54 (src: 0xd)
[   11.602072] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 55 (src: 0xd)
[   11.602139] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 56 (src: 0xd)
[   11.602211] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 57 (src: 0xd)
[   11.602284] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 58 (src: 0xd)
[   11.602348] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 59 (src: 0xd)
[   11.602418] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 60 (src: 0xd)
[   11.602483] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 61 (src: 0xd)
[   11.602553] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 62 (src: 0xd)
[   11.602619] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 63 (src: 0xd)
[   11.602688] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 64 (src: 0xd)
[   11.602754] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 65 (src: 0xd)
[   11.602820] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 66 (src: 0xd)
[   11.602889] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 67 (src: 0xd)
[   11.602958] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 68 (src: 0xd)
[   11.603026] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 69 (src: 0xd)
[   11.603098] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 70 (src: 0xd)
[   11.603168] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 71 (src: 0xd)
[   11.603230] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 72 (src: 0xd)
[   11.603298] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 73 (src: 0xd)
[   11.603363] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 74 (src: 0xd)
[   11.603516] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 75 (src: 0xd)
[   11.603562] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 76 (src: 0xd)
[   11.603633] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 77 (src: 0xd)
[   11.603699] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 78 (src: 0xd)
[   11.603768] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 79 (src: 0xd)
[   11.603847] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 80 (src: 0xd)
[   11.603920] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 81 (src: 0xd)
[   11.603994] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 82 (src: 0xd)
[   11.604068] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 83 (src: 0xd)
[   11.604128] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 84 (src: 0xd)
[   11.604213] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 85 (src: 0xd)
[   11.604268] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 86 (src: 0xd)
[   11.604338] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 87 (src: 0xd)
[   11.604411] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 88 (src: 0xd)
[   11.604473] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 89 (src: 0xd)
[   11.604548] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 90 (src: 0xd)
[   11.604613] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 91 (src: 0xd)
[   11.604682] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 92 (src: 0xd)
[   11.604754] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 93 (src: 0xd)
[   11.604820] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 94 (src: 0xd)
[   11.604928] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 95 (src: 0xd)
[   11.605004] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 96 (src: 0xd)
[   11.605073] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 97 (src: 0xd)
[   11.605140] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 98 (src: 0xd)
[   11.605209] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 99 (src: 0xd)
[   11.605279] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: incoming msg 100 (src: 0xd)
[   11.605286] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: goodbye!



root@AM62x:~# lsmod | grep rpmsg
rpmsg_client_sample    12288  0
rpmsg_ctrl             12288  0
rpmsg_char             12288  1 rpmsg_ctrl
root@AM62x:~#
root@AM62x:~# lsmod | grep remoteproc
ti_k3_r5_remoteproc    24576  0
ti_k3_m4_remoteproc    16384  0
root@AM62x:~#
root@AM62x:~# lsmod | grep rproc
pru_rproc              20480  0
pruss                  12288  1 pru_rproc




root@AM62x:~# ll /sys/module/*_remoteproc
/sys/module/ti_k3_m4_remoteproc:
total 0
-r--r--r-- 1 root root 4096 10月 24 01:02 coresize
drwxr-xr-x 2 root root    0 10月 24 01:03 drivers
drwxr-xr-x 2 root root    0 10月 24 01:02 holders
-r--r--r-- 1 root root 4096 10月 24 01:03 initsize
-r--r--r-- 1 root root 4096 10月 24 01:03 initstate
-r--r--r-- 1 root root 4096 10月 24 01:02 refcnt
-r--r--r-- 1 root root 4096 10月 24 01:03 taint
--w------- 1 root root 4096 10月 24 01:03 uevent

/sys/module/ti_k3_r5_remoteproc:
total 0
-r--r--r-- 1 root root 4096 10月 24 01:02 coresize
drwxr-xr-x 2 root root    0 10月 24 01:03 drivers
drwxr-xr-x 2 root root    0 10月 24 01:02 holders
-r--r--r-- 1 root root 4096 10月 24 01:03 initsize
-r--r--r-- 1 root root 4096 10月 24 01:03 initstate
-r--r--r-- 1 root root 4096 10月 24 01:02 refcnt
-r--r--r-- 1 root root 4096 10月 24 01:03 taint
--w------- 1 root root 4096 10月 24 01:03 uevent

root@AM62x:~# ll /sys/bus/platform/drivers/*-rproc
/sys/bus/platform/drivers/k3-m4-rproc:
total 0
lrwxrwxrwx 1 root root    0 10月 24 01:03 5000000.m4fss -> ../../../../devices/platform/bus@f0000/bus@f0000:bus@4000000/5000000.m4fss
--w------- 1 root root 4096 10月 24 01:03 bind
lrwxrwxrwx 1 root root    0 10月 24 01:03 module -> ../../../../module/ti_k3_m4_remoteproc
--w------- 1 root root 4096 10月 24 01:03 uevent
--w------- 1 root root 4096 10月 24 01:03 unbind

/sys/bus/platform/drivers/pru-rproc:
total 0
lrwxrwxrwx 1 root root    0 10月 24 01:03 30074000.pru -> ../../../../devices/platform/bus@f0000/30040000.pruss/30074000.pru
lrwxrwxrwx 1 root root    0 10月 24 01:03 30078000.pru -> ../../../../devices/platform/bus@f0000/30040000.pruss/30078000.pru
lrwxrwxrwx 1 root root    0 10月 24 01:03 module -> ../../../../module/pru_rproc
--w------- 1 root root 4096 10月 24 01:03 uevent




root@AM62x:~# ll /lib/firmware
total 8
lrwxrwxrwx 1 root root   63  2月 28  2025 am62-main-r5f0_0-fw -> /lib/firmware/pdk-ipc/ipc_echo_testb_mcu1_0_release_strip.xer5f
lrwxrwxrwx 1 root root   72  2月 28  2025 am62-mcu-m4f0_0-fw -> /lib/firmware/pdk-ipc/ipc_echo_baremetal_test_mcu2_0_release_strip.xer5f
drwxr-xr-x 1 root root 4096 10月 24 00:57 cypress
drwxr-xr-x 2 root root  122  2月 28  2025 pdk-ipc

root@AM62x:~# ll /lib/firmware/pdk-ipc/
total 243
-rwxr-xr-x 1 root root  55100  2月 28  2025 ipc_echo_baremetal_test_mcu2_0_release_strip.xer5f
-rwxr-xr-x 1 root root 193296  2月 28  2025 ipc_echo_testb_mcu1_0_release_strip.xer5f





root@AM62x:~# ll /dev/rpmsg*
crw------- 1 root root 234, 0 10月 24 00:59 /dev/rpmsg0
crw------- 1 root root 234, 1 10月 24 00:59 /dev/rpmsg1
crw------- 1 root root 511, 0 10月 24 00:59 /dev/rpmsg_ctrl0
crw------- 1 root root 511, 1 10月 24 00:59 /dev/rpmsg_ctrl1
root@AM62x:~#
root@AM62x:~# ll /sys/class/rpmsg
total 0
lrwxrwxrwx 1 root root 0 10月 24 01:04 rpmsg0 -> ../../devices/platform/bus@f0000/bus@f0000:bus@4000000/5000000.m4fss/remoteproc/remoteproc0/rproc-virtio.2.auto/virtio0/virtio0.rpmsg_chrdev.-1.14/rpmsg/rpmsg0
lrwxrwxrwx 1 root root 0 10月 24 01:04 rpmsg1 -> ../../devices/platform/bus@f0000/bus@f0000:bus@b00000/bus@f0000:bus@b00000:r5fss@78000000/78000000.r5f/remoteproc/remoteproc1/rproc-virtio.3.auto/virtio1/virtio1.rpmsg_chrdev.-1.14/rpmsg/rpmsg1
lrwxrwxrwx 1 root root 0 10月 24 01:04 rpmsg_ctrl0 -> ../../devices/platform/bus@f0000/bus@f0000:bus@4000000/5000000.m4fss/remoteproc/remoteproc0/rproc-virtio.2.auto/virtio0/virtio0.rpmsg_ctrl.0.0/rpmsg/rpmsg_ctrl0
lrwxrwxrwx 1 root root 0 10月 24 01:04 rpmsg_ctrl1 -> ../../devices/platform/bus@f0000/bus@f0000:bus@b00000/bus@f0000:bus@b00000:r5fss@78000000/78000000.r5f/remoteproc/remoteproc1/rproc-virtio.3.auto/virtio1/virtio1.rpmsg_ctrl.0.0/rpmsg/rpmsg_ctrl1
root@AM62x:~#
root@AM62x:~# ll /sys/bus/rpmsg/devices
total 0
lrwxrwxrwx 1 root root 0 10月 24 01:04 virtio0.rpmsg_chrdev.-1.14 -> ../../../devices/platform/bus@f0000/bus@f0000:bus@4000000/5000000.m4fss/remoteproc/remoteproc0/rproc-virtio.2.auto/virtio0/virtio0.rpmsg_chrdev.-1.14
lrwxrwxrwx 1 root root 0 10月 24 01:04 virtio0.rpmsg_ctrl.0.0 -> ../../../devices/platform/bus@f0000/bus@f0000:bus@4000000/5000000.m4fss/remoteproc/remoteproc0/rproc-virtio.2.auto/virtio0/virtio0.rpmsg_ctrl.0.0
lrwxrwxrwx 1 root root 0 10月 24 01:04 virtio0.rpmsg_ns.53.53 -> ../../../devices/platform/bus@f0000/bus@f0000:bus@4000000/5000000.m4fss/remoteproc/remoteproc0/rproc-virtio.2.auto/virtio0/virtio0.rpmsg_ns.53.53
lrwxrwxrwx 1 root root 0 10月 24 01:04 virtio0.ti.ipc4.ping-pong.-1.13 -> ../../../devices/platform/bus@f0000/bus@f0000:bus@4000000/5000000.m4fss/remoteproc/remoteproc0/rproc-virtio.2.auto/virtio0/virtio0.ti.ipc4.ping-pong.-1.13
lrwxrwxrwx 1 root root 0 10月 24 01:04 virtio1.rpmsg_chrdev.-1.14 -> ../../../devices/platform/bus@f0000/bus@f0000:bus@b00000/bus@f0000:bus@b00000:r5fss@78000000/78000000.r5f/remoteproc/remoteproc1/rproc-virtio.3.auto/virtio1/virtio1.rpmsg_chrdev.-1.14
lrwxrwxrwx 1 root root 0 10月 24 01:04 virtio1.rpmsg_ctrl.0.0 -> ../../../devices/platform/bus@f0000/bus@f0000:bus@b00000/bus@f0000:bus@b00000:r5fss@78000000/78000000.r5f/remoteproc/remoteproc1/rproc-virtio.3.auto/virtio1/virtio1.rpmsg_ctrl.0.0
lrwxrwxrwx 1 root root 0 10月 24 01:04 virtio1.rpmsg_ns.53.53 -> ../../../devices/platform/bus@f0000/bus@f0000:bus@b00000/bus@f0000:bus@b00000:r5fss@78000000/78000000.r5f/remoteproc/remoteproc1/rproc-virtio.3.auto/virtio1/virtio1.rpmsg_ns.53.53
lrwxrwxrwx 1 root root 0 10月 24 01:04 virtio1.ti.ipc4.ping-pong.-1.13 -> ../../../devices/platform/bus@f0000/bus@f0000:bus@b00000/bus@f0000:bus@b00000:r5fss@78000000/78000000.r5f/remoteproc/remoteproc1/rproc-virtio.3.auto/virtio1/virtio1.ti.ipc4.ping-pong.-1.13



root@AM62x:~# ll /sys/bus/virtio/devices
total 0
lrwxrwxrwx 1 root root 0 10月 24 01:04 virtio0 -> ../../../devices/platform/bus@f0000/bus@f0000:bus@4000000/5000000.m4fss/remoteproc/remoteproc0/rproc-virtio.2.auto/virtio0
lrwxrwxrwx 1 root root 0 10月 24 01:04 virtio1 -> ../../../devices/platform/bus@f0000/bus@f0000:bus@b00000/bus@f0000:bus@b00000:r5fss@78000000/78000000.r5f/remoteproc/remoteproc1/rproc-virtio.3.auto/virtio1




root@AM62x:~# ll /sys/class/remoteproc
total 0
lrwxrwxrwx 1 root root 0 10月 24 01:04 remoteproc0 -> ../../devices/platform/bus@f0000/bus@f0000:bus@4000000/5000000.m4fss/remoteproc/remoteproc0
lrwxrwxrwx 1 root root 0 10月 24 01:04 remoteproc1 -> ../../devices/platform/bus@f0000/bus@f0000:bus@b00000/bus@f0000:bus@b00000:r5fss@78000000/78000000.r5f/remoteproc/remoteproc1
lrwxrwxrwx 1 root root 0 10月 24 01:04 remoteproc2 -> ../../devices/platform/bus@f0000/30040000.pruss/30074000.pru/remoteproc/remoteproc2
lrwxrwxrwx 1 root root 0 10月 24 01:04 remoteproc3 -> ../../devices/platform/bus@f0000/30040000.pruss/30078000.pru/remoteproc/remoteproc3
root@AM62x:~#
root@AM62x:~# head /sys/class/remoteproc/remoteproc*/name
==> /sys/class/remoteproc/remoteproc0/name <==
5000000.m4fss

==> /sys/class/remoteproc/remoteproc1/name <==
78000000.r5f

==> /sys/class/remoteproc/remoteproc2/name <==
30074000.pru

==> /sys/class/remoteproc/remoteproc3/name <==
30078000.pru
root@AM62x:~#
root@AM62x:~# head /sys/class/remoteproc/remoteproc*/state
==> /sys/class/remoteproc/remoteproc0/state <==
running

==> /sys/class/remoteproc/remoteproc1/state <==
attached

==> /sys/class/remoteproc/remoteproc2/state <==
offline

==> /sys/class/remoteproc/remoteproc3/state <==
offline
root@AM62x:~#
root@AM62x:~# head /sys/class/remoteproc/remoteproc*/firmware
==> /sys/class/remoteproc/remoteproc0/firmware <==
am62-mcu-m4f0_0-fw

==> /sys/class/remoteproc/remoteproc1/firmware <==
unknown

==> /sys/class/remoteproc/remoteproc2/firmware <==
am62x-pru0-fw

==> /sys/class/remoteproc/remoteproc3/firmware <==
am62x-pru1-fw


root@AM62x:~# head /sys/class/remoteproc/remoteproc*/coredump
==> /sys/class/remoteproc/remoteproc0/coredump <==
disabled

==> /sys/class/remoteproc/remoteproc1/coredump <==
disabled

==> /sys/class/remoteproc/remoteproc2/coredump <==
disabled

==> /sys/class/remoteproc/remoteproc3/coredump <==
disabled
root@AM62x:~# head /sys/class/remoteproc/remoteproc*/recovery
==> /sys/class/remoteproc/remoteproc0/recovery <==
disabled

==> /sys/class/remoteproc/remoteproc1/recovery <==
disabled

==> /sys/class/remoteproc/remoteproc2/recovery <==
disabled

==> /sys/class/remoteproc/remoteproc3/recovery <==
disabled


root@AM62x:~# ll /sys/class/remoteproc/remoteproc*/rproc-virtio.*.auto/virtio*/
/sys/class/remoteproc/remoteproc0/rproc-virtio.2.auto/virtio0/:
total 0
-r--r--r-- 1 root root 4096 10月 24 01:05 device
lrwxrwxrwx 1 root root    0 10月 24 01:05 driver -> ../../../../../../../../../bus/virtio/drivers/virtio_rpmsg_bus
-r--r--r-- 1 root root 4096 10月 24 01:05 features
-r--r--r-- 1 root root 4096 10月 24 01:05 modalias
drwxr-xr-x 2 root root    0 10月 24 01:05 power
-r--r--r-- 1 root root 4096 10月 24 01:05 status
lrwxrwxrwx 1 root root    0 10月 24 01:05 subsystem -> ../../../../../../../../../bus/virtio
-rw-r--r-- 1 root root 4096 10月 24 01:05 uevent
-r--r--r-- 1 root root 4096 10月 24 01:05 vendor
drwxr-xr-x 4 root root    0 10月 24 01:05 virtio0.rpmsg_chrdev.-1.14
drwxr-xr-x 4 root root    0 10月 24 01:05 virtio0.rpmsg_ctrl.0.0
drwxr-xr-x 3 root root    0 10月 24 01:05 virtio0.rpmsg_ns.53.53
drwxr-xr-x 3 root root    0 10月 24 01:05 virtio0.ti.ipc4.ping-pong.-1.13

/sys/class/remoteproc/remoteproc1/rproc-virtio.3.auto/virtio1/:
total 0
-r--r--r-- 1 root root 4096 10月 24 01:05 device
lrwxrwxrwx 1 root root    0 10月 24 01:05 driver -> ../../../../../../../../../../bus/virtio/drivers/virtio_rpmsg_bus
-r--r--r-- 1 root root 4096 10月 24 01:05 features
-r--r--r-- 1 root root 4096 10月 24 01:05 modalias
drwxr-xr-x 2 root root    0 10月 24 01:05 power
-r--r--r-- 1 root root 4096 10月 24 01:05 status
lrwxrwxrwx 1 root root    0 10月 24 01:05 subsystem -> ../../../../../../../../../../bus/virtio
-rw-r--r-- 1 root root 4096 10月 24 01:05 uevent
-r--r--r-- 1 root root 4096 10月 24 01:05 vendor
drwxr-xr-x 4 root root    0 10月 24 01:05 virtio1.rpmsg_chrdev.-1.14
drwxr-xr-x 4 root root    0 10月 24 01:05 virtio1.rpmsg_ctrl.0.0
drwxr-xr-x 3 root root    0 10月 24 01:05 virtio1.rpmsg_ns.53.53
drwxr-xr-x 3 root root    0 10月 24 01:05 virtio1.ti.ipc4.ping-pong.-1.13





root@AM62x:~# ll /sys/kernel/debug/remoteproc/*
/sys/kernel/debug/remoteproc/remoteproc0:
total 0
-r-------- 1 root root 0 10月 24 00:59 carveout_memories
-rw------- 1 root root 0 10月 24 00:59 coredump
--w------- 1 root root 0 10月 24 00:59 crash
-r-------- 1 root root 0 10月 24 00:59 name
-rw------- 1 root root 0 10月 24 00:59 recovery
-r-------- 1 root root 0 10月 24 00:59 resource_table
-r-------- 1 root root 0 10月 24 00:59 trace0

/sys/kernel/debug/remoteproc/remoteproc1:
total 0
-r-------- 1 root root 0 10月 24 00:59 carveout_memories
-rw------- 1 root root 0 10月 24 00:59 coredump
--w------- 1 root root 0 10月 24 00:59 crash
-r-------- 1 root root 0 10月 24 00:59 name
-rw------- 1 root root 0 10月 24 00:59 recovery
-r-------- 1 root root 0 10月 24 00:59 resource_table
-r-------- 1 root root 0 10月 24 00:59 trace0

/sys/kernel/debug/remoteproc/remoteproc2:
total 0
-r-------- 1 root root 0 10月 24 00:59 carveout_memories
-rw------- 1 root root 0 10月 24 00:59 coredump
--w------- 1 root root 0 10月 24 00:59 crash
-r-------- 1 root root 0 10月 24 00:59 name
-rw------- 1 root root 0 10月 24 00:59 recovery
-r-------- 1 root root 0 10月 24 00:59 regs
-r-------- 1 root root 0 10月 24 00:59 resource_table
-rw------- 1 root root 0 10月 24 00:59 single_step

/sys/kernel/debug/remoteproc/remoteproc3:
total 0
-r-------- 1 root root 0 10月 24 00:59 carveout_memories
-rw------- 1 root root 0 10月 24 00:59 coredump
--w------- 1 root root 0 10月 24 00:59 crash
-r-------- 1 root root 0 10月 24 00:59 name
-rw------- 1 root root 0 10月 24 00:59 recovery
-r-------- 1 root root 0 10月 24 00:59 regs
-r-------- 1 root root 0 10月 24 00:59 resource_table
-rw------- 1 root root 0 10月 24 00:59 single_step
root@AM62x:~#
root@AM62x:~# head /sys/kernel/debug/remoteproc/remoteproc*/name
==> /sys/kernel/debug/remoteproc/remoteproc0/name <==
5000000.m4fss

==> /sys/kernel/debug/remoteproc/remoteproc1/name <==
78000000.r5f

==> /sys/kernel/debug/remoteproc/remoteproc2/name <==
30074000.pru

==> /sys/kernel/debug/remoteproc/remoteproc3/name <==
30078000.pru



root@AM62x:~# head /sys/kernel/debug/remoteproc/remoteproc*/trace0
==> /sys/kernel/debug/remoteproc/remoteproc0/trace0 <==
[m4f0-0]     0.001146s : [IPC RPMSG ECHO] Version: REL.MCUSDK.K3.10.01.00.33+ (Feb 26 2025 03:22:51):
[m4f0-0]     0.005627s : [IPC RPMSG ECHO] Remote Core waiting for messages at end point 13 ... !!!
[m4f0-0]     0.011659s : [IPC RPMSG ECHO] Remote Core waiting for messages at end point 14 ... !!!

==> /sys/kernel/debug/remoteproc/remoteproc1/trace0 <==
[r5f0-0]     0.000931s : Sciclient direct init..... SUCCESS
[r5f0-0]     0.002024s : Sciserver Testapp Built On: Nov 14 2024 14:00:38
[r5f0-0]     0.006626s : Sciserver Version: v2024.11.0.0-REL.MCUSDK.K3.10.01.00.10+
[r5f0-0]     0.012121s : RM_PM_HAL Version: v10.01.08
[r5f0-0]     0.014873s : Starting Sciserver..... PASSED
[r5f0-0]     0.017807s : [IPC RPMSG ECHO] Version: REL.MCUSDK.K3.10.01.00.10+ (Nov 14 2024 14:02:25):
[r5f0-0]    17.051408s : [IPC RPMSG ECHO] Message exchange started with RTOS cores !!!
[r5f0-0]    17.057266s : [IPC RPMSG ECHO] Remote Core waiting for messages at end point 13 ... !!!
[r5f0-0]    17.066096s : [IPC RPMSG ECHO] Remote Core waiting for messages at end point 14 ... !!!
[r5f0-0]    23.240178s : [IPC RPMSG ECHO] All echoed messages received by main core from 1 remote cores !!!



root@AM62x:~# head /sys/kernel/debug/remoteproc/remoteproc*/carveout_memories -n 100
==> /sys/kernel/debug/remoteproc/remoteproc0/carveout_memories <==
Carveout memory entry:
        Name: vdev0vring0
        Virtual address: 00000000d1b7b35b
        DMA address: 0x000000009cb00000
        Device address: 0x9cb00000
        Length: 0x3000 Bytes

Carveout memory entry:
        Name: vdev0vring1
        Virtual address: 00000000a8bf7470
        DMA address: 0x000000009cb04000
        Device address: 0x9cb04000
        Length: 0x3000 Bytes


==> /sys/kernel/debug/remoteproc/remoteproc1/carveout_memories <==
Carveout memory entry:
        Name: vdev0vring0
        Virtual address: 0000000092344a32
        DMA address: 0x000000009da00000
        Device address: 0x9da00000
        Length: 0x3000 Bytes

Carveout memory entry:
        Name: vdev0vring1
        Virtual address: 0000000012b5871e
        DMA address: 0x000000009da04000
        Device address: 0x9da04000
        Length: 0x3000 Bytes


==> /sys/kernel/debug/remoteproc/remoteproc2/carveout_memories <==

==> /sys/kernel/debug/remoteproc/remoteproc3/carveout_memories <==



root@AM62x:~# head /sys/kernel/debug/remoteproc/remoteproc*/coredump
==> /sys/kernel/debug/remoteproc/remoteproc0/coredump <==
disabled

==> /sys/kernel/debug/remoteproc/remoteproc1/coredump <==
disabled

==> /sys/kernel/debug/remoteproc/remoteproc2/coredump <==
disabled

==> /sys/kernel/debug/remoteproc/remoteproc3/coredump <==
disabled




root@AM62x:~# head /sys/kernel/debug/remoteproc/remoteproc*/recovery
==> /sys/kernel/debug/remoteproc/remoteproc0/recovery <==
disabled

==> /sys/kernel/debug/remoteproc/remoteproc1/recovery <==
disabled

==> /sys/kernel/debug/remoteproc/remoteproc2/recovery <==
disabled

==> /sys/kernel/debug/remoteproc/remoteproc3/recovery <==
disabled


root@AM62x:~# head /sys/kernel/debug/remoteproc/remoteproc*/resource_table -n 500
==> /sys/kernel/debug/remoteproc/remoteproc0/resource_table <==
Entry 0 is of type vdev
  ID 7
  Notify ID 0
  Device features 0x1
  Guest features 0x1
  Config length 0x0
  Status 0x7
  Number of vrings 2
  Reserved (should be zero) [0][0]

  Vring 0
    Device Address 0x9cb00000
    Alignment 4096
    Number of buffers 256
    Notify ID 0
    Physical Address 0x0

  Vring 1
    Device Address 0x9cb04000
    Alignment 4096
    Number of buffers 256
    Notify ID 1
    Physical Address 0x0

Entry 1 is of type trace
  Device Address 0x38000
  Length 0x1000 Bytes
  Reserved (should be zero) [0]
  Name trace:m4fss0_0


==> /sys/kernel/debug/remoteproc/remoteproc1/resource_table <==
Entry 0 is of type vdev
  ID 7
  Notify ID 0
  Device features 0x1
  Guest features 0x1
  Config length 0x0
  Status 0x7
  Number of vrings 2
  Reserved (should be zero) [0][0]

  Vring 0
    Device Address 0x9da00000
    Alignment 4096
    Number of buffers 256
    Notify ID 0
    Physical Address 0x0

  Vring 1
    Device Address 0x9da04000
    Alignment 4096
    Number of buffers 256
    Notify ID 1
    Physical Address 0x0

Entry 1 is of type trace
  Device Address 0x9dcb2780
  Length 0x1000 Bytes
  Reserved (should be zero) [0]
  Name trace:r5fss0_0


==> /sys/kernel/debug/remoteproc/remoteproc2/resource_table <==
No resource table found

==> /sys/kernel/debug/remoteproc/remoteproc3/resource_table <==
No resource table found



root@AM62x:~# rpmsg_char_simple -r 9 -n 10
Created endpt device rpmsg-char-9-1001, fd = 4 port = 1026
Exchanging 10 messages with rpmsg device rpmsg-char-9-1001 on rproc id 9 ...

Sending message #0: hello there 0!
Received message #0: round trip delay(usecs) = 123660
hello there 0!
Sending message #1: hello there 1!
Received message #1: round trip delay(usecs) = 92380
hello there 1!
Sending message #2: hello there 2!
Received message #2: round trip delay(usecs) = 634345
hello there 2!
Sending message #3: hello there 3!
Received message #3: round trip delay(usecs) = 84530
hello there 3!
Sending message #4: hello there 4!
Received message #4: round trip delay(usecs) = 75065
hello there 4!
Sending message #5: hello there 5!
Received message #5: round trip delay(usecs) = 71545
hello there 5!
Sending message #6: hello there 6!
Received message #6: round trip delay(usecs) = 69315
hello there 6!
Sending message #7: hello there 7!
Received message #7: round trip delay(usecs) = 66695
hello there 7!
Sending message #8: hello there 8!
Received message #8: round trip delay(usecs) = 59995
hello there 8!
Sending message #9: hello there 9!
Received message #9: round trip delay(usecs) = 60265
hello there 9!

Communicated 10 messages successfully on rpmsg-char-9-1001

TEST STATUS: PASSED



root@AM62x:~# rpmsg_char_simple -r 15 -n 10
Created endpt device rpmsg-char-15-1003, fd = 4 port = 1026
Exchanging 10 messages with rpmsg device rpmsg-char-15-1003 on rproc id 15 ...

Sending message #0: hello there 0!
Received message #0: round trip delay(usecs) = 117345
hello there 0!
Sending message #1: hello there 1!
Received message #1: round trip delay(usecs) = 56270
hello there 1!
Sending message #2: hello there 2!
Received message #2: round trip delay(usecs) = 53160
hello there 2!
Sending message #3: hello there 3!
Received message #3: round trip delay(usecs) = 51455
hello there 3!
Sending message #4: hello there 4!
Received message #4: round trip delay(usecs) = 49990
hello there 4!
Sending message #5: hello there 5!
Received message #5: round trip delay(usecs) = 133830
hello there 5!
Sending message #6: hello there 6!
Received message #6: round trip delay(usecs) = 54975
hello there 6!
Sending message #7: hello there 7!
Received message #7: round trip delay(usecs) = 91560
hello there 7!
Sending message #8: hello there 8!
Received message #8: round trip delay(usecs) = 50990
hello there 8!
Sending message #9: hello there 9!
Received message #9: round trip delay(usecs) = 49695
hello there 9!

Communicated 10 messages successfully on rpmsg-char-15-1003

TEST STATUS: PASSED



root@AM62x:~# echo stop > /sys/class/remoteproc/remoteproc0/state
[  493.938218] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: rpmsg sample client driver is removed
[  493.970769] remoteproc remoteproc0: stopped remote processor 5000000.m4fss
root@AM62x:~#
root@AM62x:~# echo detach > /sys/class/remoteproc/remoteproc1/state
[  503.146245] rpmsg_client_sample virtio1.ti.ipc4.ping-pong.-1.13: rpmsg sample client driver is removed
[  503.155709] remoteproc remoteproc1: detached remote processor 78000000.r5f



root@AM62x:~# echo start > /sys/class/remoteproc/remoteproc0/state
[  538.362568] remoteproc remoteproc0: powering up 5000000.m4fss
[  538.369168] remoteproc remoteproc0: Booting fw image am62-mcu-m4f0_0-fw, size 55100
[  538.376500] rproc-virtio rproc-virtio.2.auto: assigned reserved memory node m4f-dma-memory@9cb00000
[  538.380475] virtio_rpmsg_bus virtio0: rpmsg host is online
[  538.380521] rproc-virtio rproc-virtio.2.auto: registered virtio0 (type 7)
[  538.380530] remoteproc remoteproc0: remote processor 5000000.m4fss is now up
[  538.382356] virtio_rpmsg_bus virtio0: creating channel ti.ipc4.ping-pong addr 0xd
[  538.382628] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: new channel: 0x400 -> 0xd!
[  538.382698] virtio_rpmsg_bus virtio0: creating channel rpmsg_chrdev addr 0xe
[  538.388763] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 1 (src: 0xd)
[  538.388850] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 2 (src: 0xd)
[  538.388903] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 3 (src: 0xd)
[  538.388958] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 4 (src: 0xd)
root@AM62x:~# [  538.389021] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 5 (src: 0xd)
[  538.389073] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 6 (src: 0xd)
[  538.389139] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 7 (src: 0xd)
[  538.389191] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 8 (src: 0xd)
[  538.389244] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 9 (src: 0xd)
[  538.389298] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 10 (src: 0xd)
[  538.389351] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 11 (src: 0xd)
[  538.389410] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 12 (src: 0xd)
[  538.389474] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 13 (src: 0xd)
[  538.389532] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 14 (src: 0xd)
[  538.389589] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 15 (src: 0xd)
[  538.389650] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 16 (src: 0xd)
[  538.389715] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 17 (src: 0xd)
[  538.389766] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 18 (src: 0xd)
[  538.389828] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 19 (src: 0xd)
[  538.389892] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 20 (src: 0xd)
[  538.389944] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 21 (src: 0xd)
[  538.390008] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 22 (src: 0xd)
[  538.390074] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 23 (src: 0xd)
[  538.390131] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 24 (src: 0xd)
[  538.390197] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 25 (src: 0xd)
[  538.390259] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 26 (src: 0xd)
[  538.390311] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 27 (src: 0xd)
[  538.390375] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 28 (src: 0xd)
[  538.390438] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 29 (src: 0xd)
[  538.390490] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 30 (src: 0xd)
[  538.390551] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 31 (src: 0xd)
[  538.390613] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 32 (src: 0xd)
[  538.390665] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 33 (src: 0xd)
[  538.390732] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 34 (src: 0xd)
[  538.390793] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 35 (src: 0xd)
[  538.390851] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 36 (src: 0xd)
[  538.390914] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 37 (src: 0xd)
[  538.390977] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 38 (src: 0xd)
[  538.391034] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 39 (src: 0xd)
[  538.391091] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 40 (src: 0xd)
[  538.391156] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 41 (src: 0xd)
[  538.391212] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 42 (src: 0xd)
[  538.391271] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 43 (src: 0xd)
[  538.391332] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 44 (src: 0xd)
[  538.391389] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 45 (src: 0xd)
[  538.391448] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 46 (src: 0xd)
[  538.391515] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 47 (src: 0xd)
[  538.391576] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 48 (src: 0xd)
[  538.391636] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 49 (src: 0xd)
[  538.391695] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 50 (src: 0xd)
[  538.391756] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 51 (src: 0xd)
[  538.391817] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 52 (src: 0xd)
[  538.391877] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 53 (src: 0xd)
[  538.391930] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 54 (src: 0xd)
[  538.391993] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 55 (src: 0xd)
[  538.392060] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 56 (src: 0xd)
[  538.392129] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 57 (src: 0xd)
[  538.392189] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 58 (src: 0xd)
[  538.392250] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 59 (src: 0xd)
[  538.392309] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 60 (src: 0xd)
[  538.392364] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 61 (src: 0xd)
[  538.392428] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 62 (src: 0xd)
[  538.392489] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 63 (src: 0xd)
[  538.392541] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 64 (src: 0xd)
[  538.392602] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 65 (src: 0xd)
[  538.392656] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 66 (src: 0xd)
[  538.392713] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 67 (src: 0xd)
[  538.392776] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 68 (src: 0xd)
[  538.392831] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 69 (src: 0xd)
[  538.392892] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 70 (src: 0xd)
[  538.392954] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 71 (src: 0xd)
[  538.393007] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 72 (src: 0xd)
[  538.393073] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 73 (src: 0xd)
[  538.393141] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 74 (src: 0xd)
[  538.393199] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 75 (src: 0xd)
[  538.393270] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 76 (src: 0xd)
[  538.393334] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 77 (src: 0xd)
[  538.393410] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 78 (src: 0xd)
[  538.393468] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 79 (src: 0xd)
[  538.393525] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 80 (src: 0xd)
[  538.393585] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 81 (src: 0xd)
[  538.393646] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 82 (src: 0xd)
[  538.393706] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 83 (src: 0xd)
[  538.393765] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 84 (src: 0xd)
[  538.393821] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 85 (src: 0xd)
[  538.393886] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 86 (src: 0xd)
[  538.393947] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 87 (src: 0xd)
[  538.394010] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 88 (src: 0xd)
[  538.394071] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 89 (src: 0xd)
[  538.394133] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 90 (src: 0xd)
[  538.394187] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 91 (src: 0xd)
[  538.394249] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 92 (src: 0xd)
[  538.394309] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 93 (src: 0xd)
[  538.394361] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 94 (src: 0xd)
[  538.394426] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 95 (src: 0xd)
[  538.394487] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 96 (src: 0xd)
[  538.394539] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 97 (src: 0xd)
[  538.394608] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 98 (src: 0xd)
[  538.394670] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 99 (src: 0xd)
[  538.394723] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: incoming msg 100 (src: 0xd)
[  538.394730] rpmsg_client_sample virtio0.ti.ipc4.ping-pong.-1.13: goodbye!



root@AM62x:~# echo start > /sys/class/remoteproc/remoteproc1/state
[  617.691560] remoteproc remoteproc1: attaching to 78000000.r5f
[  617.694231] rproc-virtio rproc-virtio.3.auto: assigned reserved memory node r5f-dma-memory@9da00000
[  617.697789] virtio_rpmsg_bus virtio1: rpmsg host is online
[  617.697839] rproc-virtio rproc-virtio.3.auto: registered virtio1 (type 7)
[  617.697848] remoteproc remoteproc1: remote processor 78000000.r5f is now attached

```

# 设备树
```bash

# mailbox
--------------------------------------------------------------------------------------------
# k3-am62-main.dtsi
mailbox0_cluster0: mailbox@29000000 {
        compatible = "ti,am64-mailbox";
        reg = <0x00 0x29000000 0x00 0x200>;
        interrupts = <GIC_SPI 76 IRQ_TYPE_LEVEL_HIGH>,
                     <GIC_SPI 77 IRQ_TYPE_LEVEL_HIGH>;
        #mbox-cells = <1>;
        ti,mbox-num-users = <4>;
        ti,mbox-num-fifos = <16>;
};


# k3-am62x-sk-common.dtsi
&mailbox0_cluster0 {
        mbox_m4_0: mbox-m4-0 {
                ti,mbox-rx = <0 0 0>;
                ti,mbox-tx = <1 0 0>;
        };
        mbox_r5_0: mbox-r5-0 {
                ti,mbox-rx = <2 0 0>;
                ti,mbox-tx = <3 0 0>;
        };
};

# k3-am625-sk.dts
&mailbox0_cluster0 {
        mbox_m4_0: mbox-m4-0 {
                ti,mbox-rx = <0 0 0>;
                ti,mbox-tx = <1 0 0>;
        };
};

# reserved-memory
--------------------------------------------------------------------------------------------
# k3-am62x-sk-common.dtsi
reserved-memory {
        #address-cells = <2>;
        #size-cells = <2>;
        ranges;


        mcu_m4fss_dma_memory_region: m4f-dma-memory@9cb00000 {
                compatible = "shared-dma-pool";
                reg = <0x00 0x9cb00000 0x00 0x100000>;
                no-map;
        };

        mcu_m4fss_memory_region: m4f-memory@9cc00000 {
                compatible = "shared-dma-pool";
                reg = <0x00 0x9cc00000 0x00 0xe00000>;
                no-map;
        };


        wkup_r5fss0_core0_dma_memory_region: r5f-dma-memory@9da00000 {
                compatible = "shared-dma-pool";
                reg = <0x00 0x9da00000 0x00 0x00100000>;
                no-map;
        };

        wkup_r5fss0_core0_memory_region: r5f-memory@9db00000 {
                compatible = "shared-dma-pool";
                reg = <0x00 0x9db00000 0x00 0x00c00000>;
                no-map;
        };

};

# mcu_m4fss
--------------------------------------------------------------------------------------------
# k3-am62-mcu.dtsi
mcu_m4fss: m4fss@5000000 {
	compatible = "ti,am64-m4fss";
	reg = <0x00 0x5000000 0x00 0x30000>,
	      <0x00 0x5040000 0x00 0x10000>;
	reg-names = "iram", "dram";
	resets = <&k3_reset 9 1>;
	firmware-name = "am62-mcu-m4f0_0-fw";
	ti,sci = <&dmsc>;
	ti,sci-dev-id = <9>;
	ti,sci-proc-ids = <0x18 0xff>;
	status = "disabled";
};

# k3-am62x-sk-common.dtsi
&mcu_m4fss {
        mboxes = <&mailbox0_cluster0 &mbox_m4_0>;
        memory-region = <&mcu_m4fss_dma_memory_region>,
                        <&mcu_m4fss_memory_region>;
        status = "okay";
};


# wkup_r5fss0
--------------------------------------------------------------------------------------------
# k3-am62-wakeup.dtsi
wkup_r5fss0: r5fss@78000000 {
        compatible = "ti,am62-r5fss";
        #address-cells = <1>;
        #size-cells = <1>;
        ranges = <0x78000000 0x00 0x78000000 0x8000>,
                         <0x78100000 0x00 0x78100000 0x8000>;
        power-domains = <&k3_pds 119 TI_SCI_PD_EXCLUSIVE>;

        wkup_r5fss0_core0: r5f@78000000 {
                compatible = "ti,am62-r5f";
                reg = <0x78000000 0x00008000>,
                      <0x78100000 0x00008000>;
                reg-names = "atcm", "btcm";
                ti,sci = <&dmsc>;
                ti,sci-dev-id = <121>;
                ti,sci-proc-ids = <0x01 0xff>;
                resets = <&k3_reset 121 1>;
                firmware-name = "ti-sysfw/ti-fs-stub-firmware-am62x-gp-signed.bin";
                ti,atcm-enable = <1>;
                ti,btcm-enable = <1>;
                ti,loczrama = <1>;
        };
};


# k3-am62x-sk-common.dtsi
&wkup_r5fss0_core0 {
        mboxes = <&mailbox0_cluster0 &mbox_r5_0>;
        memory-region = <&wkup_r5fss0_core0_dma_memory_region>,
                        <&wkup_r5fss0_core0_memory_region>;
};
```