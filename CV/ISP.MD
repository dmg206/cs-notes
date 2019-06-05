### 图像处理

#### 拉普拉斯算子

laplace定义：

![laplace](/Users/leon/study/github/cs-notes/CV/resource/img/laplace.jpg)

laplace离散形式：

![laplace-1](/Users/leon/study/github/cs-notes/CV/resource/img/laplace-1.jpg)

图像锐化处理的作用是使灰度反差增强，从而使模糊图像变得更加清晰。图像模糊的实质就是图像受到平均运算或积分运算，因此可以对图像进行逆运算，如微分运算能够突出图像细节，使图像变得更为清晰。由于拉普拉斯是一种微分算子，它的应用可增强图像中灰度突变的区域，减弱灰度的缓慢变化区域。因此，锐化处理可选择拉普拉斯算子对原图像进行处理，产生描述灰度突变的图像，再将拉普拉斯图像与原始图像叠加而产生锐化图像。拉普拉斯锐化的基本方法可以由下式表示：

![laplace-2](/Users/leon/study/github/cs-notes/CV/resource/img/laplace-2.jpg)





Ｙ．Ｂｏｙｋｏｖ等人提出了基于图割理论的ＧｒａｐｈＣｕｔｓ［Ｍｉ５］交互式算法，首次将图论思

想应用到了图像分割方面，大大提高了抠图算法的效率，简化了交互方式。该方法将图像看成为一个图Ｖ是图中所有的像素节点，Ｅ是连接相邻节点间的边。该分割方法将图像分割看成为一个二元标记问题，对于图中每个节点，都有唯一的标记与之相对应，通过求解Ｇｉｂｂｓ能量的最小值来获得点的标记集合，该算法的理论思想将在第２章中进行详细说明，很多算法都在Ｇｒａｐｈ Ｃｕｔｓ算法的基础上做了进一步扩展，例如引入了几何线索形状线索空间线索＂＂，以及拓扑学先验信息［“］作为Ｇｒａｐｈ Ｃｕｔｓ框架下的全局约束。



#### 点检测

点检测采用一个拉普拉斯模板

1  1 1

1 -8 1

1  1 1

#### 线检测

![image-20190604210545743](/Users/leon/study/github/cs-notes/CV/resource/img/image-20190604210545743.png)

需要检测特定方向的直线时，采用特定方向的模板，并对其输出进行阈值处理，留下的点具有最强响应点

#### 边缘检测

梯度算子：Prewitt, Roberts,Sobel

![image-20190604210920326](/Users/leon/study/github/cs-notes/CV/resource/img/image-20190604210920326.png)

   Roberts算子：边缘定位准，但是对噪声敏感。适用于边缘明显且噪声较少的图像分割。Roberts边缘检测算子是一种利用局部差分算子寻找边缘的算子,Robert算子图像处理后结果边缘不是很平滑。经分析，由于Robert算子通常会在图像边缘附近的区域内产生较宽的响应，故采用上述算子检测的边缘图像常需做细化处理，边缘定位的精度不是很高。

​    Prewitt算子：对噪声有抑制作用，抑制噪声的原理是通过像素平均，但是像素平均相当于对图像的低通滤波，所以Prewitt算子对边缘的定位不如Roberts算子。

Sobel算子：Sobel算子和Prewitt算子都是加权平均，但是Sobel算子认为，邻域的像素对当前像素产生的影响不是等价的，所以距离不同的像素具有不同的权值，对算子结果产生的影响也不同。一般来说，距离越远，产生的影响越小。

​    Isotropic Sobel算子：加权平均算子，权值反比于邻点与中心点的距离，当沿不同方向检测边缘时梯度幅度一致，就是通常所说的各向同性。

grabcut：

