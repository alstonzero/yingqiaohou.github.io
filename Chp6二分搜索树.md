# 二分搜索树(Binary Search Tree)

### 一、树结构


使用树结构的原因：

1.树结构是一种天然的组织结构

   ![](<https://img-blog.csdn.net/20180914232450544?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

2.数据使用树结构存储后，查找高效

二叉树：是动态数据结构【不需要在创建数据结构的时候就决定好要存储多少数据，要添加元素就 new 一个新的空间加入到其中即可，删除二、基础概念同理】

​    

节点 Node 中，除了要存放元素 e 之外，还要存储两个指向其他节点的引用 <font color='tomato'>left【左孩子】 和 right 【右孩子】</font>

#### 二叉树要点：

<font color='tomato'>1.二叉树具有唯一的根节点；</font>

<font color='tomato'>2.二叉树中每个节点最多有两个孩子，每个节点最多有一个父亲节点；一个孩子也没有的叫做叶子节点</font>

<font color='tomato'>3.二叉树具有天然的递归结构【每个节点的左子树也是二叉树、每个节点的右子树也是二叉树】</font>

![](<https://img-blog.csdn.net/20180914234756180?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

4.二叉树不一定都是“满”的【满二叉树：除叶子节点外，每个节点都有两个孩子】

5.一个节点、甚至 NULL 也是二叉树

 

### 二、构建二分搜索树：

#### 二分搜索树的性质：

1.二分搜索树是二叉树

2.二分搜索树的每个节点的值：大于其左子树的所有节点的值且小于其右子树的所有节点的值

3.二分搜索树的每个子树也是二分搜索树

4.存储的元素必须具有可比较性【存储自定义数据类型，必须自定义好数据的比较方式



```java
public class BST<Key extends Comparable<Key>,Value> {		//E 要满足Comparable接口，要有可比较性
    private class Node {	//声明对应的节点类型
        private Key key;
        private Value value;
        private Node left, right;

        public Node(Key key,Value value) {
            this.key = key;	    //e为用户传来的e
            this.value = value;	//左孩子初始化
            left = right = null;	//右孩子初始化
        }
    }

    private Node root;	//根节点root
    private int count;	//count 记录当前二叉树存储元素的数量

    public BST(){	//二分搜索树的构造函数
        root = null;
        count = 0;
    }
    //返回二分搜索树的节点个数
    public int size(){
        return count;
    }
    //返回二分搜索树是否为空
    public boolean isEmpty(){	//二分搜索树为空
        return count == 0;
    }
}
```

### 三、向二分搜索树中添加元素
图解步骤

1，要插入元素60，先从根root节点进行比较，由于60比41大，所以要把60插入到41的右子树。

![](<https://img-blog.csdn.net/20180915094028667?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

 2，而41的右子树存在元素，因此61不能直接插入。于是60与41的右子树58进行比较，由于60比58大，因此要插入到58的右子树

 ![](<https://img-blog.csdn.net/20180915094108730?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

3，而58的右子树没有元素，则直接将60插入58的右子树。

![](<https://img-blog.csdn.net/2018091509415511?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)

![](<https://img-blog.csdn.net/20180915094218385?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYW5naGFvMjMz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70>)<font color='tomato'>特殊情况（有重复的话，该元素就相当于已经存在于树中，不做任何改变）</font>

  

如果想要包含重复元素，只需定义：左子树 <= 节点；右子树 >= 节点【只需将 = 关系放到定义中即可】

代码实现：

#### insert--在以node为根的二分搜索树中插入节点(key,value)

使用递归算法

**递归出口：1，当前节点为空；2，插入的key比node节点中的key小，且左子树为空；3、插入的key比Node节点中的key大且右子树为空；4、**

```java
// 向二分搜索树中添加新的元素e
    public void insert(Key key,Value value){

        if(root == null){
            root = new Node(e);		//根节点直接指向新建的元素即可
            count ++;
        }
        else
            add(root, key ,value);	//从根节点添加新元素e
    }

    // 向以node为根的二分搜索树中插入元素e，递归算法
    private void insert(Node node, Key key ,Value value){	//传入参数 Node 与 e
        if(key.compareTo(node.key)==0)	//【递归的终止条件1】传入的key已经存在于树中
            node.value = value;
            return;
        else if(key.compareTo(node.key) < 0 && node.left == null){	
            //【递归的终止条件2】插入的key比node节点中的key小，且左子树为空
            node.left = new Node(key,value);	//插入左子树中，直接让node的左孩子为新插入的e
            count ++;
            return;
        }
        else if(key.compareTo(node.key) > 0 && node.right == null){
            //【递归的终止条件3】插入的key比Node节点中的key大且右子树为空
            node.right = new Node(key,value);//插入右子树中，直接让node的右孩子为新插入的e
            count ++;
            return;
        }

        if(key.compareTo(node.key) < 0)	//递归调用，插入的e比Node节点中的e小，e插入左子树
            insert(node.left, key,value);
        else //e.compareTo(node.e) > 0
            insert(node.right, key,value);	//node 的右孩子成为元素e
    }
```
**<font color='tomato'>精简代码：</font>**<font color='tomato'>递归出口改为一个：即，只要node为null则必须插入新节点。将节点与二叉树挂起来</font>

```java
// 向以node为根的二分搜索树中插入元素e，递归算法
// 返回插入新节点后二分搜索树的根 
private Node add(Node node,Key key,Value value){
    if(node == null){	//只要node 为null,则必须新插入节点
        count ++;
        return new Node(key,value); //将节点与二叉树挂接起来
    }
    if(key.compareTo(node.key)==0)
        node.value = value;
    else if(e.compareTo(node.e) < 0)
        node.left = insert(node.left, e);	//传入的key小于当前节点，则用递归函数将其插入到node的左子树中
    else // key.compareTo(node.key) > 0
        node.right = insert(node.right, e);
    return node;   //最后把以node为根的树返回回去
    }
}
```
### 四、 二分搜索树的查询操作

#### contain--查看二分搜索树是否存在键key

使用递归算法

**递归出口：1，传入的node为null，返回false；2，传入的key与node的key相等则找到，返回true**

```java
// 看二分搜索树中是否包含元素e（新增代码）
public boolean contain(Key key){
    return contain(root,key); //从根节点开始查看
}

// 查看以node为根的二分搜索树中是否包含键值为key的节点, 使用递归算法
private boolean contain(Node node,Key key){
    
    if(node == null) //[递归出口1]如果传入的node为空直接返回false
        return false;
    if(key.compareTo(node.key) == 0) //[递归出口2]如果传入的key与node的key相等则找到，返回true
        return ture;
    else if(key.compareTo(node.key) < key) //如果传入的key小于node的key，则向递归函数中传入node的左子节点，进行递归调用
        return contain(node.left,key);
    else //key.compareTo(node.key)>0
        return contain(node.right,key);
}
```

### 五、search--二分搜索树中搜索键key所对应的值value。

使用

**递归出口：1，当前节点为空，返回null；2，传入的key与当前节点node的key相等，则直接返回node.value**

```java
public Value search(key key){
    return search(root,key);//从根节点开始搜索
}
// 在以node为根的二分搜索树中查找key所对应的value, 递归算法
// 若value不存在, 则返回NULL
private Value search(Node node,Key key){
    
    if(node == null)  //[递归出口1]如果传入的node为空直接返回false
        return null;
    if(key.compareTo(node.key)==0) //[递归出口2]如果传入的key与node的key相等则找到，返回node.value
        return node.value;
    else if(key.compareTo(node.key)<0) //如果传入的key小于node的key，则向递归函数中传入node的左子节点，进行递归调用
        return search(node.left,key);
    else //key.compareTo(node.key)>0
        return search(node.right,key);
}
```

### 六、遍历二叉搜索树

![](<https://img.mukewang.com/5b4d3d6a0001159721122120.png>)

#### 前序遍历

只要node非null。首先打印当前node自己的key值；再通过递归函数遍历node的左子节点；最后通过递归函数遍历node的右子节点。

```java
public void preOrder(){
    preorder(root); //从根节点开始
}
// 对以node为根的二叉搜索树进行前序遍历, 递归算法
private void preOrder(Node node){
    if(node !=null){ //在node不为空的情况下，则进行遍历
        System.out.println(node.key);
        preOrder(node.left);
        preOrder(node.right);
    }
}
```

#### 中序遍历(遍历后是从小到大排好序的)

只要node非null。首先用递归函数遍历node的左子节点；再打印当前node的key值；最后用递归函数遍历node的右子节点。

```java
public void inOrder(){
    inOrder(root);
}
private void inOrder(Node node){
    if(node != null){
        inOrder(node.left);
        System.out.println(node.key);
        inOrder(node.right);
    }
}
```

#### 后续遍历

只要node非null。首先用递归函数遍历node的左子节点；再用递归函数遍历node的右子节点；最后打印当前node的key值。

```java
public void postOrder(){
    postOrder();
}
private void postOrder(Node node){
    postOrder(node.left);
    postOrder(node.right);
    System.out.println(node.key);
}
```

### 七、二分搜索树的层序遍历

![](<https://img.mukewang.com/5b4fc7f000017bc912370625.png>)

**思路：**需要借助队列实现。先把根节点root入队。进行循环条件是队列不为空，队首元素出列，打印该节点node的key值，随后该节点的左子节点和右子节点入队。



```java
public void levelOrder(){
    //我们使用LinkedList来作为我们的队列
    ListedList<Node> q = new LinkedList<Node>();
    q.add(root);
    while(! q.isEmpty()){
        
        Node node = q.remove();//当前节点出队
        System.out.println(node.key); //打印当前节点的key
        
        if(node.left != null)
            q.add(node.left);
        if(node.right != null)
            q.add(node.right);            
    }
}
```



### 八、寻找最小和最大的key的节点

**思路**：定义递归函数。递归出口：若该节点左(右)子节点为空，则直接返回其自身；其左（右）子节点不为空，则递归调用其左（右）子节点

```java
// 寻找二分搜索树的最小的键值
public Key minimum(){
    assert count != 0;  //只要二分搜索树不为空
    Node minNode = minimum( root );
    return minNode.key;
        
}
// 返回以node为根的二分搜索树的最小键值所在的节点
private Node minimum(Node node){
    if( node.left == null )  //递归出口
        return node;         //若该节点左子节点为空，则直接返回其自身

    return minimum(node.left); //其左子节点不为空，则递归调用其左子节点
}


// 寻找二分搜索树的最大的键值
public Key maximum(){
    assert count != 0;
    Node maxNode = maximum(root);
    return maxNode.key;
}

// 返回以node为根的二分搜索树的最大键值所在的节点
private Node maximum(Node node){
     if( node.right == null )
        return node;

     return maximum(node.right);
    }
```

### 九、删除最小和最大key的节点

**思路：** 定义递归函数。递归出口：当前节点node的左孩子为空（即证明当前节点为最小节点），用当前节点的右子节点node.right，代替当前节点返回给当前节点的母节点。

```java
// 从二分搜索树中删除最小值所在节点
    public void removeMin(){
        if( root != null )
            root = removeMin( root );
    }

    // 从二分搜索树中删除最大值所在节点
    public void removeMax(){
        if( root != null )
            root = removeMax( root );
    }
  private Node removeMin(Node node){

        if( node.left == null ){  //递归出口。如果当前节点的左孩子为空则当前节点就是最小节点

            Node rightNode = node.right;  //把node节点的右孩子，作为原来node节点的父节点的左孩子
            node.right = null;     
            count --;                     
            return rightNode;    //用当前node的右孩子代替了node
        }

        node.left = removeMin(node.left); //当前节点左孩子不为空，则递归调用。
        return node;
    }

    // 删除掉以node为根的二分搜索树中的最大节点
    // 返回删除节点后新的二分搜索树的根
    private Node removeMax(Node node){

        if( node.right == null ){

            Node leftNode = node.left;
            node.left = null;
            count --;
            return leftNode;
        }

        node.right = removeMax(node.right);
        return node;
    }

```

### 十、删除二分搜索树中的任意节点

![](<https://img.mukewang.com/5b5079d30001eb5a12130546.png>)

![](<https://img.mukewang.com/5b507be600012ed911620527.png>)





```java
// 从二分搜索树中删除键值为key的节点
    public void remove(Key key){
        root = remove(root, key);
    }
// 删除掉以node为根的二分搜索树中的最小节点
    // 返回删除节点后新的二分搜索树的根
    private Node remove(Node node, Key key){

        if( node == null )
            return null;

        if( key.compareTo(node.key) < 0 ){
            node.left = remove( node.left , key );
            return node;
        }
        else if( key.compareTo(node.key) > 0 ){
            node.right = remove( node.right, key );
            return node;
        }
        else{   // key == node->key

            // 待删除节点左子树为空的情况
            if( node.left == null ){
                Node rightNode = node.right;
                node.right = null;
                count --;
                return rightNode;
            }

            // 待删除节点右子树为空的情况
            if( node.right == null ){
                Node leftNode = node.left;
                node.left = null;
                count--;
                return leftNode;
            }

            // 待删除节点左右子树均不为空的情况

            // 找到比待删除节点大的最小节点, 即待删除节点右子树的最小节点
            // 用这个节点顶替待删除节点的位置
            Node successor = new Node(minimum(node.right));
            count ++;

            successor.right = removeMin(node.right);
            successor.left = node.left;

            node.left = node.right = null;
            count --;

            return successor;
        }
    }
```

### 十一、测试二分搜索树

```java
// 测试二分搜索树
    public static void main(String[] args) {

        int N = 1000000;

        // 创建一个数组，包含[0...N)的所有元素
        Integer[] arr = new Integer[N];
        for(int i = 0 ; i < N ; i ++)
            arr[i] = new Integer(i);

        // 打乱数组顺序
        for(int i = 0 ; i < N ; i ++){
            int pos = (int) (Math.random() * (i+1));
            Integer t = arr[pos];
            arr[pos] = arr[i];
            arr[i] = t;
        }
        
        // 我们测试用的的二分搜索树的键类型为Integer，值类型为String
        // 键值的对应关系为每个整型对应代表这个整型的字符串
        BST<Integer,String> bst = new BST<Integer,String>();
        for(int i = 0 ; i < N ; i ++)
            bst.insert(new Integer(arr[i]), Integer.toString(arr[i]));

        // 对[0...2*N)的所有整型测试在二分搜索树中查找
        // 若i在[0...N)之间，则能查找到整型所对应的字符串
        // 若i在[N...2*N)之间，则结果为null
        for(int i = 0 ; i < 2*N ; i ++){
            String res = bst.search(new Integer(i));
            if( i < N )
                assert res.equals(Integer.toString(i));
            else
                assert res == null;
        }
    }
```

#### 注意事项：

由于我们实现的二分搜索树不是平衡二叉树，所以如果按照顺序插入一组数据，我们的二分搜索树会退化成为一个链表。



