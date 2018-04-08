# MT7688-OpenOCD
OpenOCD configuration for MT7688AN/MT7688KN/MT7628x

# 这是什么
MT7688AN/MT7688KN/MT7628x的OpenOCD配置文件.
请相信他可以救活一台已经彻底变砖的路由器/智能设备/各种物联网设备, 甚至您的设备flash已经完全损坏/空白/未焊接.
该代码已在MT7688AN测试通过, 测试设备为OrangePi, 使用bitbang(即gpio软件模拟)方式与MT7688AN通讯.
理论上通用所有MT76x8系列soc.

# 这是如何做到的
MT76X8系列是MIPS-24kec内核的soc芯片, 该系列soc提供了JTAG接口可用于调试. 对于MT7688系列, 相关引脚定义与操作流程如下:
使用时只需要让soc以DBG_JTAG_MODE的模式开机, 要进入这个模式, 只需要将UART_TXD1(编号111)引脚拉至低电平上电即可.
在该模式下, 5个以太网状态LED分别对应如下:
143 EPHY_LED0 JTDO
142 EPHY_LED1 JTDI
141 EPHY_LED2 JTMS
140 EPHY_LED3 JTCLK
139 EPHY_LED4 JTRST_N

如您需要复位信号可在如下引脚引出:
138 PORST_N

# 如何使用
下载debug.cfg与ramboot.bin, 其中debug.cfg为OpenOCD配置文件, ramboot.bin为预先编译好的uboot, 代码空间位于内存当中.
请使用这个配置文件启动OpenOCD:
openocd -f /path/to/debug.cfg
之后请telnet连接到openocd的会话，执行 ddrinit 命令初始化DDR内存.
DDR内存初始化成功后, 执行 load_ramboot 命令, 将uboot下载到ram中运行. 之后您即可使用uboot向flash烧写新的代码.

# 特殊说明
您也可以自行编译您自己的uboot并使用该脚本下载到ram当中运行, 当然, 您需要自行寻找编译uboot到MT76x8的方法.
这个项目可以自动识别您的DDR与soc类型, 理论上可通用所有MT76x8系列芯片.
如果ramboot.bin加载成功但串口仍然无任何输出, 则可能是您的设备完全没有任何固件导致, 如遇该情况您可以尝试使用ramboot-cache.bin代替ramboot.bin加载到设备当中.

# 有许可证么
拿走, 不谢, WTFPL.
