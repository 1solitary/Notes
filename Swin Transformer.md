# 缺点和前沿
二、Transformer带来的成本增加是巨大的
虽然transformer结构会对CV任务带来一定的精度提升，但同时，使用transformer也需要付出一定代价。仅举几例如下：
1、Transformer模型巨大的参数量将成倍地增加服务器成本以及随之而来的运维难度。
2、Transformer在小数据集上很难训练，极易过拟合，因此训练所需的数据量显著增加。并且，有些业务场景天然难以提供大量数据。
3、Transformer的训练时间长，相比CNN，transformer有时甚至需要3~5倍的训练时间，这一方面阻碍了许多需要快速响应的需求的开发（例如某些热点事件的跟进），同时也隐形地增加了公司的人力成本（阿里的算法工程师工资可是很高的哟）。

- 虽然在几个常用benchmark上提升明显，但是在某些实际项目的落地中发现可能还不如vit好用，或者需要精细调参，这个在resnet中是不存在的；

- 整个模型设计，我认为并没有真正从原理上真正解决cnn和vit之间的问题。transformer放在cv两个问题，local feat无法专注，长序列导致显存炸裂。swin采用的local self attention等于是套着cnn的模式到了transformer中，后面接一个shift window，我理想当中应该是采用一种全新的attention设计去解决这两个问题，我甚至有点好奇把local self attention替换一个普通卷积会怎么样。再者就是shift window那一步略显复杂，毕竟simple is best。

- 计算损耗就不多说了，显存这块要求是很高的，平民组不知道能不能玩。
两张1080ti跑的 大概一晚上八九个小时

这些成本增加能否接受，则取决于应用该技术的业务是否具有足够的价值了。国内各大互联网公司中，都有transformer的落地先例，例如阿里的搜索推荐、美团的搜索排序、腾讯的广告推荐等。这些无一不是能够带来巨大商业价值的应用场景，在其中取得很小的精度提升，就可以cover相应的改造成本。能否找到类似价值体量的应用场景，也许是transformer能否顺利落地在CV领域的关键。

# 优势
- 首先，transformer对long sequence的建模优势就不说了，CNN在这块是天然的劣势。但transformer目前带来的精度提升，既来自self-attention的理论优势，也不能否认模型参数规模的贡献。
- Transformer真的像变形金刚，只要数据到位、训练到位，模型泛化效果超乎想象。

# 前景
这点我觉得应该没人能黑的了，我后续会更新Swin在各大CV赛事上的表现，这里先列举刚出的Google地标检索和识别2021两大竞赛结果（写完这个，俺赶紧去吃午饭了，之后有时间再更），这两个竞赛真滴卷，Kaggle前10名有一半大佬都打了这两个赛事，你再看看Top的解决方案：Google Landmark Recognition 2021：
前三名方案均用了Swin TransformerGoogle 
Landmark Retrieval 2021: 第一名、第三名方案均用了Swin Transformer

# 实验
top1就是你预测的label取最后概率向量里面最大的那一个作为预测结果，你的预测结果中概率最大的那个类必须是正确类别才算预测正确。而top5就是最后概率向量最大的前五名中出现了正确概率即为预测正确。

# 作者介绍
微软亚洲研究院MSRA（ai届黄埔军校)
1. 传承。从孙老大、恺明、夷晨、季峰、祥雨、锡洲等等，到胡瀚、张拯和我，在组内传承的是科研taste的培养和科研素质的训练，包括如何产生一个好idea并把它做work、内部讨论时平等激烈乃至对工作challenge到极致、对实验solid程度的近乎苛求、对写作逻辑与细节的把控和质量的要求等等。这个过程像是model distillation

# 前言
“统一性”是很多学科共同追求的目标，例如在物理学领域，科学家们追求的大统一，就是希望用单独一种理论来解释力与力之间的相互作用。人工智能领域自然也存在着关于“统一性”的目标。在深度学习的浪潮中，人工智能领域已经朝着统一性的目标前进了一大步。比如，一个新的任务基本都会遵循同样的流程对新数据进行预测：收集数据，做标注，定义网络结构，训练网络参数。

但是，在人工智能的不同子领域中，基本建模的方式各种各样，并不统一，例如：在自然语言处理（NLP）领域目前的主导建模网络是 Transformer；计算机视觉（CV）领域很长一段时间的主导网络是卷积神经网络（CNN）；社交网络领域目前的主导网络则是图网络等。

