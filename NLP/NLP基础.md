## NLP基础串连
_从RNN -> LSTM -> Attention家族 -> Transformer -> LLM系列_

---
### RNN 

_**提出的前提**_

- 传统文本处理任务的方法中一般将**TF-IDF**向量作为特征输入。显而易见，这样的表示实际上丢失了输入的文本序列中每个单词的顺序
- **卷积神经网络**处理文本信息时，通过**滑动窗口加池化的方式**将原先的输入转换成一个固定长度的向量表示，这样做可以捕捉到原文本中的一些**局部特征**，但是两个单词之间的**长距离依赖关系**还是很难被学习到。

_**RNN能很好的解决上面的问题，顺序和长距离依赖**_

                                  经典RNN结构

![经典RNN结构](https://github.com/space-zxs/ML-DL/assets/77714764/0b05a54c-d670-492d-a0cd-855b4542f133)

                                  
第t层的隐含状态ht编码了序列中前t个输入的信息，可以通过当前的输入xt和上一层神经网络的状态ht−1计算得到；最后一层的状态hT编码了整个序列的信息

例如，在hT后面直接接一个Softmax层，输出文本所属类别的预测概率y，就可以实现文本分类

![image](https://github.com/space-zxs/ML-DL/assets/77714764/91730ba8-a90f-4c9d-b262-345ad6156627)

其中f和g为激活函数，U为输入层到隐含层的权重矩阵，W为隐含层从上一时刻到下一时刻状态转移的权重矩阵。在文本分类任务中，f可以选取Tanh函数或者ReLU函数，g可以采用Softmax函数。

### RNN中的梯度问题

_**RNN中为什么会出现梯度消失或梯度爆炸？又该如何解决？**_

根本原因在于[反向传播算法](https://www.cnblogs.com/charlotte77/p/5629865.html) 链式求导法则

并且存在激活函数，当激活函数的导数大于1的时候，梯度大小会呈指数增长，导致梯度爆炸，反之，梯度的大小会呈指数缩小，产生梯度消失

[梯度爆炸的解决办法](),[梯度消失的解决办法]()

_**更换RELU函数还是不能解决梯度问题的原因**_
![image](https://github.com/space-zxs/ML-DL/assets/77714764/fc8aef27-4cf6-4710-9e3e-2058a4f13938)

因为w的n次方在单位矩阵周围趋近于1

---

### LSTM

- RNN中的**梯度消失**和**梯度爆炸**的问题，学习能力有限，在实际任务中的效果往往达不到预期效果
- LSTM可以对有价值的信息进行**长期记忆**，从而减小RNN的学习难度

_**LSTM是如何实现长短期记忆功能的？**_

                                LSTM结构图
                                
  ![image](https://github.com/space-zxs/ML-DL/assets/77714764/e10a46f3-ea94-4de3-abe2-53507e16d9d0)

与RNN相比，LSTM**仍然是基于xt和ht−1来计算ht**，只不过对内部的结构进行了更加精心的设计，加入了**输入门it、遗忘门ft以及输出门ot三个门和一个内部记忆单元ct**。输入门控制当前计算的新状态以多大程度更新到记忆单元中；遗忘门控制前一步记忆单元中的信息有多大程度被遗忘掉；输出门控制当前的输出有多大程度上取决于当前的记忆单元。

经典的LSTM中，第t步的更新计算公式为：

![image](https://github.com/space-zxs/ML-DL/assets/77714764/108bce27-b08c-46fb-948a-537c6b930188)

输入门it的结果是向量，其中每个元素是0到1之间的实数，用于控制各维度流过阀门的信息量；Wi、Ui两个矩阵和向量bi为输入门的参数，是在训练过程中需要学习得到的。遗忘门ft和输出门ot的计算方式与输入门类似，它们有各自的
参数W、U和b。与传统的循环神经网络不同的是，从上一个记忆单元的状态ct−1到当前的状态ct的转移不一定完全取决于激活函数计算得到的状态，还由输入门和遗忘门来共同控制

_**LSTM里各模块分别使用什么激活函数，可以使用别的激活函数吗？**_


_在LSTM中，遗忘门、输入门和输出门使用Sigmoid函数作为激活函数；在生成候选记忆时，使用双曲正切函数Tanh作为激活函数。值得注意的是，这两个激活函数都是饱和的，也就是说在输入达到一定值的情况下，输出就不会发生明显变化了。如果是用非饱和的激活函数，例如ReLU，那么将难以实现门控的效果。 Sigmoid函数的输出在0～1之间，符合门控的物理定义。且当输入较大或较小时，其输出会非常接近1或0，从而保证该门开或关。在生成候选记忆时，使用Tanh函数，是因为其输出在−1～1之间，这与大多数场景下特征分布是0中心的吻合。此外，Tanh函数在输入为0附近相比Sigmoid函数有更大的梯度，通常使模型收敛更快。_

### seq2seq 

- Seq2Seq模型提出之前，深度神经网络在图像分类等问题上取得了非常好的效果。在深度学习擅长的问题中，**输入和输出通常都可以表示为固定长度的向量**，如果长度稍有变化，会使用补零等操作

Seq2Seq模型的核心思想是，通过深度神经网络将一个作为输入的序列映射为一个作为输出的序列，这一过程由编码输入与解码输出两个环节构成。

经典实现使用RNN实现，也可使用LSTM

![image](https://github.com/space-zxs/ML-DL/assets/77714764/266c8f12-c5a0-422a-b1c7-b41dbce5407a)

_对应于机器翻译过程，如图10.4所示。输入的序列是一个源语言的句子，有三个单词A、B、C，编码器依次读入A、B、C和结尾符<EOS>。 在解码的第一步，
解码器读入编码器的最终状态，生成第一个目标语言的词W；第二步读入第一步的输出W，生成第二个词X；如此循环，直至输出结尾符<EOS>。输出的序列W、X、Y、Z就是翻译后目标语言的句子。_

_**Seq2Seq模型在解码时，有哪些常用的办法？**_

- Seq2Seq模型最**核心的部分是其解码部分**，大量的改进也是在解码环节衍生
- Seq2Seq模型**最基础的解码方法是贪心法**，即选取一种度量标准后，每次都在当前状态下选择最佳的一个结果，直到结束
- **集束搜索是常见的改进算法**，它是一种启发式算法。该方法会保存beamsize（后面简写为b）个当前的较佳选择，然后解码时每一步根据保存的选择进行下一步扩展和排序，接着选择前b个进行保存，循环迭代，直到结束时选择最佳的一个作为解码的结果

![image](https://github.com/space-zxs/ML-DL/assets/77714764/4bcad691-4cfe-435f-a1fe-cef783d4c5c2)

_**Seq2Seq模型引入注意力机制是为了解决什么问题？为什么选用了双向的循环神经网络模型？**_

_在实际使用中，会发现随着输入序列的增长，模型的性能发生了显著下降。
这是因为编码时输入序列的全部信息压缩到了一个向量表示中。随着序列增长，
句子越前面的词的信息丢失就越严重_

_在注意力机制中，
仍然可以用普通的循环神经网络对输入序列进行编码，得到隐状态h1
,h2…hT。但是
在解码时，每一个输出词都依赖于前一个隐状态以及输入序列每一个对应的隐状
态_

![image](https://github.com/space-zxs/ML-DL/assets/77714764/20b8d129-3979-40d8-b631-f831b69a450a)


---
### 注意力机制
[注意力机制讲解](https://mp.weixin.qq.com/s/R8LROxrPGnZRWfak1IpkoA)

_**Attention思想步骤**_

一个典型的Attention思想包括三部分：Q query、K key、V value。
- Q是query，是输入的信息；key和value成组出现，通常是原始文本等已有的信息；
- 通过计算Q与K之间的相关性a，得出不同的K对输出的重要程度；
- 再与对应的v进行相乘求和，就得到了Q的输出；

attention 计算图解

![image](https://github.com/space-zxs/ML-DL/assets/77714764/c814503c-8789-429c-8203-0832ea86bb5e)

step1，计算Q对每个K的相关性相似性，即函数；
_这里计算相关性的方式有很多种，常见方法比如有：
求两者的【向量点击】，。
求两者的向量【余弦相似度】，。
引入一个额外的神经网络来求值，。
step2，对step1的注意力的分进行归一化；
softmax的好处首先可以将原始计算分值整理成所有元素权重之和为1的概率分布；
其次是可以通过softmax的内在机制更加突出重要元素的权重；
即为value_i对应的权重系数;
step3，根据权重系数对V进行加权求和，即可求出针对Query的Attention数值。_

值得强调的一点是：K和V等价，它俩是一个东西。

_**Self-Attention**__

_self-attention，顾名思义它只关注输入序列元素之间的关系，即每个输入元素都有它自己的Q、K、V，比如在一般任务的Encoder-Decoder框架中，输入**Source和输出Target内容是不一样的**，比如对于英-中机器翻译来说，Source是英文句子，Target是对应的翻译出的中文句子，Attention机制发生在Target的元素Query和Source中的所有元素之间。而Self Attention指的不是Target和Source之间的Attention机制，而是Source内部元素之间或者Target内部元素之间发生的Attention机制，也可以理解为**Target=Source**这种特殊情况下的注意力计算机制。_

![image](https://github.com/space-zxs/ML-DL/assets/77714764/76d686ce-a207-4482-afa2-11d7c4190f32)

_** Self-Attention的计算步骤**_

论文原文中的实现


![image](https://github.com/space-zxs/ML-DL/assets/77714764/614374eb-9839-46c6-80ac-fff80e8cefbf)

![image](https://github.com/space-zxs/ML-DL/assets/77714764/5466f34a-0291-47f8-846f-241cf765b642)

可以理解为：self-Attention中的Q是对自身（self）输入的变换，而在传统的Attention中，Q来自于外部。

![image](https://github.com/space-zxs/ML-DL/assets/77714764/8b4506f7-e2d7-4708-9f02-070ff5bb1cac)


那么整个self-attention的计算过程可以如下：

![image](https://github.com/space-zxs/ML-DL/assets/77714764/09c5a99d-d432-46f6-9427-4eb4627ec978)

![image](https://github.com/space-zxs/ML-DL/assets/77714764/c78a66c0-3b95-4398-84b1-fabf03a6ffc7)

Q、K、V的矩阵计算示意图如下：

![image](https://github.com/space-zxs/ML-DL/assets/77714764/1f58f5ac-c26b-40af-9098-fc8ff3ebc4d0)

_** 根据代码进一步理解Q、K、V**_


![image](https://github.com/space-zxs/ML-DL/assets/77714764/d4e2dc80-d2ba-483f-9169-14e578ad084e)

假设三种操作的输入都是同一个矩阵，这里暂且定为长度为L的句子，每个token的特征维度是768，那么输入就是（L, 768），每一行就是一个字，像这样：

![image](https://github.com/space-zxs/ML-DL/assets/77714764/491f7f6d-978f-49a5-b9a0-7f5d0d7369e6)

乘以上面三种操作就得到了Q、K、V，(L, 768)*(768,768) = (L,768)，维度其实没变，即此刻的Q、K、V分别为：

![image](https://github.com/space-zxs/ML-DL/assets/77714764/2d86d3b9-4f89-4d75-b7c0-00b3bca345a5)

![image](https://github.com/space-zxs/ML-DL/assets/77714764/11ac3946-ebc2-4590-9339-8bb60982edd1)

然后根据缩放点积公式进行打分；

![image](https://github.com/space-zxs/ML-DL/assets/77714764/bf667385-70f0-4ea1-ae67-0cb2229e4fdf)

![image](https://github.com/space-zxs/ML-DL/assets/77714764/24b6c9ce-0d3f-4d8e-96aa-8f298e0a5dd8)

首先用Q的第一行，即“我”字的768特征和K中“我”字的768为特征点乘求和，得到输出（0，0）位置的数值，这个数值就代表了“我想吃酸菜鱼”中“我”字对“我”字的注意力权重，
然后显而易见输出的第一行就是“我”字对“我想吃酸菜鱼”里面每个字的注意力权重；整个结果自然就是“我想吃酸菜鱼”里面每个字对其它字（包括自己）的注意力权重（就是一个数值）了。

2 然后是除以根号dim，这个dim就是768，至于为什么要除以这个数值？主要是为了缩小点积范围，确保softmax梯度稳定性。

然后就是刚才的注意力权重和V矩阵乘了，如图：

![image](https://github.com/space-zxs/ML-DL/assets/77714764/007a59d7-42ab-4eef-83a0-7ff8822a74cf)

首先是“我”这个字对“我想吃酸菜鱼”这句话里面每个字的注意力权重，和V中“我想吃酸菜鱼”里面每个字的第一维特征进行相乘再求和，这个过程其实就相当于用每个字的权重对每个字的特征进行加权求和，然后再用“我”这个字对对“我想吃酸菜鱼”这句话里面每个字的注意力权重和V中“我想吃酸菜鱼”里面每个字的第二维特征进行相乘再求和，依次类推最终也就得到了（L,768）的结果矩阵，和输入保持一致。

![image](https://github.com/space-zxs/ML-DL/assets/77714764/35756a31-5af6-44a9-a5e7-41e5412c0e65)

**注意力机制是没有位置信息的，所以需要引入位置编码，下一篇transformer中会讲解。**

_**Multi-Head Attention**_


在理解了self-attention之后，对于多头注意力的理解就很简单了。多头注意力机制，**是在自注意力的基础上，使用多种变换生成的Q、K、V进行计算，再将它们对相关性的结论综合起来**，进一步增强自注意力的效果。

![image](https://github.com/space-zxs/ML-DL/assets/77714764/d6d05ee7-8d36-4554-9b6c-224bbc072a36)

进一步的，multi-head attention相当于h个不同的self-attention的集成，说白了就是对其的简单堆叠，在这里以h=8举例说明

![image](https://github.com/space-zxs/ML-DL/assets/77714764/c8c4263b-67cd-4dd2-8b92-69614879faef)

本文没提，self-attention和multi-head attention之后都用了残差网络的跳跃连接。

![image](https://github.com/space-zxs/ML-DL/assets/77714764/6a07c394-73cc-4e38-9a70-c8a67a6b190d)

---

### Transformer

[Transformer讲解](https://mp.weixin.qq.com/s/wjhtrMuonlQjiWEUt_Gwcg)


_** 摘要部分**_

  1、说明了transformer的架构，没有使用之前的rnn或者cnn模块二十提出了一种纯基于注意力的新模型架构
  2、论文的实验是基于两个机器翻译任务，从德语到英语，取得了不错的分数
 
 _**结论部分** _
    一个完全基于注意力的序列模型，使用多头注意力替换了传统的基于rnn的架构
    
 _**模型结构图** _
 
![image](https://github.com/space-zxs/ML-DL/assets/77714764/d8849731-4b63-4f93-9761-c5e917f0222f)

rnn的对比和缺点，不能并行容易梯度消失，忘记开始学的信息

编码器和解码器

对于输入（x1，x2，x3.....） 经过编码器得到(z1,z2,z3....zn)，每一个x代表一个词或一个句子，每一个z代表他的表示向量.

把z传入解码器，得到输出序列（y1，y2，y3.。。。）, 对于每一步来说都是一个自回归的过程，用先前生成的字符作为一个加和作为下一个输出的输入。

  一个transformer解码器块有6层，每一层有两个子层，一个多头注意力层一个全连接层，输出的维度是512，总体来说只有两个参数可以调，块数n和输出维度512
  一个transformer编码器块有6层，但是插入了一个第三层mask层，保证预测的时候不会看到后面的句子
 


### Transformer 面试问题汇总

1. Transformer 结构分拆
   ![image](https://github.com/space-zxs/ML-DL/assets/77714764/0464cbf2-15df-4253-aa08-6b52ff25849a)

2.transformer中的缩放点积注意力为什么要除以根号d

在两个向量维度非常大的时候，点乘结果的方差也会很大，即结果中的元素差距很大，在点乘的值非常大的时候，softmax的梯度会趋近于0，也就是梯度消失。在原文中有提到，假设q和k的元素是相互独立维度为dk的随机变量，它们的均值是0，方差为1，那么q和k的点乘的平均值为0，方差为dk，如果将点乘的结果进行缩放操作，也就是除以dk，就可以有效控制方差从dk回到1，也就是有效控制梯度消失问题。

回去看原论文。作者认为，当d较大时，点积的幅度也就变大，容易进入softmax函数的梯度消失区域。

什么叫点积的幅度变大？这里作者指的应该是方差会变大。点积之后，数据的方差会改变，不等同于原分布。这就需要我们进行一个操作使方差等同于原分布。假设原分布是标准正态分布，那为什么需要除以根号d来归一化到标准正态分布呢？我尝试自己推导一下公式：

![image](https://github.com/space-zxs/ML-DL/assets/77714764/df8d48e8-b451-4b07-a7b2-b56cb19148ac)

综上，除以维度的算术平方根会使得分布重新归一化到数据原分布，防止出现梯度消失。

[transformer面试问题](https://mp.weixin.qq.com/s/wjhtrMuonlQjiWEUt_Gwcg)

### bert 

[bert讲解](https://mp.weixin.qq.com/s/HRXoZBtupj74cyRnsfFc-Q)


