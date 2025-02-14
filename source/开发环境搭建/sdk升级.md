# 万帮原理图
## 指示灯
### 运行状态
### 错误状态

## 启动模式
### 主MMCSD
### 主QSPI
### 主USB

## RTC
PCF8563T/5

## CAN

## 4G

## 音频


# 20250123

## 仓库
```bash


```

## 配置文件
```bash

{
    "soc_vars": {
        "sdk": [
            {
                "model": "M62xx-T@1.0.7+20250110hsse",
                "info": {
                     "in_used":        "y", // options: "y","n"
                     "uboot":          "u-boot-2021.01+gitAUTOINC+2ee8efd654-g2ee8efd654",
                     "kernel":         "linux-rt-5.10.168+gitAUTOINC+c1a1291911-gc1a1291911",
                     "atf":            "trusted-firmware-a-2.8+gitAUTOINC+2fcd408bb3",
                     "tee":            "optee_os-3.20.0",
                     "dtb":            "k3-am62x-sk.dtb",
                     "ab_partition":   "a", // options: "a","b","ab"
                     "filesystem":     "buildroot", // options: "buildroot", "yocto", "ubuntu", "debian"
                     "device_type":    "hs" // options: "gp","hs-fs","hs"
                }
            }
        ],
        "board": [
            {
                "model": "M62xx-T Rev.E",
                "info": {
                    "in_used":         "y", // options: "y","n"
                    "evm_model":       "M62xx-EV-Board Rev.C",
                    "client":          "demo"
                }
            }
        ],
        "oldi": [
            {
                "model": "LCD-1280800W101TC,ZY",
                "info": {
                    "in_used":          "y",
                    "peripheral_dtbo":  "k3-am62-oldi.dtbo",
                    "resolution":       "1280x800"
                }
            },
            {
                "model": "LI10600T101C2504,DWIN",
                "info": {
                    "in_used":          "n",
                    "peripheral_dtbo":  "k3-am62-oldi-lvds.dtbo",
                    "resolution":       "1024x600"
                }
            },
            {
                "model": "HW480272F-0B-0A,ZY",
                "info": {
                    "in_used":          "n",
                    "peripheral_dtbo":  "k3-am62-oldi-core-test.dtbo",
                    "resolution":       "480x272"
                }
            }
        ],
        "ethernet": [
            {
                "model": "YT8531SH,QFN-48,motorcomm",
                "info": {
                    "in_used":          "y",
                    "peripheral_dtbo":  "k3-am62-ethernet.dtbo"
                }
            },
            {
                "model": "ip179n",
                "info": {
                    "in_used":           "n",
                    "peripheral_dtbo":  "k3-am62-ethernet-switch.dtbo"
                }
            }
        ],
        "audio": [
            {
                "model": "TLV320AIC3101IRHBT,QFN-32,TI",
                "info": {
                    "in_used":          "y",
                    "peripheral_dtbo":  "k3-am62-audio.dtbo"
                }
            },
            {
                "model": "TLV320AIC3101IRHBT,QFN-32,TI",
                "info": {
                    "in_used":          "n",
                    "peripheral_dtbo":  "k3-am62-audio-master.dtbo"
                }
            },
            {
                "model": "TAS2780RYAR,TI",
                "info": {
                    "in_used":          "n",
                    "peripheral_dtbo":  "k3-am62-audio-master-tas2780.dtbo"
                }
            }
        ],
        "camera": [
            {
                "model": "NVP6188,TQFP-128,NEXTCHIP",
                "info": {
                    "in_used":          "y",
                    "peripheral_dtbo":  "k3-am62-csi-camera.dtbo"
                }
            }
        ],
        "mcan": [
            {
                "model": "CTM1051AMG,ZY",
                "info": {
                    "in_used":          "y",
                    "peripheral_dtbo":  "k3-am62-mcan.dtbo"
                }
            }
        ],
        "wifibt": [
            {
                "model": "AW-NM372SM,SMT,AzureWAVE",
                "info": {
                    "in_used":          "n",
                    "peripheral_dtbo":  "k3-am62-wifibt.dtbo"
                }
            }
        ],
        "bus_spi": [
            {
                "model": "AP-283",
                "info": {
                    "in_used":          "n",
                    "peripheral_dtbo":  "k3-am62-spi-ap283.dtbo"
                }
            }
        ],
        "factory_core": [
            {
                "model": "FACTORY,ZY",
                "info": {
                    "in_used":            "n",
                    "peripheral_dtbo":    "k3-am62-factory-core.dtbo"
                }
            }
        ],
        "factory_evm": [
            {
                "model": "FACTORY,ZY",
                "info": {
                    "in_used":           "n",
                    "peripheral_dtbo":   "k3-am62-factory-evm.dtbo"
                }
            }
        ],
        "production": [
            {
                "model": "PRODUCT,ZY",
                "info": {
                    "in_used":           "n",
                    "peripheral_dtbo":   "k3-am62-production.dtbo"
                }
            }
        ],
        "sdram": [
            {
                "model": "H5AN8G6NCJR-XNI,FBGA-96,HYNIX",
                "info": {
                    "in_used":    "y",
                    "density":    "1GB"
                }
            },
            {
                "model": "H5ANAGG6NCJR-XNI,FBGA-96,HYNIX",
                "info": {
                    "in_used":    "n",
                    "density":    "2GB"
                }
            }
        ],
        "mmcsd": [
            {
                "model": "S40FC004C1B1I00000,BGA-153,SkyHigh",
                "info": {
                    "in_used":    "y",
                    "density":    "4GB",
                    "dtsi":       "k3-am62-mmcsd.dtsi"
                }
            }
        ],
        "usb": [
            {
                "model": "USB,TI",
                "info": {
                    "in_used":    "y",
                    "dtsi":       "k3-am62-usb.dtsi"
                }
            }
        ],
        "ospi": [
            {
                "model": "IS25LP064D-JKLE,WSON-8,ISSI",
                "info": {
                    "in_used":    "y",
                    "density":    "8MB",
                    "dtsi":       "k3-am62-ospi.dtsi"
                }
            }
        ],
        "uart": [
            {
                "model": "UART,TI",
                "info": {
                    "in_used":    "y",
                    "dtsi":       "k3-am62-uart.dtsi"
                }
            }
        ],
        "i2c": [
            {
                "model": "I2C,TI",
                "info": {
                    "in_used":    "y",
                    "dtsi":       "k3-am62-i2c.dtsi"
                }
            }
        ],
        "spi": [
            {
                "model": "SPI,TI",
                "info": {
                    "in_used":    "y",
                    "dtsi":       "k3-am62-spi.dtsi"
                }
            }
        ],
        "ecap_capture": [
            {
                "model": "ECAP-CAPTURE,TI",
                "info": {
                    "in_used":    "y",
                    "dtsi":       "k3-am62-ecap-capture.dtsi"
                }
            }
        ],
        "ecap_pwm": [
            {
                "model": "ECAP-PWM,TI",
                "info": {
                    "in_used":    "y",
                    "dtsi":       "k3-am62-ecap-pwm.dtsi"
                }
            }
        ],
        "epwm": [
            {
                "model": "EPWM,TI",
                "info": {
                    "in_used":    "y",
                    "dtsi":       "k3-am62-epwm.dtsi"
                }
            }
        ],
        "misc": [
            {
                "model": "MISC,ZY",
                "info": {
                    "in_used":    "y",
                    "dtsi":       "k3-am62-misc.dtsi"
                }
            }
        ],
        "set": [
            {
                "model": "dtsi",
                "info": {
                    "in_used": "y",
                    "dtsi1": "dtsi/$mmcsd_dtsi",
                    "dtsi2": "dtsi/$usb_dtsi",
                    "dtsi3": "dtsi/$ospi_dtsi",
                    "dtsi4": "dtsi/$uart_dtsi",
                    "dtsi5": "dtsi/$i2c_dtsi",
                    "dtsi6": "dtsi/$spi_dtsi",
                    "dtsi7": "dtsi/$ecap_capture_dtsi",
                    "dtsi8": "dtsi/$ecap_pwm_dtsi",
                    "dtsi9": "dtsi/$epwm_dtsi",
                    "dtsi10": "dtsi/$misc_dtsi"
                }
            }
        ],
        "dtsi": [
            {
                "model": "zy",
                "info": {
                    "in_used": "y",
                    "include": "$set_dtsi1 $set_dtsi2 $set_dtsi3 $set_dtsi4 $set_dtsi5 $set_dtsi6 $set_dtsi7 $set_dtsi8 $set_dtsi9 $set_dtsi10"
                }
            }
        ]
    },
    "combo_vars": {
        "its_input": {
            "kernel_0":  "Image.gz",
            "ramdisk_0": "rootfs.cpio.gz",
            "fdt_0":     "$sdk_dtb"
        },
        "sec_its_input": {
            "kernel_0":  "linux.bin.sec",
            "ramdisk_0": "ramdisk.sec",
            "fdt_0":     "${sdk_dtb}.sec"
        }
    },
    "bool_vars": {
        "build_atf":                         {"value": "y"}, // options: "y","n"
        "build_tee":                         {"value": "y"},
        "build_dm":                          {"value": "y"},
        "build_uboot":                       {"value": "y"},
        "build_patch":                       {"value": "y"},
        "build_kernel":                      {"value": "y"},
        "build_dts":                         {"value": "y"},
        "build_gpu":                         {"value": "y"},
        "build_fit":                         {"value": "y"},
        "build_boot":                        {"value": "y"},
        "build_opt":                         {"value": "y"},
        "build_pack":                        {"value": "y"},
        "build_docker":                      {"value": "n"},
        "build_usb_msc":                     {"value": "n"},
        "build_usb_dfu": {
                                              "value": "n",
            "build_usb_dfu_qspi":            {"value": "y"},
            "build_usb_dfu_emmc":            {"value": "n"},
            "build_usb_dfu_sdcard":          {"value": "n"}
        },
        "build_fastboot":                    {"value": "n"},
        "build_third_party_cert":            {"value": "n"}
    },
    "misc_vars": {
        "info_cross_compile": {
            "value": "$ti_toolchain_usr_bin_dir/aarch64-none-linux-gnu-"
        },
        "info_external_cross_compile_aarch64": {
            "value": "$ti_toolchain_aarch64_bin_dir/aarch64-none-linux-gnu-"
        },
        "info_external_cross_compile_aarch32": {
            "value": "$ti_toolchain_aarch32_bin/arm-none-linux-gnueabihf-"
        },
        "info_release_archive_dir": {
            "value": "/Volumes/ti/client/$board_client"
        },
        "info_opt_dir": {
            "value": "/opt"
        },
        "info_tslib_install_dir": {
            "value": "$info_opt_dir/tslib"
        },
        "info_patch_file": {
            "value": "$ti_build_script_dir/patching"
        },
        "info_sd_boot_dir": {
            "value": "/Volumes/boot"
        },
        "info_sd_roofs_dir": {
            "value": "/Volumes/rootfs"
        },
        "info_update_file": {
            "value": "update-img.txt"
        },
        "info_img_split_size": {
            "value": "980M"
        },
        "ota_hash_file": {
            "value": "hash.txt"
        },
        "rootfs_crop_level": {
            "value": "level_1" // options: "level_1". "level_2", "level_3"
        },
        "remove_packages_file": {
            "value": "remove_packages_file"
        },
        "fastboot_type": {
            "value": "backup_emmc_uda_fs" // options: "emmc_uda_raw", "emmc_uda_fs", "backup_emmc_uda_fs", "sd", "qspi", "usbdfu"
        },
        "fastboot_uda_raw_boot_a_sector": {
            "value": "1" // 1MB
        },
        "fastboot_uda_raw_boot_fit_max_size": {
            "value": "48" // 48MB
        }
    },
    "code_tree_vars": {
        "$root": {
            "build": {
                "alias": "ti_build_script",
                "compile.sh": {
                    "type": "file", // options: "file"
                    "alias": "compile"
                },
                "env.py": {
                    "type": "file"
                },
                "env.json": {
                    "type": "file",
                    "alias": "env_json"
                },
                "env.sh": {
                    "type": "file",
                    "alias": "env_script"
                },
                "00_common.sh": {
                    "type": "file",
                    "alias": "common_script"   
                },
                "51_rootfs_ubuntu.sh": {
                    "type": "file",
                    "alias": "ubuntu_build_script"   
                },
                "52_roofs_initramfs.sh": {
                    "type": "file",
                    "alias": "initramfs_build_script"   
                },
                "53_roofs_buildroot.sh": {
                    "type": "file",
                    "alias": "buildroot_build_script"   
                },
                "54_roofs_crop.sh": {
                    "type": "file",
                    "alias": "rootfs_crop_script"
                },
                "gen-img": {
                    "itb": {
                        "input": {
                            "alias": "itb_input",
                            "rootfs": {
                                "alias": "initramfs_rootfs",
                                "scripts": {
                                    "alias": "initramfs_rootfs_scripts",
                                    "fastboot_env.sh": {
                                        "type": "file",
                                        "alias": "initramfs_fastboot_env"
                                    }
                                }
                            },
                            "rootfs.cpio.gz": {
                                "type": "file",
                                "alias": "initramfs_rootfs_tar"
                            }
                        },
                        "output": {
                            "fitImage": {
                                "type": "file",
                                "alias": "itb_fitimage"
                            }
                        },
                        "am62xx-kernel-image.its": {
                            "type": "file",
                            "alias": "itb"
                        },
                        "fitImage-sec.its": {
                            "type": "file",
                            "alias": "itb_sec"
                        },
                        "am62xx-kernel-image-template.its": {
                            "type": "file",
                            "alias": "itb_template"
                        },
                        "fitImage-sec-template.its": {
                            "type": "file",
                            "alias": "itb_sec_template"
                        }
                    }
                }
            },
            "output": {
                "alias": "ti_linux_output",
                "tiboot3.bin": {
                    "type": "file",
                    "alias": "r5_spl"
                },
                "tispl.bin": {
                    "type": "file",
                    "alias": "a53_spl"
                },
                "u-boot.img": {
                    "type": "file",
                    "alias": "a53_uboot"
                },
                "uEnv.txt": {
                    "type": "file",
                    "alias": "uboot_uenv"
                },
                "atf-tee-dm-kernel-fdt.bin": {
                    "type": "file",
                    "alias": "fastboot"
                },
                "dtbo": {
                    "alias": "ti_linux_output_dtbo",
                    "dtbos": {
                        "type": "file",
                        "alias": "loaded_dtbo_file_list"
                    }
                },
                "boot": {
                    "alias": "boot_root",
                    "boot": {
                        "alias": "boot_install",
                        "tiboot3.bin": {
                            "type": "file",
                            "alias": "boot_r5_spl"
                        },
                        "tispl.bin": {
                            "type": "file",
                            "alias": "boot_a53_spl"
                        },
                        "u-boot.img": {
                            "type": "file",
                            "alias": "boot_a53_uboot"
                        },
                        "run": {
                            "alias": "boot_run_install"
                        }
                    }
                },
                "usbmsc": {
                    "alias": "ti_output_usbmsc"
                }, 
                "DFU_flash": {
                    "alias": "dfu_flash_script",
                    "dfu_cmd.sh": {
                        "type": "file",
                        "alias": "dfu_cmd_script"
                    },
                    "am62xx-evm": {
                        "alias": "dfu_am62xx_evm",
                        "uEnv_dfu_ospi-nor.txt": {
                            "type": "file",
                            "alias": "uenv_dfu_ospi"
                        },
                        "uEnv_dfu_emmc.txt": {
                            "type": "file",
                            "alias": "uenv_dfu_emmc"
                        },
                        "uEnv_dfu_sdcard.txt": {
                            "type": "file",
                            "alias": "uenv_dfu_sdcard"
                        }
                    },
                    "img": {
                        "alias": "dfu_img",
                        "loader": {
                            "alias": "dfu_loader_img",
                            "uEnv.txt": {
                                "type": "file",
                                "alias": "dfu_loader_uenv"
                            }
                        },
                        "raw": {
                            "alias": "dfu_raw_img"
                        }
                    }
                },
                "logo.png": {
                    "type": "file",
                    "alias": "weston_bgd"
                },
                "backup.tar": {
                    "type": "file",
                    "alias": "backup_tar"
                },
                "boot.tar": {
                    "type": "file",
                    "alias": "boot_tar"
                },
                "boot.img": {
                    "type": "file",
                    "alias": "boot_img"
                },
                "mac.txt": {
                    "type": "file",
                    "alias": "eth_mac_addr"
                }
            },
            "bin": {
                "alias": "ti_bin",
                "DFU_flash": {
                    "alias": "bin_dfu_flash_script"
                }
            },
            "external-toolchain": {
                "alias": "ti_external_toolchain",
                "gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu": {
                    "bin": {
                        "alias": "ti_toolchain_aarch64_bin"
                    }
                },
                "gcc-arm-9.2-2019.12-x86_64-arm-none-linux-gnueabihf": {
                    "bin": {
                        "alias": "ti_toolchain_aarch32_bin"
                    }
                }
            },
            "linux-devkit": {
                "alias": "ti_toolchain",
                "sysroots": {
                    "x86_64-arago-linux": {
                        "usr": {
                            "bin": {
                                "alias": "ti_toolchain_usr_bin"
                            }
                        }
                    }
                }
            },
            "board-support": {
                "prebuilt-images": {
                    "alias": "ti_prebuilt_img",
                    "ipc_echo_testb_mcu1_0_release_strip.xer5f": {
                        "type": "file",
                        "alias": "ti_r5_ipc_fw"
                    },
                    "ipc_echo_baremetal_test_mcu2_0_release_strip.xer5f": {
                        "type": "file",
                        "alias": "ti_m4_ipc_fw"
                    },
                    "uEnv.txt": {
                        "type": "file",
                        "alias": "uenv"
                    },
                    "wificfg": {
                        "type": "file",
                        "alias": "wificfg"
                    },
                    "zy_uboot.tar.xz": {
                        "type": "file",
                        "alias": "zy_prebuilt_uboot_img"
                    }
                },
                "k3-image-gen-2022.01": {
                    "alias": "k3_img_gen",
                    "tiboot3-am62x-${sdk_device_type}-evm.bin": {
                        "type": "file",
                        "alias": "r5_spl_img"
                    }
                },
                "u-boot-2021.01+gitAUTOINC+2ee8efd654-g2ee8efd654": {
                    "alias": "ti_uboot",
                    "configs": {
                        "alias": "ti_uboot_configs",
                        "am62x_evm_a53_defconfig":{
                            "type": "file",
                            "alias": "ti_uboot_a53_config"
                        },
                        "am62x_evm_r5_defconfig":{
                            "type": "file",
                            "alias": "ti_uboot_r5_config"
                        },
                        "am62x_evm_a53_default_config":{
                            "type": "file",
                            "alias": "ti_uboot_a53_default_config"
                        },
                        "am62x_evm_r5_default_config":{
                            "type": "file",
                            "alias": "ti_uboot_r5_default_config"
                        },
                        "am62x_evm_r5_fastboot_default_config":{
                            "type": "file",
                            "alias": "ti_uboot_r5_fastboot_default_config"
                        },
                        "am62x_evm_a53_usbdfu_defconfig":{
                            "type": "file",
                            "alias": "ti_uboot_a53_usbdfu_config"
                        },
                        "am62x_evm_r5_usbdfu_defconfig":{
                            "type": "file",
                            "alias": "ti_uboot_r5_usbdfu_config"
                        },
                        "am62x_evm_r5_usbdfu_fastboot_defconfig":{
                            "type": "file",
                            "alias": "ti_uboot_r5_usbdfu_fastboot_config"
                        },
                        "am62x_evm_a53_usbmsc_defconfig":{
                            "type": "file",
                            "alias": "ti_uboot_a53_usbmsc_config"
                        },
                        "am62x_evm_r5_usbmsc_defconfig":{
                            "type": "file",
                            "alias": "ti_uboot_r5_usbmsc_config"
                        }
                    }
                },
                "linux-rt-5.10.168+gitAUTOINC+c1a1291911-gc1a1291911": {
                    "alias": "ti_linux",
                    "tools": {
                        "spi":{
                            "alias": "ti_linux_spi_tool"
                        },
                        "testing":{
                            "alias": "ti_linux_testing",
                            "selftests":{
                                "alias": "ti_linux_selftests",
                                "ptp":{
                                    "alias": "ti_linux_testptp",
                                    "testptp":{
                                        "type": "file",
                                        "alias": "ti_linux_selftests_testptp"
                                    }
                                }
                            }
                        }
                    },
                    "fitImage": {
                        "type": "file",
                        "alias": "ti_fitimage"
                    },
                    "arch": {
                        "arm64": {
                            "boot": {
                                "Image.gz": {
                                    "type": "file",
                                    "alias": "ti_image"
                                },
                                "dts": {
                                    "ti": {
                                        "alias": "ti_dts",
                                        "k3-am62x.h": {
                                            "type": "file",
                                            "alias": "dtsi_include"
                                        },
                                        "dtsi": {
                                            "alias": "ti_dtsi"
                                        },
                                        "dtso": {
                                            "alias": "ti_dtso"
                                        }
                                    }
                                }
                            }
                        }
                    },
                    "drivers": {
                        "alias": "ti_linux_drivers",
                        "zy": {
                            "alias": "ti_linux_drivers_zy",
                            "media": {
                                "alias": "ti_linux_drivers_zy_media",
                                "i2c": {
                                    "alias": "ti_linux_drivers_zy_i2c",
                                    "nvp6188.ko": {
                                        "type": "file",
                                        "alias": "ti_linux_drivers_camera"
                                    }
                                }
                            }
                        }
                    }
                },
                "u-boot_build": {
                    "alias": "ti_uboot_build_output",
                    "a53": {
                        "alias": "uboot_a53_img",
                        "tispl.bin": {
                            "type": "file",
                            "alias": "a53_spl_img"
                        },
                        "tispl.bin_HS": {
                            "type": "file",
                            "alias": "a53_spl_hs_img"
                        },
                        "u-boot.img": {
                            "type": "file",
                            "alias": "a53_uboot_img"
                        },
                        "u-boot.img_HS": {
                            "type": "file",
                            "alias": "a53_uboot_hs_img"
                        }
                    },
                    "r5": {
                        "alias": "uboot_r5_img"
                    }
                }
            },
            "targetNFS": {
                "alias": "ti_rootfs",
                "boot": {
                    "alias": "ti_rootfs_boot"
                },
                "opt": {
                    "alias": "ti_rootfs_opt"
                }
            }
        }
    }
}
```

