## CLIP

[github链接](https://github.com/openai/CLIP)

通过一个自然语言的监督，可以训练一个迁移效果很好的视觉模型。

是一个牵扯文字图片的多模态工作。

输入是文本和图片对，通过对比学习的方式


正样本和负样本，对角线上是文本和图像的对齐是正样本，其他是训练的负样本

![image](https://github.com/space-zxs/ML-DL/assets/77714764/1c661069-f316-4cb4-9709-2567ebc61f8a)

[https://www.jianshu.com/p/23623fe17f64] 交叉熵损失函数， 预测正确的时候损失很小，预测错误损失很大

[pytorch交叉熵函数的实现](https://blog.csdn.net/weixin_44211968/article/details/123906631)

文本编码器是一个 transormer的 encoder
