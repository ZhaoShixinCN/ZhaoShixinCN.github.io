---
layout: post
title: 在A10-CubieBoard添加AP6210驱动
category : technology
tagline: "帮朋友研究一下"
tags : [Embedded]
---
{% include JB/setup %}

##需求
	一个基于A10-CubieBoard的二次开发板，使用了A20-CubieTrunk上的WiFi+BT芯片AP6210，读写方式同A20-CubieTrunk一致，需要将AP6210驱动移植到A10-CubieBoard上。
	 
	使用的系统是Cebian for CubieBoard
	 
	代码下载：git clone https://github.com/mmplayer/linux-sunxi.git -b dev/sunxi-3.4 --depth=1

	 
##由于A10和A20是使用同一份代码的，所以先分析一下源代码：

    AP6210的代码放在：/drivers/net/wireless/bcmdhd/
	 
    看了一下/drivers/net，/drivers/net/wireless，/drivers/net/wireless/bcmdhd各个目录的Kconfig和Makefile，能否用到这份驱动，应该主要用的是BCMDHD这个宏区分的。

	 
##比较A10和A20的config文件

    分别为：/arch/arm/configs/sun4i_defconfig和/arch/arm/configs/sun7i_defconfig
	 
    将sun4i_defconfig中添加字段：
	 
	CONFIG_BCMDHD=m
		  
	CONFIG_BCMDHD_FW_PATH="/lib/firmware/ap6210/fw_bcmxxxx.bin"
		  
	CONFIG_BCMDHD_NVRAM_PATH="/lib/firmware/ap6210/nvram_apxxxx.txt"
		  
	CONFIG_BCMDHD_OOB=y

##GPIO相关的改动（可能是最难的）
    管脚重新映射？
	 
    移植GPIO驱动
	 
          \arch\arm\mach-sun7i\rf\wifi_pm.c
		  
          \arch\arm\mach-sun7i\rf\wifi_pm_ap6xxx.c
	
##生成config并编译。
     
	还未验证
	 
	 
	 

###PS1：
	配置文件使用方法 
    
	cd ./linux-sunxi
	 
    cp arch/arm/configs/sun7i_defconfig .config
	 
###PS2：
	BCMDHD这个宏依赖MMC和GPIO两个模块，应该都要使能上。看文档，