## uboot
- TF卡启动、qspi启动已经适配完成；
- usb msc启动，已经能正常启动到文件系统
	- 遗留问题：
		- 从usb msc启动升级系统后，将系统切换为qspi启动，无法启动
			- 原因：在升级的过程中，写入的是usb msc的uboot，故无法正常启动，需要写入非usb msc专用的uboot
![[Pasted image 20250208170727.png]]
		- u盘要加延时才初始化完成
![[Pasted image 20250208170659.png]]
- 剩余usb dfu没适配
- 下面的不能加
![[Pasted image 20250211113120.png]]

- 安全启动
	- 固件校验如果有问题，则触发软复位（这里先不合入）
![[Pasted image 20250210153100.png]]

- 以下文件已完成适配
```bash
arch/arm/mach-k3/common.c
arch/arm/mach-k3/security.c
cmd/ti/Kconfig
cmd/ti/Makefile
cmd/ti/tisci.c
drivers/firmware/ti_sci.c
drivers/firmware/ti_sci.h
include/linux/soc/ti/ti_sci_protocol.h
tools/binman/btool/openssl.py
```

- 以太网适配完成
![[Pasted image 20250210175952.png]]


- 文件系统
	- 以下文件已完成适配
```bash
cmd/fs.c
fs/fs.c
fs/ext4/ext4_common.c
```

