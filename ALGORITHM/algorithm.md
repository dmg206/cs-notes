**排序**

**冒泡，直接排序，快速排序是比较重要的**

内排序：待排序的所有记录都放在内存中

外排序：由于排序的记录太多，排序过程需要在内外存之间多次数据交换。

typedef struct

{

​	int r[MAXSIZE+1];	/* 用于存储要排序数组，r[0]用作哨兵或临时变量 */

​	int length;			/* 用于记录顺序表的长度 */

}SqList;



\1. 冒泡排序

基本思想：两两比较相邻记录的关键字，如果反序则交换，直到没有反序的记录为止。





/* 对顺序表L作冒泡排序 */

void BubbleSort(SqList *L)

{ 

​	int i,j;

​	for(i=1;i<L->length;i++)

​	{

​		for(j=L->length-1;j>=i;j--)  /* 注意j是从后往前循环 */

​		{

​			if(L->r[j]>L->r[j+1]) /* 若前者大于后者（注意这里与上一算法的差异）*/

​			{

​				 swap(L,j,j+1);/* 交换L->r[j]与L->r[j+1]的值 */

​			}

​		}

​	}

}



/* 对顺序表L作改进冒泡算法 */

void BubbleSort2(SqList *L)

{ 

​	int i,j;

​	**Status flag=TRUE;**			/* flag用来作为标记 */

 /* 若flag为true说明有过数据交换，否则说明数据已经排序后，FALSE则停止循环 */

​	for(i=1;i<L->length && flag;i++)

​	{

​		**flag=FALSE;**				/* 初始为False */

​		for(j=L->length-1;j>=i;j--)

​		{

​			if(L->r[j]>L->r[j+1])

​			{

​				 swap(L,j,j+1);	/* 交换L->r[j]与L->r[j+1]的值 */

​				 **flag=TRUE;**		/* 如果有数据交换，则flag为true */

​			}

​		}

​	}

}

当flag为false，说明队列已经排好序了，不需要再进行排序

可以减少有序的情况下，无意义的循环。



def bubble_sort(lists):

 \# 冒泡排序

​    	count = len(lists)

​    	for i in range(0, count):

​        	for j in range(i + 1, count):

​            		if lists[i] > lists[j]:

​                		lists[i], lists[j] = lists[j], lists[i]

​    	return lists



冒泡法时间复杂度是O（n*n）

2, 选择排序

基本思想：从i开始循环，在[i+1,n]之间进行n-i次关键字间的比较，从n-i+1个记录中选出关键字最小的记录，并和第i个记录交换。

时间复杂度：

比较次数与冒泡法一样，是n(n-1)/2，但交换次数最坏情况是n-1次。总的时间复杂度依然是O（n*n)

3, 直接插入排序

将一个记录插入到已经排好序的有序表中，从而得到一个新的记录新增1的有序表

时间复杂度：O（n*n），但是性能比冒泡法和简单排序法性能好一些。

\4. 希尔排序

也就是分组插入排序

该方法的基本思想是：先将整个待排元素序列分割成若干个子序列（由相隔某个“增量”的元素组成的）分别进行直接插入排序，然后依次缩减增量再进行排序，待整个序列中的元素基本有序（增量足够小）时，再对全体元素进行一次直接插入排序。因为直接插入排序在元素基本有序的情况下（接近最好情况），效率是很高的，因此希尔排序在时间效率上比前两种方法有较大提高。

希尔排序的时间复杂度：

![img](https:////note.youdao.com/src/D1FE57FDAFA848378ABF959760E0F3F9)

\5. 堆排序（简单选择排序的改进）

完全二叉树：只有最下面的两层结点度能够小于2，并且最下面一层的结点都集中在该层最左边的若干位置的二叉树

堆是具有下列性质的完全二叉树：每个结点都大于或者等于左右结点的值，称为大顶堆（左图），或者每个结点的值都小于或者等于其左右孩子的结点，称为小顶堆（右图）。

![img](https:////note.youdao.com/src/CB6927CB52CA4415BD26C4A3C41C7AE0)



堆排序基本思想：将待排序的序列构造成一个大顶堆。此时，整个序列的最大值就是堆顶的根结点。将它与堆数组末尾元素交换，然后将剩余的n-1个序列构造成一个堆，这样就会得到n个元素的次小值。如此反复。

时间复杂度：O（nlogn）

由于初始构建堆所需的比较次数比较多，因此不适合待排序序列个数较少的情况。



\6. 归并排序（分治算法）

归并排序原理是假设初始序列含有n个记录，则可以看成是n个有序的子序列，每个子序列的长度为1，然后两两合并，得到[n/2]个长度为2或1的有序子序列；再两两归并，如此重复，直到得到一个长度为n的有序序列为止，这种排序方法称为2路归并排序。

归并算法是一种比较耗内存，但效率高且稳定的算法，时间复杂度为O(nlogn)





\#子排序，两个序列进行合并

**def merge(left,right):**

​	result=[]

​	i,j=0,0

​	while i<len(left) and j<len(right):

​		if left[i]<=right[j]:

​			result.append(left[i])

​			i+=1

​		else:

​			result.append(right[j])

​			j+=1

​	result+=left[i:]

​	result+=right[j:]

​	return result



\# 将一个序列进行排序

**def mergesort(seq):**

​	if len(seq)<=1:

​		return seq

​	mid=int(len(seq)/2)

​	left=mergesort(seq[:mid])

​	right=mergesort(seq[mid:])

​	return merge(left,right)

if __name__=='__main__':

​	seq=[4,5,7,9,7,5,1,0,7,-2,3,-99,6]

​	print(mergesort(seq))

​	



\7. 快速排序（分治算法）

基本思想：通过一趟排序将待排记录分割成独立的两部分，其中一部分记录的关键字均比另一部分记录的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序的目的。

分割过程：选取一个key，从两端往中间进行扫描，将比key大的放在右边，比key小的放在左边。直到完全满足条件。

时间复杂度：最优的情况下，时间复杂度为O（nlogn）。最坏情况的时间复杂度为O(n*n)。平均复杂度为O（nlogn）

快速排序算法是一种不稳定的排序方法。

在数据量比较小的时候，快速排序反而不如直接插入排序，因为快速排序用到了递归操作，



**def sub_sort(array,low,high):**     	key = array[low]     	while low < high:

​        	# scan high         	while low < high and array[high] >= key:             		high -= 1

​        	if low<high:

​    			array[low] = array[high]

​       	# scan low         while low < high and array[low] < key:          	low += 1

​       	if low<high:             	 array[high] = array[low]

\#  排序完key补在中间，key的索引 为low     	array[low] = key     	return low

 **def quick_sort(array,low,high):**      if low < high:         key_index = sub_sort(array,low,high)         quick_sort(array,low,key_index)         quick_sort(array,key_index+1,high) if __name__ == '__main__':     array = [8,10,9,6,4,16,5,13,26,18,2,45,34,23,1,7,3]     print array     quick_sort(array,0,len(array)-1)     print array



排序总结:

![image-20190518092942755](/Users/leon/study/github/cs-notes/ALGORITHM/image-20190518092942755.png)



从算法的简单性来看，我们将7种算法分为两类：

简单算法：冒泡，简单选择，直接插入

改进算法：希尔，堆，归并，快速

![img](https:////note.youdao.com/src/52B818A970C34A4FB421F698CC1E4D93)