Ｃａｒｓｔｅｎ Ｒｏｔｈｅｒ等在Ｇｒａｐｈ Ｃｕｔｓ算法的基础上设计提出了ＧｒａｂＣｕｔ算法［２＂］。主要在以下三个方面进行了改进：第一，ＧｒａｂＣｕｔ算法处理的是彩色图像，该算法在描述前、背景像素分布时采用的是高斯混合模型；第二，高斯混合模型中的各个参数是通过几次迭代得到的，而不是一步得到结果；第三，在进行交互时，用户使用的是不完全标记法，只需要在图像中用一个矩形框硬性地将把前景和背景部分分割开来，大大减少了用户的工作量。近两年很多学者对该算法进行了研究改进，针对Ｇｒａｂ Ｃｕｔ算法对于前景和背景相似度比较高的图像分割效果不佳的现象，２０１３年Ｐｒｉｙａｋａ和Ａｎｕｒａｇ对Ｇｒａｂｃｕｔ算法进行了改进，将其应用到ＨＳＶ颜色空间，该算法在图片的前景和背景具有很高相似度的情况下较原算法而比具有很好地分割效果２０１４年Ｋａｎｇ等提出的基于格网格

掩码和均值漂移的Grab cut算法，该算法能自动获取标识矩形框。

infomation-flow matting

•之前李辉提供了两种matting算法，knn matting和closed-form matting，效果都不太好。

•现在我替换成了IFM(Information-flow matting)算法。这个算法是<http://www.alphamatting.com/>上排名第二的算法。结果比较好。

•IFM 方法通过定义多种像素间的相似度，将已知的alpha值传播到未知的区域。

•IFM虽然和knn matting，closed matting都属于affinity-based methods，但是该方法定义了4种像素间关系（closed-form1种，knn matting2种），并且使用了两种预处理trimap的方法。

•IFM结合了多个算法（包括knn和closed-form）的优点，使得结果优于上述两种算法。

 SIFT算法的主要思想是在尺度空间寻找极值点，然后对极值点进行过滤，找出稳定的特征点。最后在每个稳定的特征点周围提取图像的局部特性，形成局部描述子并将其用在以后的匹配中。SIFT算法是基于Lindeberg的理论解决了尺度不变性的问题，本文会对尺度空间理论做一些介绍。 



SURF (Speeded Up Robust Features)也是一种类似于SIFT的兴趣点检测及描述子算法。其通过Hessian矩阵的行列式来确定兴趣点位置，再根据兴趣点邻域点的Haar小波响应来确定描述子，其描述子大小只有64维（也可以扩展到128维，效果更好），是一种非常优秀的兴趣点检测算法。

**直方图均衡化 **

一副图像中灰度级为rk出现的概率：

![image-20190521231449117](/Users/leon/study/github/cs-notes/CV/resource/img/image-20190521231449117.png)

n为图像中像素的个数，nk是灰度级为rk的像素的个数，L为图像中可能的灰度级总数。

![image-20190521231751920](/Users/leon/study/github/cs-notes/CV/resource/img/image-20190521231751920.png)

将输入图像中灰度级为rk的像素映射到输出中灰度级为sk对应的像素。sk也可以看成是输出函数的概率密度函数。3.3.8给出的变换是直方图均衡化或者直方图线性化。

**hough**

![image-20190522222249276](/Users/leon/study/github/cs-notes/CV/resource/img/image-20190522222249276.png) 

1，计算图像的梯度并对其设置门限得到一副二值图像

2，在极坐标平面内确定再细分

3，对像素高度集中的地方检验其累加器单元的数目

4，检验选择单元中像素之间的关系



