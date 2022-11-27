现在的LUT存在的问题主要就是感受野太小，所以
-   提出串行并列查找表（SPLUT）
-   级联多个LUT提升感受野，包括两个分支的级联LUT并行网络
-   不需要插值方法，进一步降低计算量

三个主要贡献：

1.  最先提出级联LUT来正大感受野（和同期华为MuLUT文章撞车了
2.  提出新的并行网络弥补离散化时带来的精度损失
3.  证明了SPLUT高效的拓展性

**与SRLUT比较：**

![](https://wiki.megvii-inc.com/download/attachments/419350153/image2022-11-17_11-34-14.png?version=1&modificationDate=1668656053396&api=v2 "秦海岩(实习) > （SPLUT）Learning Series-Parallel Lookup Tables for Efficient Image Super-Resolution > image2022-11-17_11-34-14.png")

-   两种方法扩大感受野的方式不同，旋转VS级联
-   对图像的处理不同，旋转VS分高低频（MSB与LSB）
-   SRLUT使用插值来提高恢复的精度，SPLUT通过MSB与LSB的并行来弥补精度损失

  

方法：

下一级的LUT的索引是上一级的LUT，LUT级联的方式来扩大感受野，以获得更好的上下文信息。也正是因为LUT级联扩大了感受野，插值的负担指数上升，因此不采用插值。

为了弥补离散化的损失，使用并联框架，同时处理MSB和LSB。（那这样的方法是不是降低了LUT的大小，16^8→(16^4)*2）

  

影响LUT大小的三个因素，索引的像素n，可能的像素值v，输出向量的长度c，LUT的大小为![](https://wiki.megvii-inc.com/plugins/servlet/latexmath/placeholder?key=fd482f4621507317ca299540024d8b6a&vertAlign=-2px)。

SRLUT的n一般不超过4.256个像素值也进行了采样处理，一般是16。

c的值取决于超分的倍数，x4超分的话，c=16。

为了权衡精度和效率，实验中通过改变训练时中间层的out channel来分了三档，SPLUT-S/M/L。

  

把RGB三个通道同等看待，把input的8位像素，对半拆成MSBs和LSBs

MSBs关注语义语境信息，LSBs关注高频信息。

两个分支把![](https://wiki.megvii-inc.com/plugins/servlet/latexmath/placeholder?key=b4101f83104556c09982bde2590e3b7c&vertAlign=-7px)分别作为输入。

  

![](https://wiki.megvii-inc.com/download/attachments/419350153/image2022-11-15_14-58-56.png?version=1&modificationDate=1668495537564&api=v2 "秦海岩(实习) > （SPLUT）Learning Series-Parallel Lookup Tables for Efficient Image Super-Resolution > image2022-11-15_14-58-56.png")

以SPLUT-M为例，它有八个中间层结果，分为两组，分别过水平聚合模块和垂直聚合模块。

**聚合模块**

**![](https://wiki.megvii-inc.com/download/attachments/419350153/image2022-11-17_14-36-55.png?version=1&modificationDate=1668667015013&api=v2 "秦海岩(实习) > （SPLUT）Learning Series-Parallel Lookup Tables for Efficient Image Super-Resolution > image2022-11-17_14-36-55.png")**

对于两个输入，分别在左边和右边进行pad，再相加，再量化得到的结果就拥有更大的空间感受野。

这个想法还是很有心意，眼前一亮，感受野拓展到空间维度。

**感受野扩大的过程**

**![](https://wiki.megvii-inc.com/download/attachments/419350153/image2022-11-17_14-46-29.png?version=1&modificationDate=1668667588358&api=v2 "秦海岩(实习) > （SPLUT）Learning Series-Parallel Lookup Tables for Efficient Image Super-Resolution > image2022-11-17_14-46-29.png")**

**训练**

训练过程中把LUT部分替换成立Net，这个网络的基本结构和SRLUT是一样的，除第一层外都是1x1卷积。最后一层更加超分大小设置，再用pixelshuffle来恢复为3通道。

对于不同的LUT，替换时Net的第一层也不一样，对于空间LUT部分，input channel为1，![](https://wiki.megvii-inc.com/plugins/servlet/latexmath/placeholder?key=74101d3172c49a7b91f1537d79bfb905&vertAlign=-5px)，对于![](https://wiki.megvii-inc.com/plugins/servlet/latexmath/placeholder?key=125e1f5347f4068b93bb8df69e70d997&vertAlign=-7px)，input channel为2，对于![](https://wiki.megvii-inc.com/plugins/servlet/latexmath/placeholder?key=0b367ff3ee55a1743e60e72741b5a930&vertAlign=-5px)，![](https://wiki.megvii-inc.com/plugins/servlet/latexmath/placeholder?key=74eed99d58e62188e852c3a3112de0af&vertAlign=-7px)，对于![](https://wiki.megvii-inc.com/plugins/servlet/latexmath/placeholder?key=05ce0f45e65bec7dbc9c30be5a3bff60&vertAlign=-7px)。

量化激活部分使用直通估计器实现端到端的反向传播。