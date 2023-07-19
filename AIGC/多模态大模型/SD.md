## sd 原理
## diffusion model 的原理

[一文弄懂扩散模型](https://mp.weixin.qq.com/sbiz=MzA4MjY4NTk0NQ==&mid=2247507103&idx=1&sn=09c6fab1805edb3e266dadb2a17106a7&scene=21#wechat_redirect)

[diffusion 公式推导](https://wrong.wang/blog/20220605-什么是diffusion模型/)


## sd 原理  

[sd原理](https://mp.weixin.qq.com/s/38y_00LgzXg45BZCcIOdtw)

_**sd主要功能**_

- 文生图 (text2img) 上图表示输入文本到生成图像的流程。
- 图生图 (img2img) 下图表示输入文本和图像到生成图像的流程。

_Stable Diffusion 是由多个组件和模型组成的一个系统。并非单一的模型_

SD 构成： 文本编码器、VAE、图像生成器    unet 开始和结束前都要用到VAE

![image](https://github.com/space-zxs/ML-DL/assets/77714764/71e3d648-8f51-4d15-87c6-6ffa451b3bf4)


_**文本编码器**_

使用CLIP多模态模型->文本向量

• ClipText 用于文本编码。
Input:  输入文本。
Output: 77 token embeddings vectors, 每个向量有 768 个维度

_**图像编码器**_

 UNet + Scheduler逐步处理/扩散信息（Latent）空间中的信息。
Input: 文本嵌入和由噪点组成的起始多维矩阵（结构化数字列表，也称为张量Tensor）。
Output: 处理后的信息矩阵

_**Autoencoder Decoder**_

Autoencoder Decoder . 使用处理后的信息矩阵绘制最终图像的自动编码器解码器。
Input: 处理后的信息矩阵，维度: (4,64,64) Output: 生成的图像，维度: (3, 512, 512) 为（红/绿/蓝，宽，高）

![image](https://github.com/space-zxs/ML-DL/assets/77714764/a3f37943-298b-4d6e-a193-c167ef7c753c)


_**扩散作用在哪里**_

_扩散的过程发生在下图中粉红色的图像信息创建器组件中。包含输入文本并通过 CLIP 模型输出的 token 嵌入，和随机的初始图像信息矩阵 (也称之为 latents) ，最终通过图像解码器来绘制最终图像的信息矩阵。_

![image](https://github.com/space-zxs/ML-DL/assets/77714764/3525b148-7d94-430c-ae7e-745f1628fe44)

这个过程以 **step-by-step **的方式运作，每一步都会添加更多相关信息。 

整个扩散过程包含多个 steps，每个 step 都对输入的 Latent 矩阵进行操作，并生成另一个更接近输入文本的 Latent 矩阵，同时从选择模型的图像库中获取视觉信息。

![image](https://github.com/space-zxs/ML-DL/assets/77714764/fa1bba07-0d9d-4f2b-867c-b2854b3be740)

_**扩散的工作原理**_

受热力学的扩散影响，一种马尔可夫链形式的扩散过程

_前向扩散_

为一张图像生成噪音，加到图像中视为一次训练过程