- “zy”启动停止字符串
	- 以下文件已完成适配
```bash
common/autoboot.c
include/asm-generic/global_data.h
```

- 看门狗
	- 以下文件已完成适配
```bash
boot/bootm.c
```

- 设备树
	- 以下文件已完成适配
```bash
boot/image-fdt.c
```

- mmcsd
	- 以下文件已完成适配
```bash
drivers/mmc/am654_sdhci.c
drivers/mmc/mmc.c
```

- 环境变量
	- 环境变量保存到 emmc 的分区1中，名字为uboot.env
```bash
root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/src/zy/ti-processor-sdk-linux-rt-am62xx-evm-10.01.10.04/board-support/ti-u-boot-2024.04+git# git diff
diff --git a/configs/am62x_evm_a53_default_config b/configs/am62x_evm_a53_default_config
index 7cf5708..5fac4e0 100755
--- a/configs/am62x_evm_a53_default_config
+++ b/configs/am62x_evm_a53_default_config
@@ -58,8 +58,11 @@ CONFIG_SYS_SPL_MALLOC=y
 CONFIG_SPL_DMA=y
 CONFIG_SPL_ENV_SUPPORT=y
 CONFIG_ENV_SIZE=0x1f000
-CONFIG_ENV_OFFSET=0x680000
-CONFIG_ENV_IS_IN_MMC=y
+CONFIG_ENV_IS_IN_FAT=y
+CONFIG_SPL_ENV_IS_IN_FAT=y
+CONFIG_ENV_FAT_INTERFACE="mmc"
+CONFIG_ENV_FAT_DEVICE_AND_PART="0:1"
+CONFIG_ENV_FAT_FILE="uboot.env"
 CONFIG_SYS_MMC_ENV_PART=2
 CONFIG_SPL_ETH=y
 CONFIG_SPL_FS_LOAD_PAYLOAD_NAME="u-boot.img"
```
```bash
U-Boot 2024.04-dirty (Feb 11 2025 - 15:55:23 +0800)

SoC:   AM62X SR1.0 HS-SE
Model: ZHIYUAN Electronics AM625
DRAM:  1 GiB
Core:  94 devices, 32 uclasses, devicetree: separate
WDT:   Started hw_watchdog with servicing every 250ms (2s timeout)
MMC:   mmc@fa10000: 0, mmc@fa00000: 1
Loading Environment from FAT... Unable to read "uboot.env" from mmc0:1...


=> saveenv
Saving Environment to FAT... OK
=> ls mmc 0:1
            boot/
   126976   uboot.env

1 file(s), 1 dir(s)


root@AM62x:~# ll /run/media/mmcblk0p1
total 126
drwxr-xr-x 2 root root   2048 10月 28 04:13 boot
-rwxr-xr-x 1 root root 126976  1月  1  1980 uboot.env
```

