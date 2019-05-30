# 栈和队列

### 栈 Stack

#### 概念： 

栈是一种线性结构；

相比数组，栈对应操作的是数组的子集；

只能从一端添加元素，也只能从一段取出元素；

**这一段称作栈顶。**

#### 性质：

栈是一种**先进后出**的数据结构；Last In First Out;在计算机的世界里栈有着不可思议的作用。

#### 栈的应用：

Undo操作(撤销)

#### 栈的实现

Stack

构造方法：根据参数规定的容量创建一个新栈，栈的域包括表示最大容量的变量(即数组的大小)。



**push()方法：**将top(栈顶)的值增加一，使它指向原顶端数据项上面的一个位置，并在这个位置上存储一个数据项。

**pop()方法：** 从栈中移除数据项。返回index为top(栈顶)的数据项值，然后top减一。

**peek()方法：**返回栈顶（top）元素所指的数据项的值，不对栈做任何变动。



### LeetCode例题

给定一个只包括

**思路**

1，import java.util.Stack。并声明一个栈

2，遍历这个字符串，每次用charAt()具体来看字符串中第i个字符是什么样子的，存成 char c

3，如果c=='(' || '[' ||'{' 满足这个条件，将c push进入栈中；否则 (如果当前栈顶没有字符的话，显然匹配失败，则直接return false。)用pop方法查看当前栈顶的字符topchar，如果c是')'但是当前的topchar不是'('则匹配失败;同理，如果c是']'但是当前的topchar不是'['则匹配失败;

4，在循环之外，如果栈里还有字符串则匹配失败。

```java
import java.util.Stack
public class Solution{
      public boolean isVaild(String s){
             Stack<Character> stack = new Stack<>(); //声明这个栈
             for(int i = 0;i<s.length();i++){ //遍历字符串
                 char c = s.charAt(i); //查看字符串中第i个字符是什么样子的，存成char c
                 if（c == '('|| c=='[' || c=='{'）
                      stack.push(c); //如果符合条件，则push压入栈中，以便后期pop出来匹配
                 else{
                     if(stack.isEmpty()) //如果没有要匹配的字符串
                         return false;
                     char topChar = stack.pop();//pop使元素出栈，将栈顶元素存入topChar中
                     //如果不能匹配成功
                     if(c == ')' && topChar != '(')
			   //如果检索到的符号和pop出的符号无法匹配则失败
			    	     return false;
			         if(c == ']' && topChar != '[')
			    	     return false;
			         if(c == '}' && topChar != '}')
			    	     return false;
                 }
             }
             //如果栈为空则匹配成功
             return stack.isEmpty();
      }
}
```





### 队列 Quene

**特点：先进先出**

#### 创建循环队列--对长度取余数

**属性：**头front，尾tail。front == tail 队列为空；(front+1)%data.length == tail 队列满

#### 几个重要方法

**enqueue()方法：** 从队尾入队。

1，判断队列是否已经满了（<font color='tomato'>tail转了一圈之后，与front是否相等</font>），若满了则进行扩容。

2，把元素加到队尾(下标为tail)。

3，tail向后移动一位（<font color='tomato'>为了实现循环，因此需要对长度取余数</font>），size数量增加。

```java
public void enqueue(E e){
    
    if((tail+1)%data.length == front) //如果队列已经满了(已经转了一圈了)
        resize(2*data.length);        //进行扩容
    
    data[tail] = e;                   //把元素加到队尾
    tail = (tail+1)%data.length;      //队尾下标加一，并对数组长度取余数，以便循环。
    size++;  
}
```

**dequeue()方法：**从队首出队。

1，判断队列是否为空，若为空则抛出异常。

2，把要被删除的元素（下标为front）存入ret中，后删除这个元素（指向null）。

3，front向后移动一位（<font color='tomato'>为了实现循环，因此需要对长度取余数</font>），size数量减少

4，为了节约空间，有必要进行缩容。但是缩容的这个值不应该为0。

```java
public E dequeue(){
    if(isEmpty())                      //如果队列为空，则抛出异常
        throw new IllegalArgumentException("Cannot dequeue from an empty queue.");
    E ret = data[front];               //把要删除的元素存在ret中
    data[front] = null;
    front = (front+1) % data.length;   //front向后移动一位，为了实现循环，因此对长度取余数
    size--;
                                       //进行缩容，以免空间被浪费
    if(size == getCapacity()/4 && getCapacity()/2 != 0 )
        resize(getCapacity()/2);
    return ret;
}
```

**resize()方法：**调整队列的容量。

1，新建数组newData

2，通过循环，把旧数组中元素都复制到新数组中。<font color='tomato'>但是，在原来的data中有可能front不是0，可是我们要把它放在新的空间的0的位置，因此存在front个偏移，由于是循环队列i+front会越界，因此加上偏移后还有对长度取余数。</font>

3，把引用变量指向新数组，并设置front为0，tail为size

```java
public void resize(int newCapacity){
    E[] newData =(E[]) new Object[newCapacity+1];
    for(int i = 0;i < size;i++)
        newData[i] = data[(i+front)%data.length];
    data = newData;
    front = 0;
    tail = size;
}
```

#### 整体实现——代码实例

```java
public class LoopQuene<E>{            //支持泛型
    private E[] data;
    private int front,tail;           //队首和队尾的下标
    private int size;                 //元素的数量
    
    public LoopQuene(int capacity){   //有参的构造方法
        data = (E[]) new Object[capacity+1]; //新建一个泛型数组
        front = 0;                    //此时队列中没有元素
        tail = 0;                     
        size = 0;
    }
    public LoopQuene(){               //无参的构造方法
        this(10);
    }
    public int getCapacity(){         //队列的容量
        return data.length-1;
    }
    public boolean isEmpty(){         //是否为空
        return front == tail;
    }
    public int getSize(){             //队列中元素的个数
        return size;
    }
    public void enqueue(E e){         //入队
        if((front+1)%data.length == tail)
            resize(2*data.length);
        data[tail] = e;
        tail = (tail+1)%data.length;
        size++;
    }
    public E dequeue(){               //出队
        if(isEmpty())                 //若队列为空，则抛出异常
            throw new IllegalArgument("");
        E ret = data[front];
        front = (front+1)%data.length; //front向前移动
        data[front] = null;            //将front对应元素设置为null，真正删除
        size--;                        //元素数量减少
        
        if(getCapacity == size/4 && getCapacity) //缩容，但是缩容的这个值不应该为0
            resize(getCapacity/2);
        return ret;
    }
    public void resize(int newCapacity){
        E[] newData = (E[])new Object[newCapacity+1];
        for(int i =0;i<size;i++)      //解释同上
            newData[i] = data[(i+front)%data.length];  
        data = newData;
        front = 0 ;
        tail = size;
    }
    public E getFront(){              //得到队首元素
        if(isEmpty())
            throw new IllegalArgumentException("Queue is empty.");
        return data[front];
    }
}
```





