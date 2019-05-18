- Portrait Lighting Transfer Using a Mass Transport Approach



Reformulating Color Histogram Transfer

改进的颜色直方图传输

![image-20190518092041430](/Users/leon/study/github/cs-notes/paper/image-20190518092041430.png)



||ci-cj||表示color i到 color j的距离，Tij表示像素中color i到color j的比例。   mass-transport问题转化为求最小值问题。

![image-20190518092130700](/Users/leon/study/github/cs-notes/paper/image-20190518092130700.png)

Incorporating Positions and Normals

同样的颜色在不同的部位，比如forehead和在cheek中如果都映射到同一个颜色，这将导致不能捕捉到光线变化对于集合形状的依赖性。

所以需要考虑postion和normal。s=(c,p,n) 其中c为color，p为position，n为normal

![image-20190518092228269](/Users/leon/study/github/cs-notes/paper/image-20190518092228269.png)

Solving the Mass-Trasport Problem

the Wasserstein distance,这个距离就是Wasserstein距离，又名铲土距离。

有两堆泥土，每一堆有n个位置，标号从1~n。第一堆泥土的第i个位置有ai克泥土，第二堆泥土的第i个位置有bi克泥土。小埃可以在第一堆泥土中任意移挪动泥土，具体地从第i个位置移动k克泥土到第j个位置，但是会消耗的体力。小埃的最终目的是通过在第一堆中挪动泥土，使得第一堆泥土最终的形态和第二堆相同，也就是ai=bi (1<=i<=n), 但是要求所花费的体力最小

**基于人物肖像的自动抠图算法**

![image-20190518092336730](/Users/leon/study/github/cs-notes/paper/image-20190518092336730.png)