- cfg
	- 剩余fastboot的cfg没适配
## linux
- spi nor 提高频率失败，无法识别到ID
![[Pasted image 20250124113123.png]]
![[Pasted image 20250124113058.png]]
![[Pasted image 20250124113108.png]]


- 音频
	- 目前不行，有可能是下面使能的原因
![[Pasted image 20250211134530.png]]

- 摄像头
	- 以下代码待评审
![[Pasted image 20250213135639.png]]
![[Pasted image 20250213135701.png]]

- 4G模块
	- 以下内容待确认，看是不是移远的修改
![[Pasted image 20250214151301.png]]

![[Pasted image 20250214152106.png]]


## atf
- 已全部适配完成
![[Pasted image 20250124103358.png]]
## optee
- 已全部适配完成
![[Pasted image 20250123175455.png]]

## ti
- 已全部适配完成
![[Pasted image 20250123172346.png]]
## opensrc
- 已全部适配完成
![[Pasted image 20250123172401.png]]
## app

除了下面的内容，其余全部适配完成

- 编译选项
![[Pasted image 20250123165205.png]]

- 蓝牙测试项
![[Pasted image 20250123163902.png]]

- 蓝牙波特率
![[Pasted image 20250123165629.png]]

- 摄像头不适配
![[Pasted image 20250123164010.png]]

