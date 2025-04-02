
## Enhancing the Performance of Transformer-based Spiking Neural Networks by SNN-optimized Downsampling with Precise Gradient Backpropagation

[论文链接](https://arxiv.org/abs/2305.05954)

一种新的MaxPooling方法

3.1

作者这里证明了在最大池化的情况下存在一定的不精确性 公式1 描绘了下采样的反向传播公式 其中y/h为最大池化的特征图/神经元输出的特征图 他们的梯度展开的话是公式2 使用LIF模型 公式3到5 通过这里 可以算出神经元的反向传播梯度h/x 为公式6 因此将其带入回公式1可以得到公式7

然后看图1a 可以发现最大池化的操作是在经过LIF神经元后的脉冲上进行的 这回导致一个问题 脉冲只能为0或者1 因此最大值一定是1 只有在最大值的情况下才会更新梯度 但是公式7的huv条件 根据基础的maxpooling的特性 只有在第一个达到最大值的会被计算 后面的数除非比第一个最大值大 否则不会计算 这就导致在一个maxpooling区块内 只能有第一个发放的脉冲的神经元会被反向传播 但是这个神经元又不一定为携带信息最多的神经元 这就可能会导致信息丢失 举例就是1a中绿色小块 明明经过ConvBN 1.28的信息最多 但是因为0.76先发放了脉冲 导致1.28被舍弃了

3.2

提出了一种改进方法CML 在图1b可以很明显的看出来 这里将最大池化操作放在了ConvBN后的x上 参考公式8 9 这样最大池化的层就乘了h/x的层 公式10和11和上述同理 这样可以在最大池化的过程中保留携带信息最多的神经元了

然后作者在实验里证明了 用CML下采样模块 比基础的maxpooling块 和其他的例如ConvBN-AvgPool-LIF等准确率均有1左右的提升