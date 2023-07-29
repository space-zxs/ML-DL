## BLIP

[github链接](https://github.com/salesforce/BLIP)

BLIP 的一大贡献在于将**自然语言理解**和**自然语言生成**任务进行融合形成了多模态通用的模型

文本编码器纯纯用了一个bert

模型架构图:

![image](https://github.com/space-zxs/ML-DL/assets/77714764/4a97595a-188b-4eac-bc6f-64f133b9a32e)


[知乎解释](https://zhuanlan.zhihu.com/p/465788919)

ITM 中使用ITC中未分类正确的图文对，进行更细粒度的训练

BLIP的MED的预训练微调被分成两个部分，Captioner 和 Filter， 对于网络的图文对，使用Captioner也就是med的decoder部分生成文本，然后将网络生成的文本和decoder生成的文，构成数据集送到Filter也就是ITM中进行过滤,从而达到微调的目的

[scdn对blip的理解](https://blog.csdn.net/moxibingdao/article/details/122955160)


![image](https://github.com/space-zxs/ML-DL/assets/77714764/0d211648-0e4c-47b1-a48d-0998087f6c40)
