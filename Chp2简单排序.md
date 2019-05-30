# 简单排序



### 冒泡排序

**思路：**

1，将最小的数据放在数组的最开始（index为0），并将最大数据放在数组的最后（数组下标为nELem-1）。

2，外层for循环的计数器out从数组最后开始，即out等于nElems-1，每次循环一次out减一。index大于out的数据已经排好序了。变量out在每完成一次内部循环（计数器为in）后就左移一位，因此算法就不再处理那些已经排好序的数据了,最终到index为1。

3，内层for从循环计数器in从index 0开始，每循环一次in加一，当in等于out时结束一次循环。在内层for循环体中，数组下标index为in和in+1的两个数据项进行比较，如果下标index为in的数据项大于index为in+1的数据项，则交换两个数据项。

```java
public void bubbleSort(){
    int out,in;
    int temp;
    for(out=nElems-1;out>1;out--){
        for(in=0;in<out;in++){
            if(a[in]>a[in+1]){
                temp = a[in];
                a[in] = a[in+1];
                a[in+1]=temp;
            }
        }
    }
}
```

### 选择排序

**思路：**

1，外层循环用循环变量out，从数组开头开始（index为0）向高位增长。内层循环用循环变量in，从index为out+1开始，同样是向右移位。

2，在每一个in的新位置，数据项a[in]和a[min]进行比较。如果a[in]更小，则min被赋值为in的值。在内层循环的最后，min指向最小的数据项，然后交换out和min指向的数组数据项。

```java
public void selectionSort(){
    int out,in,min;
    int temp;
    for(out=0;out<nElems-1;out++){
        min = out;                     //minimum
        fot(in=out+1;in<nElems;in++)   //inner loop
            if(a[in]<a[min])           //if min greater
                min = in;              //we have a new min
        temp = a[out];
        a[out] = a[min];
        a[min] = temp;
    }
}
```

#### 选择排序的效率

选择排序与冒泡排序执行了相同次数的比较：N*(N-1)/2。对于10个数据项，需要45次比较。但只需要少于10次的交换。当N值很大时，比较的次数是很重要的，结论上选择排序和冒泡排序一样运行了O(N^2)时间。但是，选择排序无疑更快，因为它进行的交换少得多。

### 插入排序

**思路：**

在数组中间有一个作为标记元素。在这个标记元素左边的所有元素已经是局部有序的，右边所有元素是无序的

```java

```

