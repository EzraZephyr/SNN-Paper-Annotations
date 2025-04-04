
## Going Deeper With Directly-Trained Larger Spiking Neural Networks

[论文链接](https://arxiv.org/pdf/2011.05280)

使用tdBN归一化训练深层的SNN

当前的直接训练SNN 最多可以直接训练10层左右 通过作者提出的tdBN的方法 带入阈值进行归一化 可以训练到50层 在imagenet实现较高的准确率

使用LIF模型进行前向传播 参考公式2，3 为最基本的LIF神经元的公式 其中ot(j)表示其他神经元t时刻的二进制脉冲输出

随后就来到了这个阈值归一化的核心部分 公式5 带阈值的输入归一化 其中E[xk]为第k通道输入的均值 Var为方差 这两个参考公式7，8 然后ε为一个防止分母为0的小参数 而且因为这个阈值归一化只对训练有帮助 推理的时候带入这个阈值归一化是没有必要的 而且还会增加消耗 所以在推理的时候对卷积核和偏置进行去归一化操作 参考公式9 10 这个tdBN和正常BN的区别就是相较于普通的BN多参考了时间和阈值 最后的分布不是N(0,1) 而是N(0, (αVth)^2)

反向传播就是正常的反向传播 其中解决脉冲为非线性函数不可微使用了替代梯度 参考公式16

然后作者使用了Block Dynamical Isometry这个概念通过把网络看作多个block 对于每个block计算雅可比矩阵 当他们的期望迹约等于1和方差约等于0时则可以证明能避免梯度问题 尤其是当τdecay为0时 但是不能设置为0 因为会导致模型完全没有了时间维度传播 所以作者用τdecay=0.25等进行了评估 梯度范数增长的很慢 所以不会产生影响

具体和Resnet的差别查看Figure5