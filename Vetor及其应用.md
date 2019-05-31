# Vetor及其应用

### Vetor的基本概念：

Vector（向量）是 java.util 包中的一个类，该类实现了类似 <font color='tomato'>动态数组</font> 的功能。

#### Vetor和Array区别:

向量和数组相似，都可以保存一组数据（数据列表）。但是数组的大小是固定的，一旦指定，就不能改变，而向量却提供了一种类似于“动态数组”的功能，向量与数组的重要区别之一就是<font color='tomato'>向量的容量是可变的</font>。

可以在向量的任意位置插入不同类型的对象，无需考虑对象的类型，也无需考虑向量的容量。

#### 适合Vetor的场合：

向量和数组分别适用于不同的场合，一般来说，下列场合更适合于使用向量：

- 如果需要频繁进行对象的插入和删除工作，或者因为需要处理的对象数目不定。
- 列表成员全部都是对象，或者可以方便的用对象表示。
- 需要很快确定列表内是否存在某一特定对象，并且希望很快了解到对象的存放位置。

#### 适合Array的场合：

向量作为一种对象提供了比数组更多的方法，<font color='tomato'>但需要注意的是，向量只能存储对象，不能直接存储简单数据类型，</font>,因此下列场合适用于使用数组：

- 所需处理的对象数目大致可以确定。
- 所需处理的是简单数据类型。



### vector 向量类提供了三种构造方法： 

```java
Vector();  //①创建空向量，初始大小为 10
Vector(int initialCapacity);  //②创建初始容量为 capacity 的空向量
Vector(int initialCapacity,int capacityIncrement);  //③创建初始容量为 initialCapacity，增量为 capacityIncrement 的空向量
```

使用第①种方式系统会自动对向量进行管理。

使用第②种方式，会创建一个初始容量（即向量可存储数据的大小）为 initialCapacity 的空向量，当真正存放的数据超过该容量时，系统会自动扩充容量，每次增加一倍。

使用第③中方式，会创建一个初始容量为 initialCapacity 的空向量，当真正存放的数据超过该容量时，系统每次会自动扩充 capacityIncrement。如果 capacityIncrement 为0，那么每次增加一倍。

通过分配多于所需的内存空间，向量减少了必须的内存分配的数目。这样能够有效地减少分配所消耗的时间，每次分配的额外空间数目将由创建向量时指定的增量所决定。  

### 插入功能： 

#### (1)将obj插入向量的尾部（是对象而不是数值）

```java
public final synchronized void adddElement(Object obj) 
```

obj可以是任何类型的对象。对同一个向量对象，亦可以在其中插入不同类的对象。<font color='tomato'>但插入的应是对象而不是数值</font>，所以插入数值时要注意将数组转换成相应的对象。 

例如：要插入整数1时，不要直接调用v1.addElement(1),正确的方法为： 

```java
Vector v1 = new Vector(); 
Integer integer1 = new Integer(1); 
v1.addElement(integer1); 
```

#### (2)将index处的对象设置成obj，原来的对象将被覆盖。

```java
public final synchronized void setElementAt(Object obj,int index) 
```

#### (3)在index指定的位置插入obj，原来对象以及此后的对象依次往后顺延。 

```java
public final synchronized void insertElement(Object obj,int index) 
```

### 删除功能： 

#### (1)从向量中删除obj,若有多个存在，则从向量头开始试，删除找到的第一个与obj相同的向量成员。 

```java
public final synchronized void removeElement(Object obj) 
```

#### (2)删除向量所有的对象 

```java
public final synchronized void removeAllElement(); 
```

#### (3)删除index所指的地方的对象 

```java
public fianl synchronized void removeElementAt(int index) 
```

### 查询搜索功能： 
#### (1)从向量头开始搜索obj,返回所遇到的第一个obj对应的下标，若不存在此obj,返回-1

````java
public final int indexOf(Object obj) 
````

#### (2)从index所表示的下标处开始搜索obj. 

```java
public final synchronized int indexOf(Object obj,int index) 
```

#### (3)从向量尾部开始逆向搜索obj. 

```java
public final int lastindexOf(Object obj) 
```

