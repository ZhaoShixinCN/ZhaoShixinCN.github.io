---
layout: post
title: 在A10-CubieBoard添加AP6210驱动
category : technology
tagline: "帮朋友研究一下"
tags : [Embedded]
---
{% include JB/setup %}

##需求
	一个基于A10-CubieBoard的二次开发板，使用了A20-CubieTrunk上的WiFi+BT芯片AP6210，同样通过SDIO方式访问AP6210，需要将AP6210驱动移植到A10-CubieBoard上。
	使用的系统是Cebian for CubieBoard
	代码下载：git clone https://github.com/mmplayer/linux-sunxi.git -b dev/sunxi-3.4 --depth=1

##解法一	
 
###分析
	由于A10和A20是使用同一份代码的，所以先分析一下源代码：
    AP6210相关驱动的代码放在：/drivers/net/wireless/bcmdhd/
    看了一下/drivers/net，/drivers/net/wireless，/drivers/net/wireless/bcmdhd各个目录的Kconfig和Makefile，能否用到这份驱动，应该主要用的是BCMDHD这个宏区分的。

	 
###引用驱动
	比较A10和A20的config文件，分别为：/arch/arm/configs/sun4i_defconfig和/arch/arm/configs/sun7i_defconfig
    将sun4i_defconfig中添加字段：
	CONFIG_BCMDHD=m
	CONFIG_BCMDHD_FW_PATH="/lib/firmware/ap6210/fw_bcmxxxx.bin"
	CONFIG_BCMDHD_NVRAM_PATH="/lib/firmware/ap6210/nvram_apxxxx.txt"
	#下面这个宏不清楚是否需要打开，似乎是使能“通过wifi远程开机”功能的，先不打开。
	CONFIG_BCMDHD_OOB=n
	此外，BCMDHD这个宏依赖MMC和GPIO两个模块，应该都要使能上。可能还有以下相关的宏，未做最终验证，仅供参考。
	CONFIG_B43_PHY_N=y
	CONFIG_B43_PHY_HT=y
	CONFIG_B43LEGACY=m
	CONFIG_BRCMFMAC=m
	CONFIG_BRCMFMAC_USB=y
	CONFIG_HOSTAP=m
	
	PS：
	配置文件使用方法：
    cd ./linux-sunxi
	cp arch/arm/configs/sun4i_defconfig .config


###其他改动
	GPIO相关的改动（可能是最难的），看代码，系统是通过GPIO来访问AP6210芯片类型，以及控制AP6210中断的。需要移植以下GPIO相关代码到\arch\arm\mach-sun4i\rf\，并重新映射管脚：
        \arch\arm\mach-sun7i\rf\bt_pm.c
        \arch\arm\mach-sun7i\rf\wifi_pm.c
        \arch\arm\mach-sun7i\rf\wifi_pm_ap6xxx.c
	
##===================华丽分割线===================

##解法二

针对CubieBoard，在github还另外有一个库：[EddyBeaupre/linux-sunxi-ap6210](https://github.com/EddyBeaupre/linux-sunxi-ap6210 "With a Title").

这套代码里是通过独立的AP6210驱动来支持AP6210芯片的。

添加代码和编译宏的方法详见网页：[Cubietruck/AP6210](http://linux-sunxi.org/Cubietruck/AP6210 "With a Title").

用这种方法，应该只需要重写以下AP6210 GPIO相关的代码，就可以了。代码均在/drivers/net/wireless/AP6210下。

##===================华丽分割线2===================

##补充

	1. 由于是帮忙研究，并未直接调试，因此不确定这个二次开发板的硬件连线同CubieTrunk完全一致，不确定是否需要修改U-boot。
	2. 由于并未真正去编译Linux内核，所以开启了一部分宏之后，是否会衍生出一些其他问题，还不清楚。

##验证

	未完成。

