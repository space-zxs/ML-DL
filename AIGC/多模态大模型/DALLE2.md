### DALLE 2 

openai 的产品，处于安全等各种原因暂时没有开源。


层级式的方式，比如先生成一个64*64，然后上采样到256*256 .... 1024*1024 高清大图

一个两阶段的模型，prior阶段 根据文本从clip得到图像特征，decoder阶段，根据图像特征生成图像。

decoder阶段是一个扩散模型，prior阶段试了自回归和扩散模型


文本特征->clip->文本rembedding -> prior -> 图像embedding -> decoder
