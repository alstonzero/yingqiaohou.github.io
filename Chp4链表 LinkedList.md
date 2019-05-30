# 链表 LinkedList

### 特点：

链表由Node节点组成，Node类似于火车车厢，由数据和指向下一个Node的引用（next）组成。

### 首先在链表类中定义私有内部类Node

而Node中next的定义类型也称作“自引用”式，因为Node中包含了一个和自己类型相同的字段（本例中为next）。类型Node的next字段仅仅是对另外的一个Node对象的“引用”，而不是一个对象。

```java
private class Node{         //用户不需要知道底层是如何实现的，因此设置为私有类。
    public E e;             //data
    public Node next;       //指向下一个Node的引用。自引用。
    
    public Node(E e,Node next){ //构造函数
        this.e = e;
        this.next = next;
    }
    public Node(E e){
        this(e,null);
    }
    public Node(){
        this(null,null);
    }
    @Override
    public String toString(){ //重写toString方法
        e.toString();
    }
}
```

### Linkelist类中的属性和重要的方法

#### 属性：

Node类型的虚拟头结点dummyHead，表示Node数量的head属性

#### 方法：

**add(int index,E e)方法:** 在链表的index位置添加节点Node

```java
public void add(int index,E e){
    if(index < 0|| index > size)
        throw new IllegalArgument("");
    Node prev = dummyHead;
    for(int i = 0;)
}
```