- awtk
-工具栏没有drm库，
![[Pasted image 20250211180632.png]]
[解决方案在线文档](https://manual.zlg.cn/web/#/235/10450)
- 改了下面的地方
![[Pasted image 20250211180712.png]]

## 文件系统
- 编译脚本已经全部完成适配
![[Pasted image 20250123163453.png]]

- 只有buildroot和ramdisk适配完成，其他的都没适配
![[Pasted image 20250124105743.png]]

- cfg报错
![[Pasted image 20250124135715.png]]

加了下面代码后，没有报错，这里待分析
![[Pasted image 20250124135804.png]]



# 问题
#  工具链的c库版本不匹配
```bash

root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04# ll /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc
-rwxr-xr-x 1 root root 1710848 Sep  3 11:35 /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc*


root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04# /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc --version
bash: /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc: No such file or directory
```
![[Pasted image 20240912193841.png]]

## 排查
```bash
root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04# ldd /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc
/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.38' not found (required by /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc)
/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.36' not found (required by /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc)
        linux-vdso.so.1 (0x00007ffdb1362000)
        libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f0cf7042000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f0cf6e19000)
        /home/liweiyu/am62x/src/ti/linux-devkit/sysroots/x86_64-arago-linux/lib/ld-linux-x86-64.so.2 => /lib64/ld-linux-x86-64.so.2 (0x00007f0cf7131000)

root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04# file /home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc
/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/linux-devkit/sysroots/x86_64-arago-linux/usr/bin/aarch64-oe-linux/aarch64-oe-linux-gcc: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), dynamically linked, interpreter /home/liweiyu/am62x/src/ti/linux-devkit/sysroots/x86_64-arago-linux/lib/ld-linux-x86-64.so.2, BuildID[sha1]=defbf7c0db4f5a73c2e73e8241e402a725501149, for GNU/Linux 3.2.0, stripped
```

![[Pasted image 20240912194020.png]]

c库版本
```bash
root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04# ldd --version
ldd (Ubuntu GLIBC 2.35-0ubuntu3.8) 2.35
Copyright (C) 2022 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
Written by Roland McGrath and Ulrich Drepper.
```
![[Pasted image 20240912194059.png]]

根据您提供的信息，问题的核心在于您的系统上的 GLIBC 版本与 aarch64-oe-linux-gcc 所需的版本不匹配。具体来说：
1.您的系统当前使用的是 GLIBC 2.35 版本。
2.aarch64-oe-linux-gcc 需要 GLIBC 2.36 和 GLIBC 2.38 版本。
![[Pasted image 20240912195054.png]]

根本原因是：

![[Pasted image 20240912195111.png]]


要自动执行解压 `ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin` 文件的操作，你可以使用以下方法：

1. 首先，确保该文件有执行权限：

   ```bash
   chmod +x ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin
   ```

2. 然后，你可以使用以下命令来自动执行解压操作：

   ```bash
   ./ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin --mode unattended --prefix /path/to/install/directory
   ```

   这里的参数解释如下：
   - `--mode unattended`: 这个选项告诉安装程序以无人值守模式运行，不需要用户交互。
   - `--prefix /path/to/install/directory`: 指定安装目录。将 `/path/to/install/directory` 替换为你想要安装 SDK 的实际路径。

3. 如果你想要更多的控制或自定义选项，你可能需要创建一个配置文件。例如，创建一个名为 `silent.cfg` 的文件，内容如下：

   ```
   # Silent configuration file
   ACCEPT_EULA=1
   INSTALL_DIR=/path/to/install/directory
   INSTALL_COMPONENTS=all
   ```

   然后使用这个配置文件运行安装程序：

   ```bash
   ./ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin --mode silent --silentconfig silent.cfg
   ```

4. 如果你只是想解压文件而不安装，你可以尝试以下命令：

   ```bash
   ./ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin --target /path/to/extract/directory
   ```

   这将把文件的内容解压到指定的目录，而不进行实际的安装。

请注意，具体的选项和参数可能会因 TI 的不同版本而略有变化。如果这些方法不起作用，你可能需要查看 TI 提供的具体文档，或者运行 `./ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin --help` 来查看可用的选项。


```bash
root@DESKTOP-GC4LAR7:/home/liweiyu/tmp2# ./ti-processor-sdk-linux-rt-am62xx-evm-09.02.01.10-Linux-x86-Install.bin --help
ti-processor-sdk-linux-rt-am62xx-evm-09.02.01.10 09.02.01.10
Usage:

 --help                                      Display the list of valid options

 --version                                   Display product information

 --unattendedmodeui <unattendedmodeui>       Unattended Mode UI
                                             Default: none
                                             Allowed: none minimal minimalWithDialogs

 --optionfile <optionfile>                   Installation option file
                                             Default:

 --debuglevel <debuglevel>                   Debug information level of verbosity
                                             Default: 2
                                             Allowed: 0 1 2 3 4

 --mode <mode>                               Installation mode
                                             Default: xwindow
                                             Allowed: gtk xwindow text unattended

 --debugtrace <debugtrace>                   Debug filename
                                             Default:

 --installer-language <installer-language>   Language selection
                                             Default: en
                                             Allowed: sq ar es_AR az eu pt_BR bg ca hr cs da nl en et fi fr de el he hu id it ja kk ko lv lt no fa pl pt ro ru sr zh_CN sk sl es sv th zh_TW tr tk va vi cy

 --prefix <prefix>                           Destination Folder
                                             Default: /opt/ti-processor-sdk-linux-rt-am62xx-evm-09.02.01.10

```

该 `ti-processor-sdk-linux-rt-am62xx-evm-09.02.01.10-Linux-x86-Install.bin 安装程序的帮助信息提供了对其使用的详细指导，包括多种命令行选项，这些选项允许用户以不同的方式定制安装过程。以下是对每个选项的详细解释：

 基本选项
- `--help`: 显示有效选项的列表，可以用来快速获取可用命令的概览。
---version`: 显示产品信息，包括版本号和其他可能的详细信息，有助于确认安装程序的具体版本### 安装模式
- `--mode <mode>`: 指定安装模式。允许的值包括：
  - `gtk`: 使用 GTK 图形界面进行安装（如果可用）。
  - `xwindow`: 使用基于X窗口系统的图形界面。
  - `text`: 使用纯文本模式，不需要图形界面。
  - `unattended`: 无人值守模式，不会有任何用户交互，通常用于自动化安装脚本。
  
### 安装配置
- `--prefix <prefix>`: 指定安装目录。默认为 `/opt/ti-processor-sdk-linux-rt-am62xx-evm-09.02.01.10`，你可以更改此路径以安装到不同的位置。

### 用户界面相关
- `--unattendedmodeui <unattendedmodeui>`: 在无人值守模式下设置用户界面的类型。选项包括：
  - `none`: 无任何界面。
  - `minimal`: 最小界面。
  - `minimalWithDialogs`: 提供最小的对话界面。
- `--installer-language <installer-language>`: 设置安装程序的语言。支持多种语言，如英语(en)、汉语简体(zh_CN)、汉语繁体(zh_TW)等。

### 调试和日志
- `--debuglevel <debuglevel>`: 设置调试信息的详细级别，从 0（最少）到 4（最详细）。
- `--debugtrace <debugtrace>`: 指定一个文件名，用于存储调试信息和跟踪日志，有助于在出现问题时进行故障排除。

### 选项文件
- `--optionfile <optionfile>`: 指定一个包含安装选项的文件，该文件可以预先定义上述各种选项，以简化命令行输入。

通过结合这些选项，用户可以更精准地控制安装流程，适应不同的部署环境和需求。例如，如果您正在自动化部署流程，可能会选择使用无人值守模式(`--mode unattended`)与最小或无界面(`--unattendedmodeui none`)，并指定一个非默认的安装路径(`--prefix`)以及必要的语言环境(`--installer-language`)。



生成一个自解压的可执行安装文件（如 `-processor-sdk-linux-rt-am62xx-evm-10.00.07.04-Linux-x86-Install.bin`）通常需要几个步骤，并且涉及到使用打包和脚本工具。下面是一个简化的过程，展示如何从头开始创建这类文件：

[Linux .bin安装文件制作 - iTech - 博客园](https://www.cnblogs.com/itech/archive/2010/02/01/1661248.html)
[Linux下创建可执行bin安装文件 - Cocowool - 博客园](https://www.cnblogs.com/cocowool/p/7733089.html)
[linux下解压bin文件 - 白菜不是菜](https://iminto.github.io/post/linux%E4%B8%8B%E8%A7%A3%E5%8E%8Bbin%E6%96%87%E4%BB%B6/)

### 步骤 1: 准备文件和目录结构

首先，你需要准备所有要包含在安装包中的文件和目录。这可能包二进制文件、库文件、文档、配置文件等。将这些文件组到一个清晰的目录结构中，这将是你安装程序解压后的结构。

例如：
```
/my-sdk/
├── bin/
│   └── some-binary
├── lib/
│   └── some-library.so
├── share/
│   └── docs/
└── install-script.sh
```

### 步骤 2: 创建安装脚本

创建一个安装脚（例如 `install-script.sh`），这个脚本将会在用户执行自解压文件时运行。脚本中应包括复制文件、设置环境变量、检查依赖等步骤。示例脚本如下：

```bash
#!/bin/bashecho "Installing My SDK..."
cp -r /tmp/my-sdk/* /opt/my-sdk/
echo "Installation Complete."
```

```bash
# sdk-install.sh
#!/bin/sh

# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
# THE SOFTWARE.

# Process command line...
while [ $# -gt 0 ]; do
  case $1 in
    --install_dir)
      shift;
      install_dir=$1;
      ;;
     *)
      shift;
      ;;
  esac
done

if [ "x$install_dir" = "x" ]; then
    # Verify that the script is being called within untar SDK directory
    if [ ! -d "$PWD/board-support" -a ! -f "$PWD/Rules.make" ]; then
        echo "Script must be called within untarred sdk directory"
        exit 1
    else
        install_dir="$PWD"
    fi
fi

# Get full path to SDK directory
cd "$install_dir"
install_dir="$PWD"
cd -

if [ ! -d $install_dir ]; then
        echo "Installation directory does not exist"
        exit 1
fi

# Remove any .svn directories that were packaged by the sourceipks
rm -rf `find $install_dir -name ".svn"`


# Update Rules.make variables
sed -i -e s=__SDK__INSTALL_DIR__=$install_dir= $install_dir/Rules.make

# Find the linux directory name
linux=`find $install_dir/board-support -maxdepth 1 -name "linux*"`
linux=`basename $linux`
sed -i -e s=__KERNEL_NAME__=$linux= $install_dir/Rules.make

threads=`cat /proc/cpuinfo | grep -c processor`

# Create a newline
echo >> $install_dir/Rules.make
# Set optimal value for the make file's -j option
echo "MAKE_JOBS=$threads" >> $install_dir/Rules.make

cd -


# Install toolchain sdk
$install_dir/linux-devkit.sh -y -d $install_dir/linux-devkit
if [ -f $install_dir/k3r5-devkit.sh ]; then
    $install_dir/k3r5-devkit.sh -y -d $install_dir/k3r5-devkit
fi

# Remove toolchain sdk
rm $install_dir/linux-devkit.sh
if [ -f $install_dir/k3r5-devkit.sh ]; then
    rm $install_dir/k3r5-devkit.sh
fi


# Update example applications CCS project files

# Grab some essential variables from environment-setup
REAL_MULTIMACH_TARGET_SYS=`sed -n 's/^export REAL_MULTIMACH_TARGET_SYS=//p' $install_dir/linux-devkit/environment-setup`
TOOLCHAIN_SYS=`sed -n 's/^export TOOLCHAIN_SYS=//p' $install_dir/linux-devkit/environment-setup`
SDK_SYS=`sed -n 's/^export SDK_SYS=//p' $install_dir/linux-devkit/environment-setup`

# Grab toolchain's GCC version
GCC_VERSION=`ls $install_dir/linux-devkit/sysroots/$SDK_SYS/usr/lib/gcc/$TOOLCHAIN_SYS/`

TOOLCHAIN_TARGET_INCLUDE_DIR="linux-devkit/sysroots/$REAL_MULTIMACH_TARGET_SYS/usr/include"
TOOLCHAIN_INCLUDE_DIR="linux-devkit/sysroots/$SDK_SYS/usr/lib/gcc/$TOOLCHAIN_SYS/$GCC_VERSION/include"

SDK_PATH_TARGET="linux-devkit/sysroots/$REAL_MULTIMACH_TARGET_SYS/"

sed -i -e s=__SDK_PATH_TARGET__=$SDK_PATH_TARGET= $install_dir/Rules.make

if [ -f "$install_dir/bin/unshallow-repositories.sh" ]
then
    sed -i -e s=__SDK_INSTALL_DIR__=$install_dir= $install_dir/bin/unshallow-repositories.sh
fi

if [ -f "$install_dir/bin/create-ubifs.sh" ]
then
    sed -i -e s=__SDK_INSTALL_DIR__=$install_dir= $install_dir/bin/create-ubifs.sh
fi

# Modify create-sdcard.sh to have user-supplied installation directory
if [ -f "${install_dir}/bin/create-sdcard.sh" ]
then
    sed -i -e "s|ti-sdk.*\[0-9\]|${install_dir##*/}|g" ${install_dir}/bin/create-sdcard.sh
fi

# Update CCS project files using important paths to headers

find $install_dir/example-applications -type f -exec sed -i "s|<TOOLCHAIN_TARGET_INCLUDE_DIR>|$TOOLCHAIN_TARGET_INCLUDE_DIR|g" {} \;
find $install_dir/example-applications -type f -exec sed -i "s|<TOOLCHAIN_INCLUDE_DIR>|$TOOLCHAIN_INCLUDE_DIR|g" {} \;


if [ -f "$install_dir/sdk-install.sh" ]; then
        # File is only able to run once so delete it from the SDK
        rm "$install_dir/sdk-install.sh"
fi

echo "Installation completed!"
exit 0

```

![[Pasted image 20240913104515.png]]

### 步骤 3: 创建自解压脚本

使用 `makeself` 或 `shar` 等工具来创建自解压脚。这些工可以将目录打包成一个自解压的 shell 脚本。`makeself` 是一个流行的选择，它允许你创建一个包含完整目录的压缩自解压档案。

[用Makeself工具打包镜像 | Escape](https://www.escapelife.site/posts/766c5534.html)


首先，安装 `makeself`：

```bash
sudo apt-get install makeself  # Debian/Ubuntu
```

然后，使用 `makeself` 打包目录：

```bash
makeself /my-sdk my-sdk-inst.bin "My SDK Installation" ./install-script.sh
```

这条命令将会把 `/my-sdk` 目录下的所有文件打包成一个名为 `my-sdk-installer.bin` 的自解压安装文件，并在用户执行该文件时自动运行 `install-script.sh` 安装脚本。

### 步骤 4: 测试安装文件

在不同的环境测试你的安装文件，确保它在所有目标平台上都能正确执行。

### 步骤 5: 分发

一旦测试无误，你可以通过网站、FTP 或其他方法分发这个装文件。

这只是创建自解压安装文件的基本步骤。根据你的具体需求，可能还需要添加更多的错误检测、用户交互和配置功能。


# uboot
## dts
```dts
k3-am625-r5-sk.dts
	#include "k3-am625-sk.dts"
		#include "k3-am62x-sk-common.dtsi"
			#include "k3-am625.dtsi"
				#include "k3-am62.dtsi"
					#include "k3-am62-main.dtsi"
					#include "k3-am62-mcu.dtsi"
					#include "k3-am62-wakeup.dtsi"
					#include "k3-am62-thermal.dtsi"

	#include "k3-am62x-sk-ddr4-1600MTs.dtsi"

	#include "k3-am62-ddr.dtsi"
		#include "k3-am64-ddr.dtsi"

	#include "k3-am625-sk-u-boot.dtsi"
		#include "k3-am625-sk-binman.dtsi"
			#include "k3-binman.dtsi"
```

### 自动在k3-am625-sk.dts加k3-am625-sk-uboot.dtsi
![[Pasted image 20241101191522.png]]
![[Pasted image 20241101191630.png]]
![[Pasted image 20241101192244.png]]
![[Pasted image 20241101191719.png]]
![[Pasted image 20241101191735.png]]

## 安全启动
```bash
doc/usage/fit/howto.rst
boot/image-fit.c

./arch/arm/mach-k3/keys/custMpk.pem
./arch/arm/mach-k3/keys/ti-degenerate-key.pem


root@DESKTOP-GC4LAR7:/home/liweiyu/am62x/debug/ti-processor-sdk-linux-rt-am62xx-evm-10.00.07.04/board-support/ti-u-boot-2024.04+git# ll ./board/ti/am62x/
total 80
drwxr-xr-x  2 root  root  4096 Sep 25 07:32 ./
drwxr-xr-x 20 root  root  4096 Sep 18 09:34 ../
-rw-r--r--  1 60898 1373   606 Aug  5 11:40 Kconfig
-rw-r--r--  1 60898 1373   213 Aug  5 11:40 MAINTAINERS
-rw-r--r--  1 60898 1373   170 Aug  5 11:40 Makefile
-rwxrwxrwx  1 root  root  2768 Sep 19 06:36 am62x.env*
-rw-r--r--  1 60898 1373   798 Aug  5 11:40 board-cfg.yaml
-rwxrwxrwx  1 root  root  4414 Sep 18 09:33 evm.c*
-rw-r--r--  1 60898 1373   244 Aug  5 11:40 pm-cfg.yaml
-rw-r--r--  1 60898 1373 25889 Aug  5 11:40 rm-cfg.yaml
-rw-r--r--  1 60898 1373 11009 Aug  5 11:40 sec-cfg.yaml

ll tools/binman/
	etype/x509_cert.py
	etype/ti_secure.py
	etype/ti_secure_rom.py

	btool/openssl.py

	entries.rst
	ftest.py
```

## lvds
### 时钟确认
![[Pasted image 20241129162518.png]]

![[Pasted image 20241129162600.png]]

![[Pasted image 20241129200228.png]]

![[Pasted image 20241129162838.png]]

![[Pasted image 20241129163014.png]]


![[Pasted image 20241129161453.png]]
![[Pasted image 20241129161436.png]]

![[Pasted image 20241129195623.png]]

![[Pasted image 20241129194748.png]]

![[Pasted image 20241129195856.png]]

[manual.zlg.cn/Public/Uploads/2024-02-18/65d1b3b2b8c3f.pdf](https://manual.zlg.cn/Public/Uploads/2024-02-18/65d1b3b2b8c3f.pdf)

![[Pasted image 20241129162044.png]]
![[Pasted image 20241129162107.png]]
![[Pasted image 20241129161659.png]]
![[Pasted image 20241129161543.png]]

![[Pasted image 20241129161610.png]]


### oldi
```bash
enum dss_oldi_modes {
	OLDI_MODE_OFF,				/* OLDI turned off / tied off in IP. */
	OLDI_SINGLE_LINK_SINGLE_MODE,		/* Single Output over OLDI 0. */
	OLDI_SINGLE_LINK_DUPLICATE_MODE,	/* Duplicate Output over OLDI 0 and 1. */
	OLDI_DUAL_LINK,				/* Combined Output over OLDI 0 and 1. */
};


struct tidss_drv_priv {

	// priv->dev = dev;
	struct udevice *dev;


	// priv->base_common = dev_remap_addr_name(dev, priv->feat->common);
	void __iomem *base_common; /* common register region of dss*/


	// priv->base_vid[i] = dev_remap_addr_name(dev, priv->feat->vid_name[i]);
	void __iomem *base_vid[TIDSS_MAX_PLANES]; /* plane register region of dss*/



	// priv->base_ovr[i] = dev_remap_addr_name(dev, priv->feat->ovr_name[i]);
	void __iomem *base_ovr[TIDSS_MAX_PORTS]; /* overlay register region of dss*/


	// priv->base_vp[i] = dev_remap_addr_name(dev, priv->feat->vp_name[i]);
	void __iomem *base_vp[TIDSS_MAX_PORTS]; /* video port register region of dss*/



	struct regmap *oldi_io_ctrl;

	// ret = clk_get_by_name(dev, "vp1", &priv->vp_clk[0]);
	struct clk vp_clk[TIDSS_MAX_PORTS];


	// priv->feat = &dss_am625_feats;
	const struct dss_features *feat;


	// ret = clk_get_by_name(dev, "fck", &priv->fclk);
	struct clk fclk;


	struct dss_vp_data vp_data[TIDSS_MAX_PORTS];


	// priv->oldi_mode = OLDI_DUAL_LINK;
	enum dss_oldi_modes oldi_mode;

	// priv->bus_format = &dss_bus_formats[7];
	struct dss_bus_format *bus_format;


	// priv->pixel_format = DSS_FORMAT_XRGB8888;
	u32 pixel_format;



	u32 memory_bandwidth_limit;


};


struct video_uc_plat {
	uint align;
	uint size;
	ulong base;
	ulong copy_base;
	ulong copy_size;
	bool hide_logo;
};


struct video_priv {

	// uc_priv->xsize = timings.hactive.typ;
	ushort xsize;

	// uc_priv->ysize = timings.vactive.typ;
	ushort ysize;



	ushort rot;

	// uc_priv->bpix = VIDEO_BPP32;
	enum video_log2_bpp bpix;


	enum video_format format;
	const char *vidconsole_drv_name;
	int font_size;

	/*
	 * Things that are private to the uclass: don't use these in the
	 * driver
	 */
	void *fb;
	int fb_size;
	void *copy_fb;
	int line_length;
	u32 colour_fg;
	u32 colour_bg;
	bool flush_dcache;
	u8 fg_col_idx;
	u8 bg_col_idx;
};


struct video_ops {
	int (*video_sync)(struct udevice *vid);
};


dss_common_regmap = priv->feat->common_regs;


struct display_timing timings;
	ret = panel_get_display_timing(panel, &timings);
	if (ret) {
		ret = ofnode_decode_panel_timing(dev_ofnode(panel),
						 &timings);
		if (ret) {
			dev_err(dev, "decode display timing error %d\n", ret);
			return ret;
		}
	}

	dss_vid_write(priv, 0, DSS_VID_BA_0, uc_plat->base & 0xffffffff);
	dss_vid_write(priv, 0, DSS_VID_BA_EXT_0, (u64)uc_plat->base >> 32);
	dss_vid_write(priv, 0, DSS_VID_BA_1, uc_plat->base & 0xffffffff);
	dss_vid_write(priv, 0, DSS_VID_BA_EXT_1, (u64)uc_plat->base >> 32);

dss_init_am65x_oldi_io_ctrl

video_set_flush_dcache



/* common */
dss_write
dss_read

FLD_MASK
FLD_VAL
FLD_GET
FLD_MOD



/* read and modify common register region of DSS*/
REG_GET
REG_FLD_MOD




/* read and modify planes vid1 and vid2 register of DSS*/
VID_REG_GET
VID_REG_FLD_MOD

dss_vid_write
dss_vid_read

dss_plane_setup
dss_plane_enable
dss_plane_init



/* read and modify overlay ovr1 and ovr2 registers of DSS*/
OVR_REG_GET
OVR_REG_FLD_MOD

dss_ovr_write
dss_ovr_read

dss_ovr_set_plane
dss_ovr_enable_layer



/* read and modify port vp1 and vp2 registers of DSS*/
VP_REG_GET
VP_REG_FLD_MOD

dss_vp_write
dss_vp_read

dss_vp_enable_clk
dss_vp_set_clk_rate
dss_vp_prepare
dss_vp_enable
dss_vp_init



------------------------------------------------------------
tidss_drv_probe
	dss_vp_enable_clk

	dss_vp_set_clk_rate

	dss_init_am65x_oldi_io_ctrl

	dss_vp_prepare
		dss_oldi_tx_power
		dss_enable_oldi

	dss_vp_enable

	dss_vp_init

```
![[Pasted image 20241129195054.png]]

[PROCESSOR-SDK-AM62X: How to decide LCD parameters in panel-simple.c - Processors forum - Processors - TI E2E support forums](https://e2e.ti.com/support/processors-group/processors/f/processors-forum/1387625/processor-sdk-am62x-how-to-decide-lcd-parameters-in-panel-simple-c?tisearch=e2e-sitesearch&keymatch=PROCESSOR-SDK-J722S)
![[Pasted image 20241129162240.png]]

#### 电源配置
![[Pasted image 20241129163915.png]]
![[Pasted image 20241129163930.png]]
![[Pasted image 20241129164004.png]]

![[Pasted image 20241129164210.png]]

![[Pasted image 20241129164021.png]]

#### 使能
![[Pasted image 20241129170921.png]]
![[Pasted image 20241129170940.png]]
![[Pasted image 20241129170955.png]]

![[Pasted image 20241129172016.png]]

![[Pasted image 20241129194535.png]]

![[Pasted image 20241129172031.png]]

# 内核
## lvds
![[Pasted image 20241031161024.png]]
![[img_v3_02g6_be16b9aa-bf40-4b30-bfd8-4d1cc6c5915g.jpg]]

原因是tidss先于panel先初始化，这个错误无影响
![[Pasted image 20241031161120.png]]