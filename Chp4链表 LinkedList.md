# 链表 LinkedList

### 一、基本概念
链表：最简单的动态数据结构

重要性：更深入的理解引用和递归，辅助组成其他数据结构

优点：真正的动态，不需要处理固定的容量

缺点：丧失了随机访问的能力

与数组对比：数组最好用于索引有语义的情况(例 score[2])，其最大的有点是支持快速查询；

![](<https://img-blog.csdn.net/20180912210534155?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)






### 特点：

图解：数据存储在 “节点”（Node）中，实际存储在 e 中，数据与数据之间的链接(对下一个链表的引用)由 next 完成；最后一个节点 next 存储的为 Null；如果为Null则证明是最后一个节点。

![](<https://img-blog.csdn.net/20180912211530274?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

### 首先在链表类中定义私有内部类Node

而Node中next的定义类型也称作“自引用”式，因为Node中包含了一个和自己类型相同的字段（本例中为next）。类型Node的next字段仅仅是对另外的一个Node对象的“引用”，而不是一个对象。

```java
private class Node{         //用户不需要知道底层是如何实现的，因此设置为私有类。
    public E e;             //data
    public Node next;       //指向下一个Node的引用。自引用。
    
    public Node(E e,Node next){ //声明构造函数，用户也同时传来 e 和 next
        this.e = e;             //声明构造函数，用户也同时传来 e 和 next
        this.next = next;       //将当前节点的next赋值成用户传来的next
    }
    public Node(E e){           //用户只传来e
        this(e,null);           //将当前节点的e赋值成用户传来的e，next设为null
    }
    public Node(){              //用户什么都不传
        this(null,null);        //将e和next都设为null
    }
    @Override
    public String toString(){ //重写toString方法
        e.toString();
    }
}
```

### 二、 在链表中添加元素

对于一个链表来说，想要访问存储在其中的所有节点，我们必须把链表的头 head 存储起来，

![](<https://img-blog.csdn.net/20180912220635222?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

#### 在链表头添加元素:

图解（要将 666【叫做node】 加入链表中而不破坏现有的链表结构）

![](<https://img-blog.csdn.net/20180912221139284?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

步骤：1.让 node 的 next 指向现在链表的头，即**执行  node.next = head**

![](<https://img-blog.csdn.net/20180912221542961?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

2. 此时，666 成为新的链表头，让 head  指向新的 666 节点，即执行  head = node

   ![](<https://img-blog.csdn.net/20180912221744783?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

#### 在链表中间添加元素:

图解：将 666 插入到链表中 1 的位置

![](<https://img-blog.csdn.net/20180912223225305?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

步骤：1.要插入节点 666 ，必须要找到插入666之后，这个节点之前的节点是谁，将其叫做 prev，其初始化和 head 在同一个地方,执行 Node prev = head;    

![](<https://img-blog.csdn.net/20180912223708275?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

步骤2：要找到 666 之前节点 ，因为插入 666 的索引为2【链表中并没有索引这个概念】故之前节点的索引为 1 ，从0开始遍历，遍历到索引为1的位置即可；

![](<https://img-blog.csdn.net/20180912224456155?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

步骤3：将 node 的 next 指向 prev 的下一个元素，即执行 **node.next = prev.next**

![](<https://img-blog.csdn.net/2018091222474631?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

步骤4：prev 的 next 指向 node, **即执行 node.next = prev.next,成功将 666 插入到链表中**

![](<https://img-blog.csdn.net/20180912225050874?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

**关键：找到要添加的节点的前一个节点（prev）**

```java
public class LinkedList<E> {
 
    private class Node{
        public E e;
        public Node next;
 
        public Node(E e, Node next){
            this.e = e;
            this.next = next;
        }
 
        public Node(E e){
            this(e, null);
        }
 
        public Node(){
            this(null, null);
        }
 
        @Override
        public String toString(){
            return e.toString();
        }
    }
	//（新增代码）
    private Node head;	//声明Node型变量head
    private int size;	//size记录链表中有多少个元素
 
    public LinkedList(){	//构造函数，链表初始化时的head与size
        head = null;
        size = 0;
    }
 
    // 获取链表中的元素个数（新增代码）
    public int getSize(){
        return size;
    }
 
    // 返回链表是否为空（新增代码）
    public boolean isEmpty(){
        return size == 0;
    }
 
    // 在链表头添加新的元素e（新增代码） 
    public void addFirst(E e){
//        Node node = new Node(e); //创建新的节点 e
//        node.next = head;		//让 node 的 next 指向现在链表的头
//        head = node;			//让 head  指向新的节点
 
        head = new Node(e, head);	//上面三行代码的优雅写法
        size ++;
    }
 
    // 在链表的index(0-based)位置添加新的元素e（新增代码）
    // 在链表中不是一个常用的操作，练习用：）
    public void add(int index, E e){		
 
        if(index < 0 || index > size)	//判断 index 的合法性
            throw new IllegalArgumentException("Add failed. Illegal index.");
 
        if(index == 0)
            addFirst(e);
        else{
            Node prev = head;	
            for(int i = 0 ; i < index - 1 ; i ++)
                prev = prev.next;		
			//把当前 prev 存的下一个节点放到 prev 变量中，prev会在链表中一直移动，直到 index - 1 这个位置
 
//            Node node = new Node(e);		//创建node,元素为 e
//            node.next = prev.next;		//将 node 的 next 指向 prev 的下一个元素
//            prev.next = node;				//prev 的 next 指向 node
 
            prev.next = new Node(e, prev.next);		//上面三行的优雅写法
            size ++;
        }
    }
 
    // 在链表末尾添加新的元素e（新增代码）
    public void addLast(E e){
        add(size, e);
    }
}

```

### 三、使用链表的虚拟头结点

上面所述的方法中在链表头添加元素时存在特殊：为链表添加元素的过程要找到待添加位之前的节点，**但对于链表头来说并没有之前的节点，所以在逻辑上特殊。通过使用链表的虚拟头节点即可将链表头添加元素与其他位置添加元素统一起来。**

图解：创建个虚拟节点即可解决链表头添加元素时的问题，对链表来说，第一个元素是 dummyHead 的 next 对应的节点的元素，而不是 dummyHead 对应的节点的元素，**dummyHead 这个位置的元素是根本不存在的，对用户来说也没有意义，只是为了逻辑编写方便设置的虚拟头结点。**这样所有的元素都有前一个位置的节点，并且初始的时候 dummyHead 指向的就是 0 这个元素的前一个节点。

![](<https://img-blog.csdn.net/20180912231104308?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

```java
public class LinkedList<E> {
 
    private class Node{
        public E e;
        public Node next;
 
        public Node(E e, Node next){
            this.e = e;
            this.next = next;
        }
 
        public Node(E e){
            this(e, null);
        }
 
        public Node(){
            this(null, null);
        }
 
        @Override
        public String toString(){
            return e.toString();
        }
    }
 
    private Node dummyHead;		//引入虚拟头结点（修改代码）
    private int size;
 
    public LinkedList(){
        dummyHead = new Node();//（修改代码）
        size = 0;
    }
 
    // 获取链表中的元素个数
    public int getSize(){
        return size;
    }
 
    // 返回链表是否为空
    public boolean isEmpty(){
        return size == 0;
    }
 
    // 在链表的index(0-based)位置添加新的元素e
    // 在链表中不是一个常用的操作，练习用：）
    public void add(int index, E e){
 
        if(index < 0 || index > size)
            throw new IllegalArgumentException("Add failed. Illegal index.");
 
        Node prev = dummyHead;	//
        for(int i = 0 ; i < index ; i ++)		//找到 index 位置的前一个节点
            prev = prev.next;	//prev 初始时是从 dummyHead 开始的（修改代码）
 
        prev.next = new Node(e, prev.next);
        size ++;
    }
 
    // 在链表头添加新的元素e
    public void addFirst(E e){
        add(0, e);		//（修改代码）直接复用 add 即可
    }
 
    // 在链表末尾添加新的元素e
    public void addLast(E e){
        add(size, e);
    }
}

```

### 四、链表遍历，查询和修改

```java
 // 获得链表的第index(0-based)个位置的元素
    // 在链表中不是一个常用的操作，练习用：）
    public E get(int index){

        if(index < 0 || index >= size)
            throw new IllegalArgumentException("Get failed. Illegal index.");

        Node cur = dummyHead.next;
        for(int i = 0 ; i < index ; i ++)
            cur = cur.next;
        return cur.e;
    }

    // 获得链表的第一个元素
    public E getFirst(){
        return get(0);
    }

    // 获得链表的最后一个元素
    public E getLast(){
        return get(size - 1);
    }

    // 修改链表的第index(0-based)个位置的元素为e
    // 在链表中不是一个常用的操作，练习用：）
    public void set(int index, E e){
        if(index < 0 || index >= size)
            throw new IllegalArgumentException("Set failed. Illegal index.");

        Node cur = dummyHead.next;
        for(int i = 0 ; i < index ; i ++)
            cur = cur.next;
        cur.e = e;
    }

    // 查找链表中是否有元素e
    public boolean contains(E e){
        Node cur = dummyHead.next;
        while(cur != null){
            if(cur.e.equals(e))
                return true;
            cur = cur.next;
        }
        return false;
    }

    @Override
    public String toString(){
        StringBuilder res = new StringBuilder();

//        Node cur = dummyHead.next;
//        while(cur != null){
//            res.append(cur + "->");
//            cur = cur.next;
//        }
        for(Node cur = dummyHead.next ; cur != null ; cur = cur.next)
            res.append(cur + "->");
        res.append("NULL");

        return res.toString();
    }
}
```

测试

```java
public class Main {

    public static void main(String[] args) {

        LinkedList<Integer> linkedList = new LinkedList<>();
        for(int i = 0 ; i < 5 ; i ++){
            linkedList.addFirst(i);
            System.out.println(linkedList);
        }

        linkedList.add(2, 666);
        System.out.println(linkedList);
    }
}

```

### 五、删除元素

删除元素和添加元素基本类似，都是定位之前一个删除元素之前的元素
**修改元素指向才是改变链表的本质（修改元素并不能改变链表）**

![](<https://img.mukewang.com/5b4a892b00017cbf13060595.png>)

```java
public E remove(int index){
        if(index < 0 || index >= size)
            throw new IllegalArgumentException("Remove failed. Index is illegal.");

        Node prev = dummyHead;
        for(int i = 0 ; i < index ; i ++)
            prev = prev.next;

        Node retNode = prev.next;
        prev.next = retNode.next;
        retNode.next = null;
        size --;

        return retNode.e;
    }

    // 从链表中删除第一个元素, 返回删除的元素
    public E removeFirst(){
        return remove(0);
    }

    // 从链表中删除最后一个元素, 返回删除的元素
    public E removeLast(){
        return remove(size - 1);
    }

    // 从链表中删除元素e
    public void removeElement(E e){

        Node prev = dummyHead;
        while(prev.next != null){
            if(prev.next.e.equals(e))
                break;
            prev = prev.next;
        }

        if(prev.next != null){
            Node delNode = prev.next;
            prev.next = delNode.next;
            delNode.next = null;
            size --;
        }
    }

```

### 六、链表中的方法和对应的时间复杂度

![](<https://img.mukewang.com/5b4a8e7b0001bf9e13561349.png>)

