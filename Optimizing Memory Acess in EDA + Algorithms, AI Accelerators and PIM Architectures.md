主要是优化显存
![[Pasted image 20221125100211.png]]

![[Pasted image 20221125100332.png]]

康老师偏下层，他们偏上层
![[Pasted image 20221125100419.png]]
![[Pasted image 20221125100433.png]]
![[Pasted image 20221125100443.png]]
![[Pasted image 20221125100505.png]]
计算访存比很低
![[Pasted image 20221125100526.png]]
![[Pasted image 20221125100555.png]]

1. 稀疏求解器
	1. 命中率提升
2. 访存
2. 多功能PIM架构

![[Pasted image 20221125100713.png]]
![[Pasted image 20221125100725.png]]
SPICE用于做电路级的仿真
![[Pasted image 20221125100810.png]]
注重分解的过程
![[Pasted image 20221125101008.png]]
针对稠密矩阵的话，Left-Looking没问题
![[Pasted image 20221125101206.png]]
对于稀疏的时候就不太合适。
稀疏的矩阵的时候，访存变得不规则
![[Pasted image 20221125101416.png]]
分散-累积

![[Pasted image 20221125101623.png]]
前人的研究认为效果不好，因为会带来额外的消耗
![[Pasted image 20221125101845.png]]

![[Pasted image 20221125102100.png]]
算法根据矩阵稀疏度来进行选择是否使用Supernode
![[Pasted image 20221125102256.png]]
![[Pasted image 20221125102402.png]]
CNN加速器里的下届
![[Pasted image 20221125102434.png]]

![[Pasted image 20221125102540.png]]
数据重用方式

搜索空间很大

通信最优的矩阵乘

片上存储无法实现整个列装载，部分装载，运算也还是得到红色的矩阵
![[Pasted image 20221125102938.png]]

![[Pasted image 20221125103051.png]]
所有的权重变成一个矩阵，每个卷积核变成一列，把不同的输出通道放到不同的列，batch size不等于1，在下面叠。
输入的话，不是等价变化
（缺了一页ppt im2col

![[Pasted image 20221125103604.png]]

把卷积变成矩阵乘差别在于输入，输入需要展开
![[Pasted image 20221125103631.png]]

通信最优的卷积
![[Pasted image 20221125104157.png]]
![[Pasted image 20221125104256.png]]
![[Pasted image 20221125104353.png]]
![[Pasted image 20221125104629.png]]
![[Pasted image 20221125104641.png]]
![[Pasted image 20221125104717.png]]
![[Pasted image 20221125104727.png]]
非线性计算怎么用PIM实现
![[Pasted image 20221125104833.png]]
所有的算子都能被映射上去
![[Pasted image 20221125104956.png]]

![[Pasted image 20221125105050.png]]
![[Pasted image 20221125105123.png]]
![[Pasted image 20221125105149.png]]
![[Pasted image 20221125105209.png]]
做逻辑用
![[Pasted image 20221125105234.png]]

![[Pasted image 20221125105315.png]]
仍存在的功能，功能如何布局（模拟退火算法
![[Pasted image 20221125105414.png]]

![[Pasted image 20221125105541.png]]

细腻度的优化，类似流水线，让本层的结果尽可能是下一层的输入