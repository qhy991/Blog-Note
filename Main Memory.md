```toc

```

## Intro to Computer Memory
内存DRAM，存储SRAM（SSD）
数据只有load到DRAM才能被CPU读取。
###  DRAM vs SSD
DRAM暂存 17nm 16GB
SSD永久存储 R/W time 50um（x3000）2TB
02:23 - Loading a Video Game
玩游戏的时候需要把游戏数据load到DRAM上。所以有些游戏对内存有要求。
04:07 - Notes
## Intro to DRAM, DIMMs & Memory Channels
DRAM又称DIMM（Dual Inline Memory Module，双线内存模块）
用于不同设备的DRAM有着不同的优化方向
|GPU|CPU|
|-|-|
|频率低||
|大带宽，同时读写|低功耗，小尺寸|

### Inside a DRAM Memory Cell
DDR 5 4 3 2
越来越快，存储电荷的电压越来越低
主板上有四个DRAM插槽
DDR5上有两个内存通道。
DRAM die上有8组（bank group），每组4个库，共32个库（bank），每个库有很多阵列，高65536个单元（$2^{16}$），宽8192个单元（$2^{13}$）
![[Pasted image 20221114235210.png]]

### An Small Array of Memory Cells
#1T1C

大的深宽比（沟槽式）能够实现更大的电容存储。
### Reading from DRAM
每次激活一行字线，再激活一列字线，就打开了一个单元
使用多路复用器来接受被激活位线上的信息，像是一个译码器
地址是字节地址，每次传输8bit

库地址选择库，行地址选择行，然后行上所有的电容预充电到0.5V

由感应放大器来维持电容中的0或1

### Writing to DRAM
写地址中线选中行，预充电
列地址传入后，由写驱动器来写数据，写驱动器比较强，
### Refreshing DRAM
关闭所有行，将位线预充电到0.5，然后打开一整行。感应放大器来根据电容器的存储值，来刷新。
按行刷新

Complicated DRAM Topics: Row Hits
一行上有很多列，包含很多字节的信息，如果下一个操作的内容在同一行，就可以省去读取行的操作，只取列，从而节省时间，与之对应的是行错过。

### Why 32 DRAM Banks?
DRAM分库，可以增加行命中的几率，因为不同库的多个行可以被同时打开。
减小CPU访问数据的时间。
可以在刷新一个库的时候，访问其他的库。

### DRAM Burst Buffers
读取、写入，加载多个字节
好处在于如果这些数据是相邻的，预先读取的数据就可以被极快访问
30:58 - Subarrays
Inside DRAM Sense Amplifiers
使用差分对的方式来搭建感应放大器
抗噪声
34:24 - Outro to DRAM
