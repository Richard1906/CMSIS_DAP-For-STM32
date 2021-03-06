*** 该工程请使用Keil 4打开编译，且要求Keil有注册RL-ARM，否则会出现编译失败。没有测试过5以上版本 ***

CMSIS_DAP-For-STM32
=============================================
此项目为移除ARM公司的CMSIS_DAP仿真器固件到STM32平台。

CMSIS-DAP是ARM公司专为嵌入式开发提供的一款简单调试工具方案，主要用于支持其Cortex内核的芯片仿真，支持的硬件接口为2线制的SWD和5线制的JTAG。开放基础代码，可移植到不同的硬件平台，支持多种不同类型的IDE，其结构如下。

![image](https://github.com/tongban/CMSIS_DAP-For-STM32/raw/master/doc/cmsis_dap_interface.png)

本方案采用了STM32F103，本设计采用的是STM32F103实现，实测可用于仿真Cortex-M0/M3/M4，其它类型的MCU未测试过。效果图如下：  
![image](https://github.com/tongban/CMSIS_DAP-For-STM32/raw/master/doc/a2380bce-ec79-4770-afc7-42df777baab2.png)

![image](https://github.com/tongban/CMSIS_DAP-For-STM32/raw/master/doc/6d01c82f-9224-48b7-944e-3687cbc5f661.png)

原电路图已遗失，不过关系不大。其结构主要为对外提供USB的STM32F103最小系统，外接5个74LVC1T45用于实现JTAG/SWD的五个管脚。  
![image](https://github.com/tongban/CMSIS_DAP-For-STM32/raw/master/doc/0d59804f-a3c4-4807-a1e8-c33b6dfb2100.png)

74LVC1T45为带方向控制的双向缓冲器，DIR控制输入输出方案，A/B为IO接口。所以，针对SWD/JTAG的每根信号号，用STM32F103的2个IO，一根用于信号，一个用于方向控制即可。

管脚映射
------------------------------------
* PB2, SWDCLK/TCK方向控制，输出高电平，打开输出
* PB15, SWDCLK/TCK, 输出模式，输出高电平
* PB1, SWDIO/TMS方向控制，输出模式，输出高电平，打开输出
* PB14, SWDIO/TMS, 输入输出模式，输出高电平
* PB0, Reset方向控制，输出模式，输出高电平，打开输出
* PB13, Reset,输出模式，输出高电平
* PB3, nTRST方向控制，输出模式，输出高电平，打开输出
* PB12, nTRST,输出模式，输出高电平
* PB4, TDI方向控制，输出模式，输出高电平，打开输出
* PA3, TDI,输出模式，输出高电平
* PB5, TDO方向控制，输出模式，输出低电平，打开输入
* PA2, TDO, 输入上下拉

实测效果
-----------------------------------
毕竟是个简化版本的仿真器，所以ARM公司在PC机上的固件做了一些限制，有些功能本可以有但实际没有，比如运行中修改变量，观察时间计数。

性能上也比较弱，受限于STM32F103本身的USB只到2.0全速，且各调试信号只能用IO模拟，所以调试速度上不去。

网上有人认为有了这个就可以不用ULink/JLink，其实是不正确的。

联系方式
------------------------------------
请访问我的GitHub主页
