# 测试命令
```bash
lsmod | grep rti
modprobe -r rti_wdt
modprobe rti_wdt heartbeat=2

devmem 0x43018170 32
devmem 0x43018170 32 0xFF

echo 1 > /dev/watchdog1

devmem 0x43018178 32
```

[\[FAQ\] AM64X/ AM62X : How to Reset the SOC when WDT timer expires in AM64X and AM62X? - Processors forum - Processors - TI E2E support forums](https://e2e.ti.com/support/processors-group/processors/f/processors-forum/1283237/faq-am64x-am62x-how-to-reset-the-soc-when-wdt-timer-expires-in-am64x-and-am62x)
[3.2.2.20. Watchdog — Processor SDK AM62x Documentation](https://software-dl.ti.com/processor-sdk-linux-rt/esd/AM62X/10_00_07_04/exports/docs/linux/Foundational_Components/Kernel/Kernel_Drivers/Watchdog.html)

## esm 中断
![[企业微信截图_17344876862150.png]]

## 复位源
![[企业微信截图_1734484723318.png]]

![[企业微信截图_17344847384486.png]]

![[企业微信截图_17344847457424.png]]


