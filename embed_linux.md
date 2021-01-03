# 嵌入式Linux学习笔记——基于迅为4412精英版开发板

## OTG接口烧写Android固件

### 1 准备工具

1. USB_fastboot_tool
2. 安卓ADB驱动

### 2 准备镜像文件

1. ramdisk-uboot.img 
2. system.img 
3. u-boot-iTOP-4412.bin  (使用 SCP 1G封装的版本)
4. zImage (使用SCP封装的版本)

### 3 烧写步骤

1. 拷贝以上4个镜像文件到USB_fastboot_tool的platform-tools文件夹下面

2. 打开串口，上电启动开发板，按回车键，进入Uboot模式

3. 创建eMMC分区并格式化，输入如下命令

   ```
   fdisk -c 0
   fatformat mmc 0:1
   ext3format mmc 0:2
   ext3format mmc 0:3
   ext3format mmc 0:4
   fastboot
   ```

4. 在platform-tools目录下面运行命令行，执行以下命令

   ```
   fastboot.exe flash bootloader u-boot-iTOP-4412.bin    # uboot
   fastboot.exe flash kernel zImage                      # 内核
   fastboot.exe flash ramdisk ramdisk-uboot.img          # ramdisk
   fastboot.exe flash system system.img                  # 文件系统
   fastboot -w                                           # 擦除
   ```

5. 命令行中输入重启开发板命令

   ```
   fastboot reboot
   ```