#### (4)从index所表示的下标处由尾至头逆向搜索obj. 

```java
public final synchornized int lastIndex(Object obj,int index) 
```

#### (5)获取向量对象中的首个obj 

```java
public final synchornized firstElement() 
```

#### (6)获取向量对象的最后一个obj 

```java
public final synchornized Object lastElement() 
```

### 例子：VectorApp.java 

```java
import java.util.Vector; 
import java.lang.*; 
import java.util.Enumeration; 
public class VectorApp 
{ 
     public static void main(String args[]) 
     { 
          Vector v1 = new Vector(); 
          Integer integer1= new Integer(1); 
          //加入为字符串对象 
          v1.addElement("one"); 
          //加入的为integer的对象 
          v1.addElement(integer1); 
          v1.addElement(integer1); 
          v1.addElement("two"); 
          v1.addElement(new Integer(2)); 
          v1.addElement(integer1); 
          v1.addElement(integer1); 
          //转为字符串并打印 
          System.out.println("The Vector v1 is:\n\t"+v1); 
         
         //向指定位置插入新对象 
          v1.insertElement("three",2); 
          v1.insertElement(new Float(3.9),3); 
          System.out.println("The Vector v1(used method insertElementAt()is:\n\t)"+v1); 
          
          //将指定位置的对象设置为新的对象 
          //指定位置后的对象依次往后顺延 
           v1.setElementAt("four",2); 
           System.out.println("The vector v1 cused method setElmentAt()is:\n\t"+v1); 
           
          //从向量对象v1中删除对象integer1 
          //由于存在多个integer1,所以从头开始。 
          //找删除找到的第一个integer1. 
          v1.removeElement(integer1); 
          System.out.println("The vector v1 (used method removeElememt()is"); 
          
          //使用枚举类(Enumeration)的方法取得向量对象的每个元素。
           Enumeration enum = v1.elements(); 
           while(enum.hasMoreElements()) //来判断集合中是否还有其他元素和方法；
               System.out.println(enum.nextElement()+""); 
           System.out.println(); 
            
            
         //按不同的方向查找对象integer1所处的位置
            System.out.println("The position of Object1(top-to-botton):"+v1.indexOf(integer1)); 
            System.out.println("The position of Object1(tottom-to-top):"+v1.lastIndexOf(integer1)); 
             
            
         //重新设置v1的大小，多余的元素被抛弃  
            v1.setSize(4); 
            System.out.println("The new Vector(resized the vector)is:"+v1); 
              
     } 
} 
```

```
运行结果： 
The vector v1 is:[one,1,1,two,2,1,1] 

The vector v1(used method insetElementAt()) is: 
[one,1,three,3.9,1,two,2,1,1] 

The vector v1(used method setElementAt()) is: 
[one,1,four,3.9,1,two,2,1,1] 

The vector v1(useed method removeElement()) is: 
one four 3.9 1 two 2 1 1 

The position of object1(top-to-botton):3 

The position of object1(botton-to-top):7 

The new Vector(resized the vector) is: 
[one,four,3.9,1] 
```



#### (1)类vector定义了方法  
**public final int size();** 
此方法用于获取向量元素的个数。它们返回值是向量中实际存在的元素个数，而非向量容量。可以调用方法capacity()来获取容量值。 

**public final synchronized void setsize(int newsize); **
此方法用来定义向量的大小，若向量对象现有成员个数已经超过了newsize的值，则超过部分的多余元素会丢失。 

#### (2)关于例子中的Enumeration

程序中定义Enumeration类的一个对象Enumeration是java.util中的一个接口类， 在Enumeration中封装了有关枚举数据集合的方法。 
在Enumeration提供了方法

**hasMoreElement()**：来判断集合中是否还有其他元素和方法；

**nextElement()**来判断集合中是否还有其他元素和方法nextElement()来获取下一个元素。

利用这两个方法，可以依次获得集合中的元素。 

Vector中提供方法： 
**public final synchronized Enumeration elements(); **

此方法将向量对象对应到一个枚举类型。java.util包中的其他类中也都有这类方法，以便于用户获取对应的枚举类型。

