### DALLE 2 

openai 的产品，处于安全等各种原因暂时没有开源。


层级式的方式，比如先生成一个64*64，然后上采样到256*256 .... 1024*1024 高清大图

一个两阶段的模型，prior阶段 根据文本从clip得到图像特征，decoder阶段，根据图像特征生成图像。

decoder阶段是一个扩散模型，prior阶段试了自回归和扩散模型


文本特征->clip->文本rembedding -> prior -> 图像embedding -> decoder


训练的时候使用cLIP的图像特征,当作ground——truth

论文里面叫自己unclip


VAE和Diffusion的区别：

    都可以看做是编码器解码器的结构，只不过扩散模型的编码器是一个固定的过程，而VAE的编码器是一个学习

    对于扩散模型来说，每一层的维度更刚开始的输入都是同样大小，但是对于VAE一般中间的维度会比较小

    对于扩散模型来说有一个time embedding 的概念

classifier guidance 和 classfier free guidance 、

    在扩散模型出来之后生成图像的质量还算不错，但是在inception score 和 fid 等分数指标上比不过gan

    那么这个分类器怎么加的呢

    在扩散模型的反向阶段，加了一个分类器，这个分类器大多都是在imagenet上训练的，只不过把数据集里面加了很多噪声
