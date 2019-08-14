## 更好也更快！最先进的图像去模糊算法DeblurGAN-v2

原创： CV君 [我爱计算机视觉](javascript:void(0);) *今天*

点击我爱计算机视觉标星，更快获取CVML新技术



------



以GAN为代表的生成模型正在视觉造假的路上越来越成熟，狗变猫、白马变斑马、实景变素描是GAN用于高级图像生成的例证。



能否将GAN应用于低级的图像处理呢？比如图像去模糊。



答案是肯定的。将GAN用于图像去模糊，生成器用于生成清晰图像，鉴别器区分真实且清晰图像与造假或模糊图像。



DeblurGAN （CVPR 2018）是这一方向新出算法中的佼佼者。



昨日公布的ICCV 2019 论文 DeblurGAN-v2: Deblurring (Orders-of-Magnitude) Faster and Better，原作者对其再升级，改进了生成器的网络结构与鉴别器，且使得算法可以方便使用现有成熟的骨干网，不仅提升了去模糊后图像的质量，同时可以轻易设计计算代价小的模型。**实现了更好也更快！**



该文作者信息：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTutxk1icbTTS8gvhoA2sT8fHBqpZg6cFYgtFiccO0rZ9D1Mn2wL3ISlKds055EhCbGXSrb3bj4GCKSg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



由论文标题知，DeblurGAN-v2在速度上获得了数量级的提升。



下图展示了该文描述的DeblurGAN-v2使用不同骨干网获得的三个模型在GoPro数据集上与其他三个SOTA去模糊算法比较结果。



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTutxk1icbTTS8gvhoA2sT8fHcoknTEe0AEb7tuCVV3u1bB2lL5acx1Y7CO4JbI9JxcXe7hLDkeQwibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



可见，使用SSIM为度量标准，DeblurGAN-v2的三个模型计算代价都较低，在使用复杂度高的inception网络时，DeblurGAN-v2可取得最好的去模糊效果，而使用轻量级网络，在FLOPs大幅度下降情况下，SSIM结果仍处于SOTA水平。



因为效果好计算代价小，将 DeblurGAN-v2用于视频去模糊也是可行的！



**算法改进**



下图展示了该文作者对算法的改进：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTutxk1icbTTS8gvhoA2sT8fHbqsYPJwZaATDsbfHaL6avlESIjTwkNcFqCq4yhBa2uo0uR2ibUlpAGQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



在生成器部分作者引入了特征金字塔网络，不同于使用图像金字塔，这种特征重用的结构可大幅降低计算时间和模型size。



且这种结构允许方便的使用不同的CNN骨干网，是一种计算量可伸缩的结果。



另外，在鉴别器部分，作者设计了新的损失函数：

![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTutxk1icbTTS8gvhoA2sT8fHJwVavbVCibzgOXBweTGSjtACngibSPsicAKnxmSLTkHrgclerUb9w6ZNQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

另外，不仅保留DeblurGAN中PatchGAN鉴别器，对图像Patch进行鉴别，还引入了全局鉴别器（如架构图的右侧部分），称此为双尺度鉴别器（double-scale discriminator）。作者发现这样的改进，可以使得DeblurGAN-v2更好的处理较大的和异质的真实世界模糊。



**实验结果**



作者在多个图像去模糊数据集上进行了实验。



在几大数据集上客观评价指标结果：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTutxk1icbTTS8gvhoA2sT8fHpGyH5bGYwrJ1aA4VpX9vMFuLetZicedrS14kIGJqTicGJ2FpTibZpic6BA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTutxk1icbTTS8gvhoA2sT8fH6AtE3btjlJibDMTia3bia4ceQpD2KXT5EjewclyzUibavb2nxYTWDfNgQw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



可见DeblurGAN-v2算法既可以获得最高精度的模型，也可以获得精度接近最好但计算量极低的模型，更加实用。



在Lai数据集上的主观评价结果：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTutxk1icbTTS8gvhoA2sT8fH1dtf7DW1KQqgTb0SibnibSiaFNbZnHayI8icH4POEBQAsOkkFpIqBtrnTw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



在Kohler数据集的去模糊示例：



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTutxk1icbTTS8gvhoA2sT8fH4ciaV1Dq8tELdRsQwB0kOkN0icTa3ICmJ8PUr77qyNYPUdtn7eJZZG5A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTutxk1icbTTS8gvhoA2sT8fHrMV83iaAdic22T5VPL8Xm6RF7VD4oY1061f060OvvocL8ZicoPYws9puw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



![img](https://mmbiz.qpic.cn/mmbiz_png/BJbRvwibeSTutxk1icbTTS8gvhoA2sT8fHkickPq5E1cs6uBiaP2ueLqtvODUDl2r6zict8rLdnkJUefjzZ6TJKX1ZA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



**结论**



本文提出的去模糊框架DeblurGAN-v2可以很方便切换骨干网，能够取得目前最好的效果，而计算量却减少几个数量级，非常值得参考。



值得一提的是，DeblurGAN-v2中生成器的网络结构和双尺度鉴别器也同样适用于其他低级图像处理任务，比如图像超分辨。期待也能对相关领域的研究有所启发。



论文地址：

https://arxiv.org/pdf/1908.03826v1.pdf

在我爱计算机视觉公众号对话界面回复“去模糊GAN”，即可收到下载地址。



代码地址：

https://github.com/TAMU-VITA/DeblurGANv2

感谢开源者，欢迎给作者标Star。