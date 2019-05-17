### 图像处理

#### 拉普拉斯算子

laplace定义：

![laplace](/Users/leon/study/github/cs-notes/CV/resource/img/laplace.jpg)

laplace离散形式：

![laplace-1](/Users/leon/study/github/cs-notes/CV/resource/img/laplace-1.jpg)

图像锐化处理的作用是使灰度反差增强，从而使模糊图像变得更加清晰。图像模糊的实质就是图像受到平均运算或积分运算，因此可以对图像进行逆运算，如微分运算能够突出图像细节，使图像变得更为清晰。由于拉普拉斯是一种微分算子，它的应用可增强图像中灰度突变的区域，减弱灰度的缓慢变化区域。因此，锐化处理可选择拉普拉斯算子对原图像进行处理，产生描述灰度突变的图像，再将拉普拉斯图像与原始图像叠加而产生锐化图像。拉普拉斯锐化的基本方法可以由下式表示：

![laplace-2](/Users/leon/study/github/cs-notes/CV/resource/img/laplace-2.jpg)