侦测直线的霍夫变换[算法](https://zh.wikipedia.org/wiki/%E6%BC%94%E7%AE%97%E6%B3%95)使用一个称作累加器(accumulator)二维的矩阵，来侦测图片中是否有直线可以用方程式{![{\displaystyle r=x\cos \theta +y\sin \theta }](https://wikimedia.org/api/rest_v1/media/math/render/svg/22a90ffe4dc97e5d76230b29c99febac9df6d53e)来描述。 Accumulator矩阵的[维度](https://zh.wikipedia.org/wiki/%E7%B6%AD%E5%BA%A6)等于未知的参数的总数，举例来说，要寻找是否有一条直线，他的参数空间的变数总共有两个{\displaystyle r}![r](https://wikimedia.org/api/rest_v1/media/math/render/svg/0d1ecb613aa2984f0576f70f86650b7c2a132538)和{\displaystyle \theta }![\theta ](https://wikimedia.org/api/rest_v1/media/math/render/svg/6e5ab2664b422d53eb0c7df3b87e1360d75ad9af) ，因此Accumulator矩阵的维度是2。 而这个二维的accumulator矩阵，一个维度代表经过量化(quantized)的{\displaystyle r}![r](https://wikimedia.org/api/rest_v1/media/math/render/svg/0d1ecb613aa2984f0576f70f86650b7c2a132538)，另一个维度则是代表量化的{\displaystyle \theta }![\theta ](https://wikimedia.org/api/rest_v1/media/math/render/svg/6e5ab2664b422d53eb0c7df3b87e1360d75ad9af)，因此每一个矩阵的元素(element)的值，是可以用该元素表示的直线 的数量总和，因此矩阵元素的最大值的意义是，该张图片里出现该元素代表的直线的信心最大。

对于每一个像素(pixel) {\displaystyle (x,y)}![(x, y)](https://wikimedia.org/api/rest_v1/media/math/render/svg/41cf50e4a314ca8e2c30964baa8d26e5be7a9386)与其邻近的点，算法会依据一些证据来判断是否有一条直线通过该像素{\displaystyle (x,y)}![(x, y)](https://wikimedia.org/api/rest_v1/media/math/render/svg/41cf50e4a314ca8e2c30964baa8d26e5be7a9386)与其邻近的点，如果有，算法就会把该条直线的参数 {\displaystyle (r,\theta )}![(r, \theta)](https://wikimedia.org/api/rest_v1/media/math/render/svg/ed8396fdc359fb06c93722137c959e7496e47ed6)所对应到的Accumulator矩阵里的元素增加1，最后在选出Accumulator矩阵里，大于阈值(threshold)的一些局部最大值(local maximum)，就可能找到真地存在于图片上的直线， 在其他状况下，不使用threshold改用其他的技巧可以让算法的表现(performance)更好。然而，霍夫变换只能求得线的参数，无法求得该条线的长度，所以，必须在霍夫变换完的下一步，将线条配对到 图上的线条。而霍夫变换的误差来源，可能是图片的不完美(噪声、遗漏像素)，使得边缘侦测器(edge detector)的侦测出错误的边界。

<https://zh.wikipedia.org/wiki/%E9%9C%8D%E5%A4%AB%E5%8F%98%E6%8D%A2>

### 表示和描述

#### 区域描述子

纹理描述子：灰度级直方图的统计矩

一阶矩是均值表示区域的平均灰度，它仅仅用做灰度的概念，不是真正的纹理

二阶矩是方差，一般将方差转化为标准差，标准差小说明灰度级变化小，即纹理更平滑

三阶矩常用于确定直方图的对称程度和偏斜方向，是左偏斜(负值)还是右偏斜(正值)，这给出了灰度级是

#### 使用主分量进行描述

图像配准是使用某种方法，基于某种评估标准，将一副或多副图片（局部）最优映射到目标图片上的方法。

**x** 为n维列向量随机变量，求取该随机变量的协方差矩阵Cx，令**A**为一个矩阵，其行右Cx组成，特征向量的排序方式为**A** 的第一行对应于最大特征向量，最后一行对应于最小特征向量。

将向量**x** 映射为向量**y**：

**y=A(x-mx)**     **mx**为均值向量

也可以由y来恢复x：

**x=A.T *y+mx**

![image-20190531153501654](/Users/leon/study/github/cs-notes/CV/resource/img/image-20190531153501654.png)