# 编译
```bash
make -s -C examples/drivers/ipc/ipc_rpmsg_echo_linux/am62x-sk/m4fss0-0_freertos/ti-arm-clang/ clean
make -s -C examples/drivers/ipc/ipc_rpmsg_echo_linux/am62x-sk/m4fss0-0_freertos/ti-arm-clang/ all
cp -a examples/drivers/ipc/ipc_rpmsg_echo_linux/am62x-sk/m4fss0-0_freertos/ti-arm-clang/ipc_rpmsg_echo_linux.mcu-m4f0_0.release.strip.out /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/output/
sync


cp -a /run/media/mmcblk1p1/ipc_rpmsg_echo_linux.mcu-m4f0_0.release.strip.out /lib/firmware/pdk-ipc/ipc_echo_baremetal_test_mcu2_0_release_strip.xer5f
sync
```