# 堆

底层：数组

### 1. 什么是堆？

> 堆是一种数据结构，它是一颗完全二叉树。

堆分为最大堆和最小堆：

1. 最大堆：任意节点的值不大于其父亲节点的值。
2. 最小堆：任意节点的值不小于其父亲节点的值。

如下图所示，就是个最大堆：

![](<https://img3.mukewang.com/5baa49020001bbf517710748.jpg>)

### 2. 堆有什么用途？

> 堆最常用于优先队列以及相关动态问题。

优先队列指的是元素入队和出队的顺序与时间无关，既不是先进先出，也不是先进后出，而是根据元素的重要性来决定的。

例如，操作系统的任务执行是优先队列。一些情况下，会有新的任务进入，并且之前任务的重要性也会改变或者之前的任务被完成出队。而这个出队、入队的过程利用堆结构，时间复杂度是`O(log2_n)`。

![](<https://img1.mukewang.com/5baa490a00016a7005000275.jpg>)



### 3. 实现堆结构

#### 3.1 元素存储

堆中的元素存储，一般是借用一个数组：**这个数组是从 1 开始计算的**。更方便子节点和父节点的表示。

![](<https://img1.mukewang.com/5baa4915000190bf05000238.jpg>)

#### 3.2 入堆

入堆即向堆中添加新的元素，然后将元素移动到合适的位置，以保证堆的性质。

在入堆的时候，需要`shift_up`操作，如下图所示：

![](<https://img2.mukewang.com/5baa491d000148a405000215.jpg>)

插入 52 元素后，不停将元素和上方父亲元素比较，如果大于，则交换元素；直到达到堆顶或者小于等于父亲元素。

#### 3.3 出堆

出堆只能弹出堆顶元素（最大堆就是最大的元素），调整元素为止保证堆的性质。

在入堆的时候，需要`shift_down`操作，如下图所示：

![](<https://img.mukewang.com/5baa49280001f63405000238.jpg>)

已经取出了堆顶元素，然后将位于最后一个位置的元素放入堆顶（图中是`16`被放入堆顶）。

重新调整元素位置。此时元素应该和子节点比较，如果大于等于子节点或者没有子节点，停止比较；否则，选择子节点中最大的元素，进行交换，执行此步，直到结束。

#### 3.4 实现优化

在优化的时候，有两个部分需要做：

1. `swap`操作应该被替换为：单次赋值，**减少赋值次数**
2. 入堆操作：空间不够的时候，应该开辟 2 倍空间，**防止数组溢出**

### 3.5堆排序

根据实现的`MaxHeap`类，实现堆排序很简单：将元素逐步`insert`进入堆，然后再`extract_max`逐个取出即可。当然，这个建堆的平均时间复杂度是`O(n*log2_n)`代码如下：





过程叫做`heapify`,实现思路如下：

1. 将数组的值逐步复制到`this->data`
2. 从第一个非叶子节点开始，执行`shift_down`
3. 重复第 2 步，直到堆顶元素

这种建堆方法的时间复杂度是: `O(n)`。因此, 编写`heap_sort2`函数：

```java
public MaxHeap(Item arr[]) {
		
		int n = arr.length;
		//创建一个可比较类型的数组，作为堆的底层，data从1开始
		data = (Item[])new Comparable[n+1];
		capacity = n;
		
		//在堆中，为了方便计算，从1开始
		for(int i = 0; i<n;i++)
			data[i+1] = arr[i];
		count = n; //堆中元素个数
		
		//heapify过程。找到堆中第一个非叶子节点，作为开始
		for(int i = count/2;i>=1;i--)
			shiftDown(i); 
	}
```

**整体代码实现**

```java
import java.util.*;
public class MaxHeap<Item extends Comparable> {
	
	protected Item[] data; //作为堆的底层的数组
	protected int count;   //堆中元素个数
	protected int capacity;
	
	//构造函数，构建一个空堆，可容纳capacity个元素
	public MaxHeap(int capacity) {
		data = (Item[]) new Comparable[capacity+1];
		count = 0;
		this.capacity = capacity;
	}
	
	//构造函数，通过一个给定数组创建一个最大堆
	public MaxHeap(Item arr[]) {
		
		int n = arr.length;
		//创建一个可比较类型的数组，作为堆的底层，data从1开始
		data = (Item[])new Comparable[n+1];
		capacity = n;
		
		//在堆中，为了方便计算，从1开始
		for(int i = 0; i<n;i++)
			data[i+1] = arr[i];
		count = n; //堆中元素个数
		
		//heapify过程。找到堆中第一个非叶子节点，作为开始
		for(int i = count/2;i>=1;i--)
			shiftDown(i); 
	}
	
	//返回堆中元素个数
	public int size() {
		return count;
	}
	
	//返回一个布尔值，表示堆中是否为空
	public boolean isEmpty() {
		return count == 0;
	}
	
	//向最大堆中插入一个新元素item
	public void insert(Item item) {
		//先判断是否超过容量
		assert count+1 <= capacity;
		data[count+1] = item; //把元素加到末尾
		count++;              //增加count数量
		shiftUp(count);       //把新添加的元素放到其合适的位置
	}
	
	//从最大堆中取出堆顶元素，即堆中所存储的最大数据
	public Item extractMax() {
		//先判断堆中有元素
		assert count>0;
		Item ret = data[1];
		//交换堆顶和末尾元素
		swap(1,count);
		count--;     //减少count的数量
		shiftDown(1);//把放到堆顶的末尾元素放到合适的位置去
		
		return ret;
	}
	
	//获取最大堆中的堆顶元素
	public Item getMax() {
		assert (count>0);
		return data[1];
	}
	
	//交换堆中索引为i和j的两个元素
	private void swap(int i,int j) {
		Item t = data[i];
		data[i] = data[j];
		data[j] = t;
	}
	
	//重要辅助函数
	private void shiftUp(int k) {
		//当前节点k大于母节点k/2，则元素交换。k变成k/2
		while(k>1 && data[k/2].compareTo(data[k])<0) {
			swap(k/2,k);
			k = k/2;
		}
	}
	
	private void shiftDown(int k) {
		while(2*k <= count) {//保证k是有孩子的。k只要有左孩子，证明一点有孩子
			int j = 2*k; //j是k的左孩子
			
			if(j+1<= count && data[j+1].compareTo(data[j])>0)
				j++; //j是k的右孩子
			//保证data[j]是data[2*k]和data[2*k+1]中的最大值
			//这样才能保证换上来的元素是最大的
			if(data[k].compareTo(data[j])>=0)
				break;
			swap(k,j);
			k = j;
	
		}
	}
	// 测试 MaxHeap
    public static void main(String[] args) {

        MaxHeap<Integer> maxHeap = new MaxHeap<Integer>(100);
        int N = 100; // 堆中元素个数
        int M = 100; // 堆中元素取值范围[0, M)
        for( int i = 0 ; i < N ; i ++ )
            maxHeap.insert( new Integer((int)(Math.random() * M)) );

        Integer[] arr = new Integer[N];
        // 将maxheap中的数据逐渐使用extractMax取出来
        // 取出来的顺序应该是按照从大到小的顺序取出来的
        for( int i = 0 ; i < N ; i ++ ){
            arr[i] = maxHeap.extractMax();
            System.out.print(arr[i] + " ");
        }
        System.out.println();

        // 确保arr数组是从大到小排列的
        for( int i = 1 ; i < N ; i ++ )
            assert arr[i-1] >= arr[i];
    }

}
```

Python代码--最大堆的实现

```python
import random
#创建Array类固定长度的数组
#一个下划线开头的实例变量名，比如_name，这样的实例变量外部是可以访问的，
#但是，按照约定俗成的规定，当你看到这样的变量时，意思就是，
# “虽然我可以被访问，但是，请把我视为私有变量，不要随意访问”。

class Array(object):
    def __init__(self,size=32): #默认长度32
        self._size = size      #_size可以被外部访问但是要视为私有变量
        self._items = [None]*size #用*号创建长度为size，元素全为None的列表

    def __getitem__(self,index): #模式方法。实现通过[]进行访问
        return self._items[index]

    def __setitem__(self, index, value): #设置相应位置的元素
        self._items[index] = value

    def __len__(self):  #返回长度值
        return self._size

    def clear(self,value=None):
        for i in range(self._items):
            self._items[i]=value



    def __iter__(self): #遍历数组
        for item in self._items:
            yield item
            
class MaxHeap(object):
    def __init__(self,capacity=None):

        self.capacity = capacity+1 #堆的容量
        self._data = Array(self.capacity) #创建内部数组，用来实现最大堆
        self._count = 0  #堆中元素个数

    def __len__(self):  #最大堆的长度
        return self._count

    def add(self,item): #向堆中添加元素
        if self._count>=self.capacity:
            raise Exception('full')

        self._count += 1
        self._data[self._count] = item
        self.__shiftUp(self._count)

    def size(self):
        return self.count

    def isEmpty(self):
        return self.count ==0

    def extractMax(self):
        if self._count <=0:
            raise Exception("empty")
        ret = self._data[1] #存储顶点元素
        self._data[1],self._data[self._count] = self._data[self._count],self._data[1]
        self._count-=1
        self.__shiftDown(1)
        return ret

    def __shiftUp(self,k):  #
        # 指针k对应节点不是root节点且子节点大于父节点
        while(k>1 and self._data[k]>self._data[k//2]):
            self._data[k//2],self._data[k]=self._data[k],self._data[k//2]
            k //=2

    def __shiftDown(self,k):
        while(2*k<=self._count): #该节点有左孩子（证明肯定有孩子）
            j = k*2
            #如果该节点有右孩子且右孩子更大时，指针指向右孩子
            if(j+1<=self._count and self._data[j+1]>self._data[j]):
                j+=1
            if(self._data[k]>=self._data[j]):
                break
            self._data[k],self._data[j]=self._data[j],self._data[k]
            k=j
            
def test_MaxHeap():
    arr = list(range(33))
    random.shuffle(arr)
    print(arr)
    heap = MaxHeap(len(arr))
    res = []
    for i in range(len(arr)): #将随机生成的arr元素入堆
        heap.add(arr[i])
    for i in range(len(arr)): #堆中元素出堆
        res.append(heap.extractMax())
    print(res) #从大到小
    for i in range(len(arr) // 2): #反转数组
        res[i], res[len(res) - 1 - i] = res[len(res) - 1 - i], res[i]
    print(res)
```





# **数据结构--索引堆**

**何为索引堆？**

索引堆是对堆进行了优化。关于堆的介绍可以查看[数据结构--堆](https://www.yasinshaw.com/article/15)。



**优化了什么？**

在堆中，构建堆、插入、删除过程都需要大量的交换操作。在之前的实现中，进行交换操作是直接交换`datas`数组中两个元素。<font color='red'>而索引堆**交换的是这两个元素的索引，而不是直接交换元素**。</font>



**有什么好处？**

主要有两个好处：

1. 减小交换操作的消耗，尤其是对于元素交换需要很多资源的对象来说，比如大字符串。
2. 可以根据原位置找到元素，即便这个元素已经换了位置。



### **如何做到的？**

索引堆使用了一个新的`int`类型的数组，用于存放索引信息。部分代码如下：

```java
// 属性
T[] datas; // 存放数据的数组 datas[1..n]
int[] indexes; // 索引数组
```

这里这个`indexes`数组，存放的是什么信息呢？它是如何工作的呢？假如我们有这样一个最小堆：

用数组表示是：

> datas: [-, 1, 15, 20, 34, 7]

现在要维护最小堆的有序性，就需要交换`15`和`7`这两个元素。交换之后的元素数组是：

> datas: [-, 1, 7, 20, 34, 15]

而此时，我们再想找到原来在datas[2]位置的元素，已经找不到了。因为此时data[2]已经换成了`7`，而系统并没有记录`15`被换到了什么地方。

这个时候，想要得到i位置的元素，直接`datas[i]`就可以了。

### 使用索引堆

<font color='red'>元素比较的时候是datas的数据，而交换时候是indexes的数据(索引)，datas不做任何改动。</font>

使用索引堆后，初始化两个数组应该是这样的：

> - datas: [-, 1, 15, 20, 34, 7]
> - indexes: [-, 1, 2, 3, 4, 5]

这个时候，我们就交换`indexes`数组里面的索引`2`和`5`，而不操作`datas`数组。交换后两个数组是这个样子：

> - datas: [-, 1, 15, 20, 34, 7]
> - indexes: [-, 1, 5, 3, 4, 2]

这个时候，想要得到i位置的元素，就需要`datas[indexes[i]]`来获取。

**用图来理解：**

构建前：

![](<http://img.dongcoder.com/up/info/201807/20180719233859584351.png>)

构建后：

![](<http://img.dongcoder.com/up/info/201807/20180719233859880231.png>)





### 索引堆的优化               

前面的index heap 虽然解决了改变堆中元素，由于data位置是不改变的，所以可以通过 data[i] = newItem; 但是，改变index数组以维护队的性质这个操作就无法通过一步实现了，此时，需要先遍历indexes数组，时间复杂度是O(N)，再调整的时间复杂度是O(logN),所以T(N) = O(N0+O(logN)。

比如，我们想要改变所以位置4的data，更改后要维护这个堆，这个堆中存储的元素是上一行的索引，所以要在indexes数组中找到4所在位置，需要遍历indexes数组，找到是第9个位置，然后对滴9个位置调整，如果，我们在对这个类中添加一个reverse属性，关系如下：

![](<http://img.dongcoder.com/up/info/201807/20180719233900125332.png>)



**rev与index的关系：**



![](<http://img.dongcoder.com/up/info/201807/20180719233900294267.png>)

