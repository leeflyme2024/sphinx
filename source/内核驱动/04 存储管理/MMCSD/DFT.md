# U-Boot

# Kernel
## emmc设备
```bash
root@AM62x:/# ll dev/mmcblk0*
brw-rw---- 1 root disk 179, 32  8月  9 19:53 dev/mmcblk0
brw-rw---- 1 root disk 179, 64  8月  9 19:46 dev/mmcblk0boot0
brw-rw---- 1 root disk 179, 96  8月  9 19:46 dev/mmcblk0boot1
brw-rw---- 1 root disk 179, 33  8月  9 19:53 dev/mmcblk0p1
brw-rw---- 1 root disk 179, 34  8月  9 19:53 dev/mmcblk0p2
brw-rw---- 1 root disk 179, 35  8月  9 19:53 dev/mmcblk0p3
crw------- 1 root root 238,  0  8月  9 19:46 dev/mmcblk0rpmb
```

## emmc 速率

[AM623: Reduce the rate of mmc0 - Processors forum - Processors - TI E2E support forums](https://e2e.ti.com/support/processors-group/processors/f/processors-forum/1452537/am623-reduce-the-rate-of-mmc0)

```bash
sdhci0: mmc@fa10000 {
	compatible = "ti,am62-sdhci";
	reg = <0x00 0xfa10000 0x00 0x260>, <0x00 0xfa18000 0x00 0x134>;
	interrupts = <GIC_SPI 133 IRQ_TYPE_LEVEL_HIGH>;
	power-domains = <&k3_pds 57 TI_SCI_PD_EXCLUSIVE>;
	clocks = <&k3_clks 57 5>, <&k3_clks 57 6>;
	clock-names = "clk_ahb", "clk_xin";
	assigned-clocks = <&k3_clks 57 6>;
	assigned-clock-parents = <&k3_clks 57 8>;
	mmc-ddr-1_8v;
	mmc-hs200-1_8v;
	ti,trm-icp = <0x2>;
	bus-width = <8>;
	ti,clkbuf-sel = <0x7>;
	ti,otap-del-sel-legacy = <0x0>;
	ti,otap-del-sel-mmc-hs = <0x0>;
    // ti,otap-del-sel-sd-hs = <0x0>;
    // ti,otap-del-sel-sdr12 = <0x0>;
    // ti,otap-del-sel-sdr25 = <0x0>;
    // ti,otap-del-sel-sdr50 = <0x8>;
    ti,otap-del-sel-sdr104 = <0x7>;
    // ti,otap-del-sel-ddr50 = <0x5>;

	// ti,otap-del-sel-ddr52 = <0x5>;
	// ti,otap-del-sel-hs200 = <0x5>;
	ti,itap-del-sel-legacy = <0xa>;
	ti,itap-del-sel-mmc-hs = <0x1>;
	status = "okay";
};
```
![[Pasted image 20241218155140.png]]

```c
// /linux-rt-5.10.168+gitAUTOINC+c1a1291911-gc1a1291911/include/linux/mmc/host.h
struct mmc_ios {
        unsigned int    clock;                  /* clock rate */
        unsigned short  vdd;
        unsigned int    power_delay_ms;         /* waiting for stable power */

/* vdd stores the bit number of the selected voltage range from below. */

        unsigned char   bus_mode;               /* command output mode */

#define MMC_BUSMODE_OPENDRAIN   1
#define MMC_BUSMODE_PUSHPULL    2

        unsigned char   chip_select;            /* SPI chip select */

#define MMC_CS_DONTCARE         0
#define MMC_CS_HIGH             1
#define MMC_CS_LOW              2

        unsigned char   power_mode;             /* power supply mode */

#define MMC_POWER_OFF           0
#define MMC_POWER_UP            1
#define MMC_POWER_ON            2
#define MMC_POWER_UNDEFINED     3

        unsigned char   bus_width;              /* data bus width */

#define MMC_BUS_WIDTH_1         0
#define MMC_BUS_WIDTH_4         2
#define MMC_BUS_WIDTH_8         3

        unsigned char   timing;                 /* timing specification used */

#define MMC_TIMING_LEGACY       0
#define MMC_TIMING_MMC_HS       1
#define MMC_TIMING_SD_HS        2
#define MMC_TIMING_UHS_SDR12    3
#define MMC_TIMING_UHS_SDR25    4
#define MMC_TIMING_UHS_SDR50    5
#define MMC_TIMING_UHS_SDR104   6
#define MMC_TIMING_UHS_DDR50    7
#define MMC_TIMING_MMC_DDR52    8
#define MMC_TIMING_MMC_HS200    9
#define MMC_TIMING_MMC_HS400    10

        unsigned char   signal_voltage;         /* signalling voltage (1.8V or 3.3V) */

#define MMC_SIGNAL_VOLTAGE_330  0
#define MMC_SIGNAL_VOLTAGE_180  1
#define MMC_SIGNAL_VOLTAGE_120  2

        unsigned char   drv_type;               /* driver type (A, B, C, D) */

#define MMC_SET_DRIVER_TYPE_B   0
#define MMC_SET_DRIVER_TYPE_A   1
#define MMC_SET_DRIVER_TYPE_C   2
#define MMC_SET_DRIVER_TYPE_D   3

        bool enhanced_strobe;                   /* hs400es selection */
};

```

```c
// linux-rt-5.10.168+gitAUTOINC+c1a1291911-gc1a1291911/drivers/mmc/core/bus.c
/*
 * Register a new MMC card with the driver model.
 */
int mmc_add_card(struct mmc_card *card)
{
        int ret;
        const char *type;
        const char *uhs_bus_speed_mode = "";
        static const char *const uhs_speeds[] = {
                [UHS_SDR12_BUS_SPEED] = "SDR12 ",
                [UHS_SDR25_BUS_SPEED] = "SDR25 ",
                [UHS_SDR50_BUS_SPEED] = "SDR50 ",
                [UHS_SDR104_BUS_SPEED] = "SDR104 ",
                [UHS_DDR50_BUS_SPEED] = "DDR50 ",
        };


        dev_set_name(&card->dev, "%s:%04x", mmc_hostname(card->host), card->rca);

        switch (card->type) {
        case MMC_TYPE_MMC:
                type = "MMC";
                break;
        case MMC_TYPE_SD:
                type = "SD";
                if (mmc_card_blockaddr(card)) {
                        if (mmc_card_ext_capacity(card))
                                type = "SDXC";
                        else
                                type = "SDHC";
                }
                break;
        case MMC_TYPE_SDIO:
                type = "SDIO";
                break;
        case MMC_TYPE_SD_COMBO:
                type = "SD-combo";
                if (mmc_card_blockaddr(card))
                        type = "SDHC-combo";
                break;
        default:
                type = "?";
                break;
        }

        if (mmc_card_uhs(card) &&
                (card->sd_bus_speed < ARRAY_SIZE(uhs_speeds)))
                uhs_bus_speed_mode = uhs_speeds[card->sd_bus_speed];

        if (mmc_host_is_spi(card->host)) {
                pr_info("%s: new %s%s%s card on SPI\n",
                        mmc_hostname(card->host),
                        mmc_card_hs(card) ? "high speed " : "",
                        mmc_card_ddr52(card) ? "DDR " : "",
                        type);
        } else {
                pr_info("%s: new %s%s%s%s%s%s card at address %04x\n",
                        mmc_hostname(card->host),
                        mmc_card_uhs(card) ? "ultra high speed " :
                        (mmc_card_hs(card) ? "high speed " : ""),
                        mmc_card_hs400(card) ? "HS400 " :
                        (mmc_card_hs200(card) ? "HS200 " : ""),
                        mmc_card_hs400es(card) ? "Enhanced strobe " : "",
                        mmc_card_ddr52(card) ? "DDR " : "",
                        uhs_bus_speed_mode, type, card->rca);
        }

#ifdef CONFIG_DEBUG_FS
        mmc_add_card_debugfs(card);
#endif
        card->dev.of_node = mmc_of_find_child_device(card->host, 0);

        device_enable_async_suspend(&card->dev);

        ret = device_add(&card->dev);
        if (ret)
                return ret;

        mmc_card_set_present(card);

        return 0;
}
```

### HS200
![[img_v3_02hm_05d2bae1-de2f-49f1-9ad4-a157d0d590eg.jpg]]

### DDR52
![[img_v3_02hm_ce04178a-10ed-4e25-b887-e49333fc5dag.jpg]]

### DDR50
![[Pasted image 20241218161757.png]]

### SDR104
![[Pasted image 20241218155338.png]]

### SDR50

### SDR25
![[Pasted image 20241218162551.png]]

### SDR12

### mmc-hs

### legacy


## 设备信息
```bash
root@AM62x:/# ll ./sys/kernel/debug/mmc*
./sys/kernel/debug/mmc0:
total 0
-r-------- 1 root root 0  8月  9 19:46 caps
-r-------- 1 root root 0  8月  9 19:46 caps2
-rw------- 1 root root 0  8月  9 19:46 clock
-r-------- 1 root root 0  8月  9 19:46 ios
drwxr-xr-x 2 root root 0  8月  9 19:46 mmc0:0001

./sys/kernel/debug/mmc1:
total 0
-r-------- 1 root root 0  8月  9 19:46 caps
-r-------- 1 root root 0  8月  9 19:46 caps2
-rw------- 1 root root 0  8月  9 19:46 clock
-r-------- 1 root root 0  8月  9 19:46 ios
drwxr-xr-x 2 root root 0  8月  9 19:46 mmc1:5048

./sys/kernel/debug/mmc2:
total 0
-r-------- 1 root root 0  8月  9 19:46 caps
-r-------- 1 root root 0  8月  9 19:46 caps2
-rw------- 1 root root 0  8月  9 19:46 clock
-r-------- 1 root root 0  8月  9 19:46 ios
drwxr-xr-x 2 root root 0  8月  9 19:46 mmc2:0001



root@AM62x:~# ll /sys/kernel/debug/mmc0/
total 0
-r-------- 1 root root 0  8月  9 19:46 caps
-r-------- 1 root root 0  8月  9 19:46 caps2
-rw------- 1 root root 0  8月  9 19:46 clock
-r-------- 1 root root 0  8月  9 19:46 ios
drwxr-xr-x 2 root root 0  8月  9 19:46 mmc0:0001



root@AM62x:~# ll /sys/kernel/debug/mmc0/mmc0\:0001/
total 0
-r-------- 1 root root 0  8月  9 19:46 ext_csd
-r-------- 1 root root 0  8月  9 19:46 state
-r-------- 1 root root 0  8月  9 19:46 status


root@AM62x:~# cat /sys/kernel/debug/mmc0/ios
clock:          52000000 Hz
actual clock:   50000000 Hz
vdd:            21 (3.3 ~ 3.4 V)
bus mode:       2 (push-pull)
chip select:    0 (don't care)
power mode:     2 (on)
bus width:      3 (8 bits)
timing spec:    8 (mmc DDR52)
signal voltage: 1 (1.80 V)
driver type:    0 (driver type B)


root@AM62x:~# cat /sys/kernel/debug/mmc0/clock
52000000
root@AM62x:~# cat /sys/kernel/debug/mmc0/caps
0x4000114b
root@AM62x:~# cat /sys/kernel/debug/mmc0/caps2
0x00860000


root@AM62x:~# cat /sys/kernel/debug/mmc0/mmc0\:0001/ext_csd
000000000000000000000000000000013b010080740000000000000000000000000101000000000000000000000000000000000000000000000000000a000001c80037000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000e90000070100000000151f20000000000000010000000000000006010100000000000008000200571f0506000000000000000000000001008074000c131707071001020106200007ffff550200000000000000010a01000000006405000400000001000000000000000534074040010101000000000000000000000000000000000000000000000000000000000000000000000000001f0100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001f0fffaff1700010301780000013f3f01010100000000000001


root@AM62x:~# cat /sys/kernel/debug/mmc0/mmc0\:0001/state
0x00000005
root@AM62x:~# cat /sys/kernel/debug/mmc0/mmc0\:0001/status
00000900
```

```bash
root@AM62x:~# ll /sys/class/mmc_host/mmc0/mmc0:0001
total 0
drwxr-xr-x 3 root root    0  8月  9 19:50 block
-r--r--r-- 1 root root 4096  8月  9 19:50 cid
-r--r--r-- 1 root root 4096  8月  9 19:50 cmdq_en
-r--r--r-- 1 root root 4096  8月  9 19:50 csd
-r--r--r-- 1 root root 4096  8月  9 19:50 date
lrwxrwxrwx 1 root root    0  8月  9 19:50 driver -> ../../../../../../../bus/mmc/drivers/mmcblk
-r--r--r-- 1 root root 4096  8月  9 19:50 dsr
-r--r--r-- 1 root root 4096  8月  9 19:50 enhanced_area_offset
-r--r--r-- 1 root root 4096  8月  9 19:50 enhanced_area_size
-r--r--r-- 1 root root 4096  8月  9 19:50 enhanced_rpmb_supported
-r--r--r-- 1 root root 4096  8月  9 19:50 erase_size
-r--r--r-- 1 root root 4096  8月  9 19:50 ffu_capable
-r--r--r-- 1 root root 4096  8月  9 19:50 fwrev
-r--r--r-- 1 root root 4096  8月  9 19:50 hwrev
-r--r--r-- 1 root root 4096  8月  9 19:50 life_time
-r--r--r-- 1 root root 4096  8月  9 19:50 manfid
drwxr-xr-x 3 root root    0  8月  9 19:50 mmcblk0rpmb
-r--r--r-- 1 root root 4096  8月  9 19:50 name
-r--r--r-- 1 root root 4096  8月  9 19:50 ocr
-r--r--r-- 1 root root 4096  8月  9 19:50 oemid
drwxr-xr-x 2 root root    0  8月  9 19:50 power
-r--r--r-- 1 root root 4096  8月  9 19:50 pre_eol_info
-r--r--r-- 1 root root 4096  8月  9 19:50 preferred_erase_size
-r--r--r-- 1 root root 4096  8月  9 19:50 prv
-r--r--r-- 1 root root 4096  8月  9 19:50 raw_rpmb_size_mult
-r--r--r-- 1 root root 4096  8月  9 19:50 rca
-r--r--r-- 1 root root 4096  8月  9 19:50 rel_sectors
-r--r--r-- 1 root root 4096  8月  9 19:50 rev
-r--r--r-- 1 root root 4096  8月  9 19:50 serial
lrwxrwxrwx 1 root root    0  8月  9 19:50 subsystem -> ../../../../../../../bus/mmc
-r--r--r-- 1 root root 4096  8月  9 19:50 type
-rw-r--r-- 1 root root 4096  8月  9 19:50 uevent
```


```bash
root@AM62x:/# ll ./sys/kernel/debug/block/mmc*
./sys/kernel/debug/block/mmcblk0:
total 0
drwxr-xr-x 4 root root 0  8月  9 19:46 hctx0
-rw------- 1 root root 0  8月  9 19:46 pm_only
-r-------- 1 root root 0  8月  9 19:46 poll_stat
-r-------- 1 root root 0  8月  9 19:46 requeue_list
drwxr-xr-x 2 root root 0  8月  9 19:46 sched
-rw------- 1 root root 0  8月  9 19:46 state
-rw------- 1 root root 0  8月  9 19:46 write_hints
-r-------- 1 root root 0  8月  9 19:46 zone_wlock

./sys/kernel/debug/block/mmcblk1:
total 0
drwxr-xr-x 4 root root 0  8月  9 19:46 hctx0
-rw------- 1 root root 0  8月  9 19:46 pm_only
-r-------- 1 root root 0  8月  9 19:46 poll_stat
-r-------- 1 root root 0  8月  9 19:46 requeue_list
drwxr-xr-x 2 root root 0  8月  9 19:46 sched
-rw------- 1 root root 0  8月  9 19:46 state
-rw------- 1 root root 0  8月  9 19:46 write_hints
-r-------- 1 root root 0  8月  9 19:46 zone_wlock

./sys/kernel/debug/block/mmcblk0boot0:
total 0
drwxr-xr-x 4 root root 0  8月  9 19:46 hctx0
-rw------- 1 root root 0  8月  9 19:46 pm_only
-r-------- 1 root root 0  8月  9 19:46 poll_stat
-r-------- 1 root root 0  8月  9 19:46 requeue_list
drwxr-xr-x 2 root root 0  8月  9 19:46 sched
-rw------- 1 root root 0  8月  9 19:46 state
-rw------- 1 root root 0  8月  9 19:46 write_hints
-r-------- 1 root root 0  8月  9 19:46 zone_wlock

./sys/kernel/debug/block/mmcblk0boot1:
total 0
drwxr-xr-x 4 root root 0  8月  9 19:46 hctx0
-rw------- 1 root root 0  8月  9 19:46 pm_only
-r-------- 1 root root 0  8月  9 19:46 poll_stat
-r-------- 1 root root 0  8月  9 19:46 requeue_list
drwxr-xr-x 2 root root 0  8月  9 19:46 sched
-rw------- 1 root root 0  8月  9 19:46 state
-rw------- 1 root root 0  8月  9 19:46 write_hints
-r-------- 1 root root 0  8月  9 19:46 zone_wlock
```

```bash
root@AM62x:/# ll ./sys/kernel/debug/regmap/*.mmc
./sys/kernel/debug/regmap/fa00000.mmc:
total 0
-r-------- 1 root root 0  8月  9 19:46 access
-r-------- 1 root root 0  8月  9 19:46 name
-r-------- 1 root root 0  8月  9 19:46 range
-r-------- 1 root root 0  8月  9 19:46 registers

./sys/kernel/debug/regmap/fa10000.mmc:
total 0
-r-------- 1 root root 0  8月  9 19:46 access
-r-------- 1 root root 0  8月  9 19:46 name
-r-------- 1 root root 0  8月  9 19:46 range
-r-------- 1 root root 0  8月  9 19:46 registers

./sys/kernel/debug/regmap/fa20000.mmc:
total 0
-r-------- 1 root root 0  8月  9 19:46 access
-r-------- 1 root root 0  8月  9 19:46 name
-r-------- 1 root root 0  8月  9 19:46 range
-r-------- 1 root root 0  8月  9 19:46 registers
```


```bash
root@AM62x:/# ll ./sys/class/block/mmc*
lrwxrwxrwx 1 root root 0  8月  9 19:59 ./sys/class/block/mmcblk0 -> ../../devices/platform/bus@f0000/fa10000.mmc/mmc_host/mmc0/mmc0:0001/block/mmcblk0
lrwxrwxrwx 1 root root 0  8月  9 19:59 ./sys/class/block/mmcblk0boot0 -> ../../devices/platform/bus@f0000/fa10000.mmc/mmc_host/mmc0/mmc0:0001/block/mmcblk0/mmcblk0boot0
lrwxrwxrwx 1 root root 0  8月  9 19:59 ./sys/class/block/mmcblk0boot1 -> ../../devices/platform/bus@f0000/fa10000.mmc/mmc_host/mmc0/mmc0:0001/block/mmcblk0/mmcblk0boot1
lrwxrwxrwx 1 root root 0  8月  9 19:59 ./sys/class/block/mmcblk0p1 -> ../../devices/platform/bus@f0000/fa10000.mmc/mmc_host/mmc0/mmc0:0001/block/mmcblk0/mmcblk0p1
lrwxrwxrwx 1 root root 0  8月  9 19:59 ./sys/class/block/mmcblk0p2 -> ../../devices/platform/bus@f0000/fa10000.mmc/mmc_host/mmc0/mmc0:0001/block/mmcblk0/mmcblk0p2
lrwxrwxrwx 1 root root 0  8月  9 19:59 ./sys/class/block/mmcblk0p3 -> ../../devices/platform/bus@f0000/fa10000.mmc/mmc_host/mmc0/mmc0:0001/block/mmcblk0/mmcblk0p3
lrwxrwxrwx 1 root root 0  8月  9 19:59 ./sys/class/block/mmcblk1 -> ../../devices/platform/bus@f0000/fa00000.mmc/mmc_host/mmc1/mmc1:5048/block/mmcblk1
lrwxrwxrwx 1 root root 0  8月  9 19:59 ./sys/class/block/mmcblk1p1 -> ../../devices/platform/bus@f0000/fa00000.mmc/mmc_host/mmc1/mmc1:5048/block/mmcblk1/mmcblk1p1
```

```bash
root@AM62x:/# ll ./sys/class/mmc_host
total 0
lrwxrwxrwx 1 root root 0  8月  9 19:48 mmc0 -> ../../devices/platform/bus@f0000/fa10000.mmc/mmc_host/mmc0
lrwxrwxrwx 1 root root 0  8月  9 19:59 mmc1 -> ../../devices/platform/bus@f0000/fa00000.mmc/mmc_host/mmc1
lrwxrwxrwx 1 root root 0  8月  9 19:59 mmc2 -> ../../devices/platform/bus@f0000/fa20000.mmc/mmc_host/mmc2
```