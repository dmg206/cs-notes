###  Alphapose



问题：sppe依赖于目标检测，检测的误差会导致sppe的失败



创新点：提出RMPE

Hi, it is not used in our torch model. We add a parallel SPPE when using the SSTN and it should be discarded during testing period. However, torch doesn't support loading part of the model during testing and this will affect our inference efficiency. Caffe supports loading part of the weights, thus you can find SSTN in our old repo: <https://github.com/MVIG-SJTU/RMPE>, which is implemented in Caffe









### crowdpose

论文的贡献：

1，我觉得文章最主要的地方在于提出target joints和interference joints这个概念,因为以往从来没想过, 都是直接保留target joints, 非target joints就直接抑制掉了.文章保留了这两类点,并且打上不同比重的label, 从而让网络有意的去学习这两类点,最后再通过构建图的方法来求得最优解.

Different from previous methods that only predict target joints for input human proposals, our joint-candidate SPPE outputs a list of candidate locations for each joint. The candidate list includes target and
interference joints. Then our association algorithm utilizes these candidates to build a person-joint connection graph. At last, we solve the joint association problem in this graph model with a global maximum joints association algorithm.

2，为了更好的评估crowed场景下算法的姿态估计效果，收集了一个dataset of crowded human poses

效果：

5.2mAP

方法：

Joint-Candidates SPPE(JC SPPE)

heatmap loss:

![image-20190816232300446](/Users/leon/study/github/cs-notes/CV/resource/image-20190816232300446.png)

u是衰减系数，T目标关节的的heatmap，T目标关节的heatmap,C是干扰关节的heat map

干扰关节在预测其他人的关节将会很有效的。在全局视野上看，可以看成是一种交叉验证。u设置为0.5，传统的heatmap函数可以看成是u=0.

在crowd场景下， human proposals 是高度overlaped的，overlaped的human proposals 将会预测出高一joints，crowdpose通过全局匹配可以解决这一问题

| 开源系统              | 准确率 | 平均速度 |
| :-------------------- | :----- | :------- |
| Openpose（CMU）       | 60 mAP | 12 FPS   |
| Mask-RCNN（Facebook） | 67 mAP | 5 FPS    |
| Alphapose（SJTU）     | 71 mAP | 20 FPS   |



为了在兼顾速度的同时保持精度，**新版AlphaPose提出了一个新的姿态估计模型**。模型的骨架网络使用 ResNet101，同时在其下采样部分添加 SE-block 作为 attention 模块——已经有很多实验证明，在 Pose Estimation 模型中引入 attention 模块能提升模型的性能，而仅在下采样部分添加 SE-block 能使 attention 以更少的计算量发挥更好的效果。

除此之外，使用 **PixelShuffle + Conv** 进行3次上采样，输出关键点的热度图。传统的上采样方法会使用反卷积或双线性插值。而使用 PixelShuffle 的好处在于，在提高分辨率的同时，保持特征信息不丢失。对比双线性插值，运算量低；对比反卷积，则不会出现网格效应。

在系统架构方面，新版 AlphaPose 采用**多级流水**的工作方式，使用多线程协作，将速度发挥到极致。

AlphaPose 系统目前在COCO的 Validation 集上的运行速度是 20FPS（平均每张图片4.6人），精度达到71mAP。 在拥挤场景下（平均每张图片15人），AlphaPose系统速度仍能保持 10FPS 以上。





#### poseflow

alphapose is a pose estimation framework, its results can be directly used in poseflow. 3x faster means the poseflow with orb is faster than poseflow with deepmatching, this speed does not consider the pose estimation process but only take pose trakcing time into consideration. poseflow is not based on pytorch, it contains no deep learning models, you can regard it as a pure module for tracking human poses cross frames.



​    **Parallel SPPE。**为了进一步帮助STN去提取更好的人体区域位置，在训练阶段添加了一个Parallel SPPE分支。这个Paralell SPPE和原来的SPPE用同一个STN模块，和SPPE并行处理时候，忽略SDTN模块。这个分支的人体姿态标签被指定为中心,更准确的说，**SPPE网络的输出直接和人体真实姿态的标签进行对比**。在训练过程中会关闭Parallel SPPE的所有层（？？？？），我们固定这个分支的权重，**其目的是将中心位置的位姿误差反向传播到STN模块**。如果STN提取的姿态不是中心位置，那么Parallel SPPE会返回一个较大的误差。通过这种方式，我们可以帮助STN聚焦在正确的中心位置并提取出高质量的区域位置。Parallel SPPE只有在训练阶段才会产生作用。

​    **Discussions。**Parallel SPPE可以看作是训练阶段的正则化过程，**有助于避免局部最优的情况**（STN不能把姿态转换到提取到人体区域框的居中位置）。但是SDTN的反向修正可以减少网络的错误进而降低陷入局部最优的可能性。这些错误对于训练STN是很有影响的。通过Parallel SPPE，可以提高STN将人体姿态移动到检测框中间的能力。

　　感觉上似乎可以在SPPE的输出后添加一个中心定位姿态的回归损失函数来取代Parallel SPPE。然而，这种方法会降低我们整个系统的性能。尽管STN可以部分修改输入，但是不可能完美的将人定位在标签的位置。SPPE输入与标签坐标空间上的差异，将在很大程度上影响其学习姿态估计的能力，导致我们主分支SPPE的性能下降。因此，为了确保STN和SPPE同时发挥自己的作用，一个固定权重的Parallel SPPE是不可缺少的。Parallel SPPE在不影响主分支SPPE性能的情况下，会对非中心姿态产生较大的误差，从而推动STN产生中心位置的姿态。

<https://blog.csdn.net/m0_37644085/article/details/82622464>





#### pytorch

在PyTorch中，上采样的层被封装在`torch.nn`中的`Vision Layers`里面，一共有4种：

- ① PixelShuffle
- ② Upsample
- ③ UpsamplingNearest2d
- ④ UpsamplingBilinear2d



#### 1.1 PixelShuffle

pixelshuffle算法的实现流程如上图，其实现的功能是：将一个H × W的**低分辨率输入图像**（Low Resolution），通过Sub-pixel操作将其变为rH x rW的**高分辨率图像**（High Resolution）。

定义
该类定义如下：

class torch.nn.PixleShuffle(upscale_factor)
1
1
这里的upscale_factor就是放大的倍数，数据类型为int。
以四维输入(N,C,H,W)为例，Pixelshuffle会将为(∗,r2C r^2Cr 
2
 C,H,W)的Tensor给reshape成(∗,C,rH,rW)的Tensor。形式化地说，它的输入输出的shape如下：

输入: (N,C x upscale_factor ^2 ,H,W)

输出: (N,C,H x upscale_factor,W x upscale_factor)