尽管如此，从2020年年底开始，Transformer 还是在 CV 领域中展现了革命性的性能提升。这就表明 CV 和 NLP 有望统一在 Transformer 结构之下。这一趋势对于两个领域的发展来说有很多好处：1）使视觉和语言的联合建模更容易；2）两个领域的建模和学习经验可以深度共享，从而加快各自领域的进展。
[![IVNZXd.png](https://z3.ax1x.com/2021/11/03/IVNZXd.png)](https://imgtu.com/i/IVNZXd)
# iccv
ICCV是计算机视觉领域最高级别的会议，会议的论文集代表了计算机视觉领域最新的发展方向和水平.
ICCV 的全称是 IEEE International Conference on Computer Vision，即国际计算机视觉大会，是公认的三个会议中级别最高的。一般会在科研实力较强的国家举行，每两年召开一次。

> transformer技术用在图像上，到底怎么把这个图像（通常来说是一个类似三维的数据变成一个类似二维的数据）。目前用的方法都是比较简单暴力的，就比如说我把我的图像压扁。但是，为什么能这么做呢？因为有一个positional encoding，这样就可以把位置的信息保存下来了。现在就可以随意操作了，不用像cnn担心只能对原图相关的区域进行整合。其实原理是比较简单的，本质就是在考虑不同的东西之间的相关性，从而达到我可以通过去看距离比较远的或者时序信号中比较远的信号，与我当前信号的相关性。

# 对比
图像数据的 Inductive bias

Local prior

图像具有 locality（局域性），例如一个像素与它邻近的像素更相关，与它远离的像素更不相关。

Global capacity

图像还具有 long-rangeS dependencies，例如一个像素与距离更远的像素同属于一个物体的相关性。

Positional prior

有些图像有 positional prior，例如人脸图片中，脸一般在图像中间位置，眼睛总是在脸部上方，嘴巴总是在脸部下方。

CNN 通过手工设计卷积核尺寸、个数、stride 等超参数，进而在数据上自动学习卷积核参数。CNN 具有捕捉 local prior 的能力，在图像识别任务中取得成功。但是传统的 CNN 只能通过加深卷积层数、增大感受野来建模 long-range dependencies。这种建模 long-range dependencies 的模式效率较低，并且可能导致优化困难。

Transformer 中的 self-attention，具有 global capacity 和 positional prior，MLP-Blocks 具有 global capacity。但是由于 Vision Transformer 不具有建模 local prior 的能力，因此需要大量的训练数据。最近这些基于 Transformer 的研究表明，更长的训练时间、更多的参数、更多的数据和或更多的正则化，就足以恢复像 ImageNet 分类这样复杂任务的重要先验。

CNN遮挡30%基本就凉了，Transformer甚至能搞定擦除50%的数据
- transformer
解决这些不同的任务，从模型角度来讲什么最重要？是特征抽取器的能力。尤其是深度学习流行开来后，这一点更凸显出来。因为深度学习最大的优点是「端到端（end to end）」，当然这里不是指的从客户端到云端，意思是以前研发人员得考虑设计抽取哪些特征，而端到端时代后，这些你完全不用管，把原始输入扔给好的特征抽取器，它自己会把有用的特征抽取出来。


之前计算机视觉相关的任务主要被CNN所统治。
·从AlexNet及其在ImageNet图像分类挑战方面的革命性表现，CNN架构已经通过更大的规模，更广泛的连接，以及更复杂的卷积形式而逐渐壮大。
自然语言处理（NLP）中网络体系结构的演变走了一条不同的道路，今天流行的体系结构取而代之的是Transformer。
·Transformer是为序列建模和转换任务而设计的，因为它关注数据中的长期依赖性建模。它在语言领域的巨大成功促使研究人员研究它对计算机视觉的适应性，最近它在某些任务上显示了不错的结果，特别是图像分类和联合视觉语言建模。
# 亮点
作者分析表明，Transformer从NLP迁移到CV上没有大放异彩主要有两点原因：

1. 两个领域涉及的scale不同，NLP的scale是标准固定的，而CV的scale变化范围非常大。

2. CV比起NLP需要更大的分辨率，而且CV中使用Transformer的计算复杂度是图像尺度的平方，这会导致计算量过于庞大。

为了解决这两个问题，Swin Transformer相比之前的ViT做了两个改进：

1.引入CNN中常用的层次化构建方式构建层次化Transformer

2.引入locality思想，对无重合的window区域内进行self-attention计算。

- 细节理解
窗口不变，每天学一章，每天任务量不变，随着时间推移，可以把所有学完。也就是最终可以捕捉到所有的感受野，全局的所有信息。
vit从始至终就保持全局感受野，导致显存爆炸，时间复杂度，空间复杂度均是n2。
vit分类
swt分割，目标检测
首先一张图片里面全是像素点组成(256 * 256)，经过卷积(k:4,s:4)之后的特征图(64, 64)由一个一个的patch组成，每个window由window size 为n的n*n个patch组成，这样patch resolution就是 64 / n

# 实验
AP（平均精度）

AP50（即，以0.5 为IoU临界值估计出平均准确度)

Mask-RCNN校验结果可以通过计算mAP值得到一个数值的衡量，在10张图片上计算平均值，增加更高的准确性。

mAP值的计算
P：precision，即准确率；

R：recall，即 召回率。

PR曲线：即以precision和recall作为纵、横轴坐标的二维曲线。

AP值：Average Precision，即平均精确度。

mAP值：Mean Average Precision，即平均AP值；是对多个验证集个体求平均AP值。
Box AP用于综合评价目标检测模型效能
Mask AP用于综合评价实例分割模型效能
# 细节理解
patch - 一个个小的灰色的，定义的为4*4个pixel
window - 一个window你里面有4*4个patch
token：[batch，seq_len，embedding]   
怎么合成的说一下

将注意力计算限制在一个窗口中，一方面能引入 CNN 卷积操作的局部性，另一方面能节省计算量。
```python


if self.shift_size > 0:
            # calculate attention mask for SW-MSA
            H, W = self.input_resolution
            img_mask = torch.zeros((1, H, W, 1))  # 1 H W 1
            h_slices = (slice(0, -self.window_size),
                        slice(-self.window_size, -self.shift_size),
                        slice(-self.shift_size, None))
            w_slices = (slice(0, -self.window_size),
                        slice(-self.window_size, -self.shift_size),
                        slice(-self.shift_size, None))
            cnt = 0
            for h in h_slices:
                for w in w_slices:
                    img_mask[:, h, w, :] = cnt
                    cnt += 1

            mask_windows = window_partition(img_mask, self.window_size)  # nW, window_size, window_size, 1
            mask_windows = mask_windows.view(-1, self.window_size * self.window_size)
            attn_mask = mask_windows.unsqueeze(1) - mask_windows.unsqueeze(2)
            attn_mask = attn_mask.masked_fill(attn_mask != 0, float(-100.0)).masked_fill(attn_mask == 0, float(0.0))
```