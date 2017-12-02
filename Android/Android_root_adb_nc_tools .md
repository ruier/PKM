# Android root 

root Android 设备，首先需要能够修改系统镜像，比如 `fastboot` 解锁，或者 `rom` 烧写工具。

## MTK

MTK 提供官方的烧写工具是 SP FLASH TOOL， 我这里下载了 SP_Flash_Tool_exe_Windows_v5.1528.00.000 ，通过这个工具读取系统 `boot.img` 这是包含 kernel image 和 ram disk 的 image， root 的过程就是修改 boot image 的过程。

## `boot.img` 的解包工具

从 github 下载 android 解包工具： https://github.com/ruier/binary-tools-android

里面包含 boot 镜像解包打包和 ramdisk 解包打包工具：

`boot_info  LICENSE.md  mkbootimg  repack_ramdisk  split_boot  unmkbootimg  unpack_ramdisk`

1. 查看 boot info
```bash
   boot_info boot.img
   Page size: 2048 (0x00000800)
   Kernel size: 6406062 (0x0061bfae)
   Ramdisk size: 1153596 (0x00119a3c)
   Second size: 0 (0x00000000)
   Board name: 1490252258
   Command line: 'bootopt=64S3,32N2,64N2'
   Base address: 1308622592 (0x4dffff00)
```

2. 解开 `boot.img`

   ```bash
   /unmkbootimg boot.img
   unmkbootimg version 1.2 - Mikael Q Kuisma <kuisma@ping.se>
   Kernel size 6406062
   Kernel address 0x40080000
   Ramdisk size 1153596
   Ramdisk address 0x44000000
   Secondary size 0
   Secondary address 0x40f00000
   Kernel tags address 0x4e000000
   Flash page size 2048
   Board name is "1490252258"
   Command line "bootopt=64S3,32N2,64N2"

   *** WARNING ****
   This image is built using NON-standard mkbootimg!
   OFF_KERNEL_ADDR is 0xF2080100
   OFF_RAMDISK_ADDR is 0xF6000100
   OFF_SECOND_ADDR is 0xF2F00100
   Please modify mkbootimg.c using the above values to build your image.
   ****************

   Extracting kernel to file zImage ...
   Extracting root filesystem to file initramfs.cpio.gz ...
   All done.
   ---------------
   To recompile this image, use:
     mkbootimg --kernel zImage --ramdisk initramfs.cpio.gz --base 0x4dffff00 --cmdline 'bootopt=64S3,32N2,64N2' --board '1490252258' -o new_boot.img
   ---------------

   ```

   按照解开的提示，这是修改过 `mkbootimg` 程序打包的 `boot.img` ，所以需要修改源码，重新编译：

   adb 程序需要编译一个能够获取 root 权限的 bin 打包进入 `boot.img`

   然后修改系统属性：`ro.secure` `ro.debuggable`

   ## 网络连接 `adb`

# adb 命令之 adb connect

​                    原创                    2013年04月11日 15:53:35                

- ​



1.adb connect + IP ，可以链接某个设备。

   这个命令在调试真机的时候，使用频繁。需要先用 USB 连接，然后打开网络调试端口。命令是 usb tcpip

 但注意：

   1.要链接的IP ，必须和自己的PC的网络在同一个局域网内，adb 不能跨局域网链接设备

   2.如果通过usb链接android设备，通过adb devices 可以看见设备列表，但是使用不了，可以参考下面的命令

```bash
adb tcpip 5555
adb connect 192.168.0.101:5555
```

权限问题，直接连接需要先连接 usb 执行 connect 命令

# nc 模拟服务器发送 json 数据

发送：

```bash
echo "{"modid":"888","name":"凯默变","status":"故障","remark":"维修时间剩余三个月"}" | nc 10.56.56.40 8155
```

接收：

```bash
nc -l 8155
```

