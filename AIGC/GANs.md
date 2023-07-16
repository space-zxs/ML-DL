## GAN 生成式对抗网络及其家族

 _**GAN->GANs变种**_

---
### GAN
 _**GANs的基本思想和训练过程**_

 ![image](https://github.com/space-zxs/ML-DL/assets/77714764/4d15e0ef-efe0-4f02-85d2-3e09feb649f7)

- 包括**生成器（Generator）** 和 **判别器（Discriminator）** 两个部分。其中，生成器用于合成“假”样本，判别器用于判断输入的样本是真实的还是合成的。具体来说，生成器从先验分布中采得随机信号，
经过神经网络的变换，得到模拟样本；判别器既接收来自生成器的模拟样本，也接收来自实际数据集的真实样本，但我们并不告诉判别器样本来源，需要它自己判断

- （1）在训练判别器时，先固定生成器G(·)；然后利用生成器随机模拟产生样本G(z)作为负样本（z是一个随机向量），并从真实数据集中采样获得正样本X；将
这些正负样本输入到判别器D(·)中，根据判别器的输出（即D(X)或D(G(z))）和样本标签来计算误差；最后利用误差反向传播算法来更新判别器D(·)的参数，

![image](https://github.com/space-zxs/ML-DL/assets/77714764/aac5b7c5-48e9-4f98-bff3-c14aee5acbb7)

-（2）在训练生成器时，先固定判别器D(·)；然后利用当前生成器G(·)随机模拟产生样本G(z)，并输入到判别器D(·)中；根据判别器的输出D(G(z))和样本标签来计算误差，最后利用误差反向传播算法来更新生成器G(·)的参数，如图所示。

![image](https://github.com/space-zxs/ML-DL/assets/77714764/16fba1dc-171a-489a-bf2f-45ced7fac518)


_**GANs的值函数**_

倘若固定D而将G优化到底，那么解GD*和此时的值函数又揭示出什么呢？

最终还是个二分类问题，所以损失式二元交叉熵

![image](https://github.com/space-zxs/ML-DL/assets/77714764/28e68ef2-c605-44e0-abf3-3ee48bbd24ad)

进一步可以写成

![image](https://github.com/space-zxs/ML-DL/assets/77714764/5beeb1ba-1c3a-47fd-a84d-850cddf48621)


![image](https://github.com/space-zxs/ML-DL/assets/77714764/14e014d1-7bbf-4972-93a2-ab021e1ddea2)

_**GANs如何避开大量概率推断计算？**_


![image](https://github.com/space-zxs/ML-DL/assets/77714764/a4b9d7ba-977a-430e-8291-0665bd52e4ab)


_**GANs在实际训练中会遇到什么问题？**_


在实际训练中，早期阶段生成器G很差，生成的模拟样本很容易被判别器D识别，使得D回传给G的梯度极其小，达不到训练目的，这个现象称为优化饱和[33]。

故G获得的梯度基本为零，这说明D强大后对G的帮助反而微乎其微。


![image](https://github.com/space-zxs/ML-DL/assets/77714764/91aadb14-1817-4562-82b0-6529e3afbeb8)


---

### stylegan




