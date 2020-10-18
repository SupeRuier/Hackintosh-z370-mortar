- [本机安装相关信息](#本机安装相关信息)
  - [硬件配置](#硬件配置)
  - [完美程度·](#完美程度)
    - [10.14](#1014)
    - [10.15.17](#101517)
- [安装步骤](#安装步骤)
  - [必备材料](#必备材料)
  - [具体步骤](#具体步骤)
  - [其他事项](#其他事项)
- [驱动相关信息](#驱动相关信息)
  - [ACPI](#acpi)
  - [Drivers](#drivers)
  - [kexts](#kexts)
- [其他关于硬件的信息](#其他关于硬件的信息)
  - [关于网卡](#关于网卡)

# 本机安装相关信息
这个文档记录本机当前使用的黑苹果信息。
并未自己配置，大部分配置文件来自网上现有的配置。

## 硬件配置
| CPU       | 具体型号                                                                 |
| --------- | ------------------------------------------------------------------------ |
| cpu       | Intel i5-8500                                                            |
| 主板      | msi Z370m mortar                                                         |
| 散热器    | 酷冷至尊 T400i                                                           |
| 内存      | ADATA 8g*4 DDR 4 3000                                                    |
| SSD       | 海康威视C2000 Pro 512g/ SAMSUNG MZYTE256HMHP-000L2 256g/ GLOWAY STK 1.5T |
| 网卡/蓝牙 | BCM943602cs/Intel I219-V Gigabit                                         |
| 显卡      | 蓝宝石 RX470 4G                                                          |
| 电源      | 爱国者电竞600 600W                                                       |
| 显示器    | KOIOS 2419UB 24寸4K                                                      |
| 机箱      | 夜行者DP501                                                              |


## 完美程度·

### 10.14
完美运行

### 10.15.17
已知缺陷：
- 机箱前部usb接口有时无法识别
- 移动硬盘会识别成内置硬盘
- u盘会识别为CD/DVD
- 蓝牙信号较弱（似乎是本显卡通病）
- 硬件加速等暂未测试

# 安装步骤

## 必备材料

必备材料：
- Clover Configurator
- Kext Utility
- Etcher
- CLOVER文件（直接使用了他人验证过的clover文件）
  - [Z370-i5-8G-RX590](https://github.com/MichaelVector/Hackintosh-Z370-i5-8G-RX590)
  - [Z370M MORTAR i7-8700K Vega 56](https://github.com/ArsnealX/Bootloader-Backup)
  - [Z370 TOMAHAWK 8700k RX580](https://github.com/luffythink/Hackintosh-EFI) 当前10.15主要用了这个

参考资料：
- 基于[B站视频](https://www.bilibili.com/video/av31949558?from=search&seid=3333347748583960377)
- 基于[黑果小兵](https://blog.daliansky.net/MacOS-installation-tutorial-XiaoMi-Pro-installation-process-records.html#undefined)

## 具体步骤

1. 准备u盘
   - Windows
     1. 格式化u盘
     2. 写入镜像（Restore with Disk Image），Mac上下载的安装包
     3. 拷入驱动，（用分区工具（DIskGenious）打开EFI分区，或用命令行挂载）
         1. 替换config.plist，kexts（驱动），
         2. 或者直接替换CLOVER文件夹（已经验证成功的配置）
         3. 安装过程中不要先替换EFI，等安装全部结果后再执行替换EFI的操作
     4. Clover Configurator需要放到u盘上
   - Mac：
     - 简单使用Etcher写入小兵的安装包。
   - 挂载分区适当调整驱动及config文件。

``` 
用命令行挂载的方式。
> diskutil list

/dev/disk4 (disk image):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        +8.0 GB     disk4
   1:                        EFI EFI                     209.7 MB   disk4s1
   2:                  Apple_HFS XiaoMiPro 10131         7.7 GB     disk4s2

> sudo diskutil mount disk4s1  # 挂载USB的EFI分区
Volume EFI on disk4s1 mounted
open .                  # 在当前位置打开Finder

# 之后将EFI复制进USB的EFI分区下即可,至于你想替换EFI也可以参考此方法操作（黑果小兵网站上的下载应该是已经包含EFI的，只需要替换相应驱动即可，但是建议最后再替换驱动。）
```

2. 开始安装
   1. U盘启动准备安装
      1. UEFI加U盘名
      2. BIOS开UEFI模式
      3. BIOS打开CSM选项
      4. 关闭安全模式（secure boot）
      5. 保存重启
   2. 选择苹果安装盘
      1. 之后就和白苹果安装很像了
      2. 磁盘工具抹盘为APFS格式（GUID分区图）
      3. 安装到刚刚抹掉的盘/这里应该可以从TimeMachine恢复
      4. 第一次安装结束 
   3. 第二次安装（有些镜像加入免二次安装补丁）
      1. 重启进入四叶草，选刚刚的Mac安装盘
   4. 选择磁盘进入系统
      1. 这次真的进入系统了，老方法设置
      2. 先不联网
      3. 这里应该也可以从TimeMachine恢复
      4. MacOS 安装结束

3. 安装结束后的设置
   1. 改硬盘启动（不插U盘启动Mac）
      1. 回到Win，打开DiskGenius/或者用Clover Configurator在mac下挂载分区
      2. 复制文件到硬盘的EFI分区，就像刚刚复制到U盘的EFI一样
      3. 这次好像不用删除，直接粘贴clover就好了
      4. 这里有一点需要注意:如果之前安装过Windows系统的话,会存在EFI的目录,只是EFI的目录下面只有BOOT和Microsoft这两个目录,如果希望添加macOS的Clover引导的话,可以将USB的EFI分区里面的EFI目录下面的CLOVER复制到磁盘里的EFI目录下,也就是执行的是**合并**的操作,让EFI同时支持WINDOWS和macOS的引导.千万不要全部复制,否则有可能造成EFI无法启动Windows.可能需要[扩大EFI分区](https://blog.csdn.net/GlowChar/article/details/88914927?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-7.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-7.channel_param)。
      5. \EFI\CLOVER\CLOVERX64.efi 复制到\EFI\BOOT文件夹并改名
   2. 安装修改引导软件（有了3.5之后或许可以省略了）
      1. 安装打开EasyUEFI
      2. 管理EFI启动项
      3. 启动序列，有个像新建的Button
      4. 选取Mac磁盘的EFI的位置，类型为Linux或其他操作系统
      5. 描述随便英文输入
      6. 文件路径\EFI\CLOVER\CLOVERX64.efi
      7. 确定
      8. 将新建引导调至第一项（有个向上键）


1~3步为创建Mac安装盘，在Mac上同样可以进行。参见[TonyMacX68](https://www.tonymacx86.com/threads/unibeast-install-macos-mojave-on-any-supported-intel-based-pc.259381/)，及[Mac上制作黑苹果引导](https://zhuanlan.zhihu.com/p/55256660)。
所使用软件为Unibeast（制作u盘，类似Unibeast）和 MultiBeast（驱动管理），同时需要clover configurator（在mac上挂载EFI分区），kext utility（修复/S/L/E (/System/Library/Extensions)里面的kext文件的权限，而且也可以方便你往这个文件夹里面添加kext文件）。

## 其他事项

1. Trim指令对于SSD意义重大，不支持Trim的话会加速SSD的老化和掉速。
所以必须要开启Trim，黑苹果默认没开启Trim，所以我们需要手动开启。
在网上找一款叫Trim Enabler的App，安装后在Trim选项卡开启Trim支持即可。

2. UHD630核显HDMI修复，见[黑果小冰教程](https://blog.daliansky.net/Tutorial-Using-Hackintool-to-open-the-correct-pose-of-the-8th-generation-core-display-HDMI-or-DVI-output.html)

3. 正常睡眠。
Mac的睡眠比Windows更智能更好用，我的Macbook就从来不关机，唤醒超快。
要支持睡眠也很简单，下载HibernationFixup.kext，使用Clover Configurator挂载EFI分区，随后把HibernationFixup.kext丢到EFI\Clover\kexts下，重启电脑即可。

4. CPU无法变频。
下载CPU-S这款软件一看，果然无法变频，无法变频会导致功耗发热增加。
解决方法很简单，使用Clover Configurator挂载EFI分区，打开Clover.config，在左侧选择boot选项，按提示添加-xcpm，随后在Kernel and kext Patches中，把FakeCPUID设置为0x0306A0，并且勾选KernelCPU、KernelPm、KernelXCPM、AppleIntelCPUPM，保存config，然后重启。
重启后再用CPU-S进行变频测试，测试结果有几档变频即可。

5. 微星Bios设置：
將開機改為 “UEFI USB Key:UEFI: 
（USB 名稱）:Parttion 1″優先 到 OC > CPU 功能
（CPU Features） > Intel VT-D 技術「取消（Disabled）」 到 OC > CPU 功能
（CPU Features） > Intel 虛擬化技術「啟用（Enabled）」 到 OC > CPU 功能
（CPU Features） > CFG Lock 「取消（Disabled）」 SATA模式需改為「AHCI模式」


# 驱动相关信息

## ACPI
- 台式机请移除SSDT-Disable-DGPU.aml
  - 已移除

## Drivers

- 300系列主板请于drivers64UEFI目录中移除AptioMemoryFix-64.efi添加OsxAptioFix2Drv-free2000.efi该驱动位于/EFI/CLOVER/drivers/off目录下 
  - 已移除并替换
  - 文件来源于 [台湾黑苹果交流中心](https://ihackintosher.wordpress.com/2019/03/15/efi-bootloader-調整設定（重要一定要做）/)


## kexts
- `Wireless.USB.Adapter.Clover-V7.zip` REALTEK无线网卡驱动，包括安装程序及通过`CLOVER`加载的驱动

- `VoodooPS2Controller.kext`和`ApplePS2SmartTouchPad.kext`两者选其一，不可全用
  - 用ApplePS2SmartTouchPad.kext，此项为键盘触控板驱动

- `IntelGraphicsDVMTFixup.kext`用于五代以上机器，四代及以下删除，`Whatevergreen.kext`v1.2.1已经包括这个补丁

- `XHCI`开头的三个`kext`对应适用于`x99`、`200`和`300`系主板，非此类主板删除
  - 已删除

- `AzulPatcher4600.kext`只适用于核心显卡`HD4600`，其他型号删除

- `CoreDisplayFixup.kext`用于破解`4K`支持，合理选用，`Whatevergreen.kext`v1.2.1已经包括这个补丁
  - **待考虑**

- `ATH9KFixup.kext`用于驱动`AR946x`、`AR956X`、`AR9485`高通无线网卡，合理选用

- `NoTouchID`用于禁止`TouchID`的检测，合理选用
  - 选用

- `WhateverGreen.kext`已经更新，合并了`igfx`、`ngfx`，以及`Shiki`，启动参数和以前一样，不论`A`卡、`N`卡还是`Intel`核心显卡，都建议使用

- `ACPIBatteryManager.kext`用于实现电量显示，如遇五国卡`BAT0`之类的请删除
  - 待考虑

- `AppleALC.kext`和`VoodooHDA-2.9.1.kext`任选其一
  - 选取AppleALC.kext

- `RTL8100.kext`、`RTL8111.kext`、`IntelMausiEthernet.kext`、`AppleIGB`、`SmallTree-Intel-211-AT-PCIe-GBE.kext`、`ALXEthernet.kext`、`AtherosE2200Ethernet.kext`分别对应不同的网卡合理选用
  - [z370m mortar](https://cn.msi.com/Motherboard/Z370M-MORTAR/Specification)网卡为Intel I219-V Gigabit 网卡
  - 故应选用IntelMausiEthernet.kext

    > 对应关系如下
    
    | RTL8100.kext                         | RTL8107E、RTL810X、RTL8139                                                                                                                           |
    | ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
    | RTL8111.kext                         | Realtek RTL8111/8168 B/C/D/E/F/G/H                                                                                                                   |
    | IntelMausiEthernet.kext              | 82578LM、82578LC、82578DM、82578DC、82579LM、82579V、I217LM、I217V、I218LM、I218V、I218LM2、I218V2、I218LM3、I219V、I219LM、I219V2、I219LM2、I219LM2 |
    | AtherosE2200Ethernet.kext            | AR816x、AR817x、Killer E220x、Killer E2400、Killer E2500                                                                                             |
    | SmallTree-Intel-211-AT-PCIe-GBE.kext | Intel I211                                                                                                                                           |
    | AppleIntelE1000.kext                 | Intel系列 82540, 82541, 82542, 82543, 82544, 82545, 82546, 82547, 82578 (P55/H55)  82579 (P67/H67) 82574L 82571 82572 82573 82574 82583 I217V        |
    | AppleIGB.kext                        | Intel 82575, 82576, 82580, dh89xxcc, i350, i210 and i211                                                                                             |
    | ALXEthernet.kext                     | Atheros alx Ethernet                                                                                                                                 |
    | FakePCIID_BCM57XX_as_BCM57765.kext   | BCM57XX                                                                                                                                              |
    | FakePCIID_Intel_GbX.kext             | Small Tree drivers for Intel chipset                                                                                                                 |
    


# 其他关于硬件的信息

## 关于网卡

| 接口                          | 模块                                       | 天线       | 无线                       | 蓝牙 | 备注                                                                                                                     |
| ----------------------------- | ------------------------------------------ | ---------- | -------------------------- | ---- | ------------------------------------------------------------------------------------------------------------------------ |
| PCIe (non-standard connector) | BCM94360CD                                 | 3T3R 4天线 | 2.4G 450M+/5G 1300M=1750M  | 4.0  | 最好的之一，iMac (2013) 使用，请勿购买三天线版本，三天线版本（3T3R）蓝牙和无线共用一根影响使用                           |
| PCIe (non-standard connector) | **BCM943602CS**  <br> 准备选这款           | 3T3R       | 2.4G 450M+/5G 1300M=1750M  | 4.1  | MacBook Pro (2015) 使用，蓝牙需要接 USB                                                                                  |
| PCIe (non-standard connector) | BCM94360CS                                 | 3T3R       | 2.4G 450M+/5G 1300M=1750M  | 4.0  | MacBook Pro (2014) 使用                                                                                                  |
| PCIe (non-standard connector) | BCM94360CS2                                | 2T2R       | 2.4G 300M+/5G 867M=1167M   | 4.0  | MacBook Air (2013) 使用，价格低廉，性价比最好                                                                            |
| M.2/NGFF 2230, key E          | BCM94352Z/AzureWave <br> AW-CE162NF/DW1560 | 2T2R       | 2.4G 300M+/5.0G 867M=1167M | 4.0  |
| M.2/NGFF 3030                 | BCM943602BAED/DELL DW1830                  | 3T3R       | 2.4G 450M+/5G 1300M=1750M  | 4.1  | M2上最好                                                                                                                 |
| M.2/NGFF 2230                 | BCM94350ZAE DW1820A                        | 2T2R       | 2.4G 300M+/5G 867M=1167M   | 4.1  | 已入 <br> 详情参见[黑果小冰Blog](https://blog.daliansky.net/DW1820A_BCM94350ZAE-driver-inserts-the-correct-posture.html) |
