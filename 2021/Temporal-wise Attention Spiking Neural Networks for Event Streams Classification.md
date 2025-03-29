
## Temporal-wise Attention Spiking Neural Networks for Event Streams Classification

[论文链接](https://openaccess.thecvf.com/content/ICCV2021/papers/Yao_Temporal-Wise_Attention_Spiking_Neural_Networks_for_Event_Streams_Classification_ICCV_2021_paper.pdf)

引入注意力机制的SNN

因为由DVS生成的事件流的特性很适合被SNN所处理 但是SNN将事件流做成帧的时候并没有考虑信噪比 导致出现大量冗余 因为有很多细微变化也被记录 导致性能降低

所以这篇论文提出了一种注意力机制的SNN， TA-SNN解决这个问题

使用LIF模型 论文还提到了LIAF模型 但是这种模型不符合生物学特性 因为起用ReLU代替脉冲触发 取消掉了阈值 而是改为电流的不停衰减与膜电位的直接累计 但是这种方法也有一个好处就是将脉冲的非线性换成了ReLU的线性 可直接反向传播训练

还突出了一种RCS的数据增强技术 通过对DVS相机的事件流进行随机切片 增加数据的多样性

现在为TA-SNN的核心方法

首先见公式5 使对空间和通道维度进行归一化 用S来代表这个向量 随后通过ReLU和Sigmoid函数 加上两个可训练的权重来控制这个向量 参考公式6 使其变成一个注意力分数 最后将这个注意力分数d与输入X进行相乘来形成加权输入用于更新膜电位 参考公式7，8

在推理阶段 设定一个阈值dth 在推理阶段直接丢弃低于该阈值的帧 参考公式6 使用一个越阶函数f来设置 通过减去一个dth来判断该注意力分数是否大于阈值 大于0输出1保留该帧 小于或等于0直接丢弃置 