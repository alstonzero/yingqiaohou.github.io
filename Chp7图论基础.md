# 图论

### 基本概念：

![img](https://pic2.zhimg.com/80/v2-480e6a7c56f1c444c87223fceb113359_hd.jpg)

图论〔Graph Theory〕是数学的一个分支。它以图为研究对象。图论中的图是由若干给定的点及连接两点的线所构成的图形，这种图形通常用来描述某些事物之间的某种特定关系，用点代表事物，用连接两点的线表示相应两个事物间具有这种关系。

图论是一种表示 "**多对多**" 的关系

图是由**顶点**和**边**组成的：(可以无边，但至少包含一个顶点)

- 一组顶点：通常用 V(vertex) 表示顶点集合
- 一组边：通常用 E(edge) 表示边的集合

图可以分为**有向图和无向图**，在图中：

- (v, w) 表示无向边，即 v 和 w 是互通的
- <v, w> 表示有向边，该边始于 v，终于 w

图可以分为**有权图和无权图**：

- 有权图：每条边具有一定的权重(weight)，通常是一个数字
- 无权图：每条边均没有权重，也可以理解为权为 1

图又可以分为**连通图和非连通图**：

- 连通图：所有的点都有路径相连
- 非连通图：存在某两个点没有路径相连

图中的**顶点有度**的概念：

- 度(Degree)：所有与它连接点的个数之和
- 入度(Indegree)：存在于有向图中，所有接入该点的边数之和
- 出度(Outdegree)：存在于有向图中，所有接出该点的边数之和



### 图的表示：

图在程序中的表示一般有两种方式：

#### **1. 邻接矩阵：**

- 在 n 个顶点的图需要有一个 n × n 大小的矩阵
- 在一个无权图中，矩阵坐标中每个位置值为 1 代表两个点是相连的，0 表示两点是不相连的
- 在一个有权图中，矩阵坐标中每个位置值代表该两点之间的权重，0 表示该两点不相连
- 在无向图中，邻接矩阵关于对角线相等

#### **2. 邻接链表：**

- 对于每个点，存储着一个链表，用来指向所有与该点直接相连的点
- 对于有权图来说，链表中元素值对应着权重

例如在无向无权图中：



![img](https://pic1.zhimg.com/80/v2-97b58740d45f3041736d45faacbbfb94_hd.png)

在无向有权图中：





![img](https://pic4.zhimg.com/80/v2-f61069964fbedf56ebf2eba6f60d0267_hd.png)

可以看出在无向图中，邻接矩阵关于对角线对称，而邻接链表总有两条对称的边



而在有向无权图中：



![img](https://pic1.zhimg.com/80/v2-a0a7be239901e18c1b3ff2195d2d7450_hd.png)

#### 邻接矩阵和链表对比：

1. 邻接矩阵由于没有相连的边也占有空间，因此存在浪费空间的问题，而邻接链表则比较合理地利用空间
2. 邻接链表比较耗时，牺牲很大的时间来查找，因此比较耗时，而邻接矩阵法相比邻接链表法来说，时间复杂度低。

**结论：** 邻接表适合表示稀疏图 (Sparse Graph)；邻接矩阵适合表示稠密图 (Dense Graph)

#### 稠密图 - 邻接矩阵代码实现

```java
// 稠密图 - 邻接矩阵
public class DenseGraph {

    private int n;  // 节点数
    private int m;  // 边数
    private boolean directed;   // 是否为有向图
    private boolean[][] g;      // 图的具体数据

    // 构造函数
    public DenseGraph( int n , boolean directed ){
        assert n >= 0;
        this.n = n;
        this.m = 0;    // 初始化没有任何边
        this.directed = directed;
        // g初始化为n*n的布尔矩阵, 每一个g[i][j]均为false, 表示没有任和边
        // false为boolean型变量的默认值
        g = new boolean[n][n];
    }

    public int V(){// 返回节点个数
    	return n;
    } 
    public int E(){ // 返回边的个数
    	return m;
    } 

    // 向图中顶点v和顶点v之间添加一条边
    public void addEdge( int v , int w ){
    //先判断是否越界
        assert v >= 0 && v < n ;
        assert w >= 0 && w < n ;
    //如果v，w之间已经有边了，则直接返回
        if( hasEdge( v , w ) )
            return;

        g[v][w] = true; //表示从v到w有(添加)条边
        if( !directed ) //如果是无向图
            g[w][v] = true;

        m ++; //边的个数增加
    }

    // 验证图中是否有从v到w的边
    boolean hasEdge( int v , int w ){
        assert v >= 0 && v < n ; //先判断是否越界
        assert w >= 0 && w < n ;
        return g[v][w];  //返回布尔矩阵中所对应元素值
    }
}
```

#### 稀疏图 - 邻接表代码实现

```java
import java.util.Vector;
import java.util.LinkedList;

// 稀疏图 - 邻接表
public class SparseGraph {

    private int n;  // 节点数
    private int m;  // 边数
    private boolean directed;    // 是否为有向图
    private Vector<Integer>[] g; // 图的具体数据

    // 构造函数
    public SparseGraph( int n , boolean directed ){
        assert n >= 0;
        this.n = n;
        this.m = 0;    // 初始化没有任何边
        this.directed = directed;
        // g初始化为n个空的vector, 表示每一个g[i]都为空, 即没有任和边
        g = (Vector<Integer>[])new Vector[n];
        //g的每一个元素g[i]也都是一个Vector
        for(int i = 0 ; i < n ; i ++)
            g[i] = new Vector<Integer>();
    }

    public int V(){ return n;} // 返回节点个数
    public int E(){ return m;} // 返回边的个数

    // 向图中添加一个边
    public void addEdge( int v, int w ){

        assert v >= 0 && v < n ;
        assert w >= 0 && w < n ;

        g[v].add(w);
        if( v != w && !directed )
            g[w].add(v);

        m ++;
    }

    // 验证图中是否有从v到w的边
    boolean hasEdge( int v , int w ){

        assert v >= 0 && v < n ;
        assert w >= 0 && w < n ;
        //g[v]是一个Vector，循环g[v]查看其中是否含有w
        for( int i = 0 ; i < g[v].size() ; i ++ )
            if( g[v].elementAt(i) == w )
                return true;
        return false;
    }
}

```

### 通过节点遍历它的临边所对应节点

1. 在邻接矩阵中：需要将这个节点的这一行全部遍历一遍，如果为0说明不相邻；如果为1说明相邻。
2. 在邻接表中：因为这个点这一行所存储的就是相邻的节点元素，则直接输出。

**实现思路：**

1. 将变量g(矩阵或链表)从priavte改为public，直接进行循环。
2. 保持g本身为private，借助迭代器实现

#### 设置迭代器的意义：

稠密图和稀疏图这两个实现，里面的具体实现被屏蔽了。它们呈现给外面的接口是完全一样的，这为我们后续实现图相关的算法带来了方便。我们的每一个算法既对稀疏图也对稠密图成立，因为我们调用的是同一个接口，我们的图算法都应该封装在同一个模板类中。我们就可以任意传入稀疏图或者稠密图到这个模板中。

**1、邻接矩阵的迭代器方法实现:**

这里的g是boolean型矩阵。

```java
// 返回图中一个顶点的所有邻边
// 由于java使用引用机制，返回一个Vector不会带来额外开销,
    
public Iterable<Integer> adj(int v) {
     //定义一个可迭代方法，求v节点的临边所对应节点
     assert v >= 0 && v < n;
     Vector<Integer> adjV = new Vector<Integer>(); 
     for(int i = 0 ; i < n ; i ++ )  //遍历v节点所在的那一行
         if( g[v][i] )               //如果为true证明有临边
             adjV.add(i);            //将临边对应的节点存入vector
     return adjV;                    //返回vector   
}
```

**2、邻接链表的迭代方法实现：**

这里的g是Vector类型变量，里面存储的元素就是临边所对应的节点。

```java
// 返回图中一个顶点的所有邻边
// 由于java使用引用机制，返回一个Vector不会带来额外开销,
public Iterable<Integer> adj(int v){
    assert v >= 0 && v < n;
    return g[v]; 
}
```



### 图的算法框架

**1、首先建立Graph的接口**

```java
// 图的接口
public interface Graph {

    public int V(); //返回节点个数
    public int E(); //返回边的个数
    public void addEdge( int v , int w ); // 向图中添加一个边
    boolean hasEdge( int v , int w );    // 验证图中是否有从v到w的边
    void show();   // 显示图的信息
    public Iterable<Integer> adj(int v); // 返回图中一个顶点的所有邻边(所对应节点)
}
```

**2、实现接口，创建稠密图类DenseGraph（建立邻接矩阵）和稀疏图类SparseGraph（建立邻接表）**

​      **稠密图 - 邻接矩阵** 具体实现

```java
import java.util.Vector;

// 稠密图 - 邻接矩阵
public class DenseGraph implements Graph{

    private int n;  // 节点数
    private int m;  // 边数
    private boolean directed;   // 是否为有向图
    private boolean[][] g;      // 图的具体数据

    // 构造函数
    public DenseGraph( int n , boolean directed ){
        assert n >= 0;
        this.n = n;
        this.m = 0;    // 初始化没有任何边
        this.directed = directed;
        // g初始化为n*n的布尔矩阵, 每一个g[i][j]均为false, 表示没有任和边
        // false为boolean型变量的默认值
        g = new boolean[n][n];
    }

    public int V(){ return n;} // 返回节点个数
    public int E(){ return m;} // 返回边的个数

    // 向图中添加一个边
    public void addEdge( int v , int w ){

        assert v >= 0 && v < n ;
        assert w >= 0 && w < n ;

        if( hasEdge( v , w ) )
            return;

        g[v][w] = true;
        if( !directed )
            g[w][v] = true;

        m ++;
    }

    // 验证图中是否有从v到w的边
    public boolean hasEdge( int v , int w ){
        assert v >= 0 && v < n ;
        assert w >= 0 && w < n ;
        return g[v][w];
    }

    // 显示图的信息
    public void show(){

        for( int i = 0 ; i < n ; i ++ ){
            for( int j = 0 ; j < n ; j ++ )
                System.out.print(g[i][j]+"\t");
            System.out.println();
        }
    }

    // 返回图中一个顶点的所有邻边
    // 由于java使用引用机制，返回一个Vector不会带来额外开销,
    public Iterable<Integer> adj(int v) {
        assert v >= 0 && v < n;
        Vector<Integer> adjV = new Vector<Integer>();
        for(int i = 0 ; i < n ; i ++ )
            if( g[v][i] )
                adjV.add(i);
        return adjV;
    }
}


```

​        **稀疏图 - 邻接表** 具体实现

```java
// 稀疏图 - 邻接表
import java.util.Vector;
public class SparseGraph implements Graph {

    private int n;  // 节点数
    private int m;  // 边数
    private boolean directed;   // 是否为有向图
    private Vector<Integer>[] g; // 图的具体数据

    // 构造函数
    public SparseGraph( int n , boolean directed ){
        assert n >= 0;
        this.n = n;
        this.m = 0;    // 初始化没有任何边
        this.directed = directed;
        // g初始化为n个空的vector, 表示每一个g[i]都为空, 即没有任和边
        g = (Vector<Integer>[])new Vector[n];
        for(int i = 0 ; i < n ; i ++)
            g[i] = new Vector<Integer>();
    }

    public int V(){ return n;} // 返回节点个数
    public int E(){ return m;} // 返回边的个数

    // 向图中添加一个边
    public void addEdge( int v, int w ){

        assert v >= 0 && v < n ;
        assert w >= 0 && w < n ;

        g[v].add(w);
        if( v != w && !directed )
            g[w].add(v);

        m ++;
    }

    // 验证图中是否有从v到w的边
    public boolean hasEdge( int v , int w ){

        assert v >= 0 && v < n ;
        assert w >= 0 && w < n ;

        for( int i = 0 ; i < g[v].size() ; i ++ )
            if( g[v].elementAt(i) == w )
                return true;
        return false;
    }

    // 显示图的信息
    public void show(){

        for( int i = 0 ; i < n ; i ++ ){
            System.out.print("vertex " + i + ":\t");
            for( int j = 0 ; j < g[i].size() ; j ++ )
                System.out.print(g[i].elementAt(j) + "\t");
            System.out.println();
        }
    }

    // 返回图中一个顶点的所有邻边
    // 由于java使用引用机制，返回一个Vector不会带来额外开销,
    public Iterable<Integer> adj(int v) {
        assert v >= 0 && v < n;
        return g[v];
    }
}
```

**3、创建类ReadGraph:**  读取图的信息，并将此算法封装在类中。而且无论是对于稀疏图还是稠密图都可以在这个算法上运行。

#### ReadGraph实现关键：

1. 创建读取文件的方法readFile()，并根据文件内容在节点之间添加边。
2. 因为构造函数的传入类型定义为Graph（向上转型）使得既能传入稀疏图也能传入稠密图，所以在调用addEdge（）方法添加边时，则调用的是各自子类的方法。

```java
import java.io.BufferedInputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.util.Scanner;
import java.util.Locale;
import java.util.InputMismatchException;
import java.util.NoSuchElementException;

public class ReadGraph {

    private Scanner scanner;

    public ReadGraph(Graph graph, String filename){//
   //定义类型为Graph(向上转型)，使得在这个模板中既可以传入稀疏图又可以传入稠密图。
        readFile(filename);  //读取文件

        try {
            int V = scanner.nextInt(); //读取文本文件第一行第一个元素（代表节点个数）
            if (V < 0)
                throw new IllegalArgumentException("number of vertices in a Graph must be nonnegative");
            assert V == graph.V();  //判断读取文件的节点数和图中的节点数是一致的

            int E = scanner.nextInt(); //读取文本文件第一行第二个元素（代表临边个数）
            if (E < 0) 
                throw new IllegalArgumentException("number of edges in a Graph must be nonnegative");

            for (int i = 0; i < E; i++) { 
                int v = scanner.nextInt(); //读取第二行第一个元素(代表节点)
                int w = scanner.nextInt(); //读取第二行第二个元素(代表节点)
                assert v >= 0 && v < V;
                assert w >= 0 && w < V;
            //在这两个节点之间加一条临边,调用的是传进来的graph所对应子类的addEdge方法。     
                graph.addEdge(v, w); 
            }
        }
        catch (InputMismatchException e) {
            String token = scanner.next();
            throw new InputMismatchException("attempts to read an 'int' value from input stream, but the next token is \"" + token + "\"");
        }
        catch (NoSuchElementException e) {
            throw new NoSuchElementException("attemps to read an 'int' value from input stream, but there are no more tokens available");
        }
    }

    private void readFile(String filename){ //定义读取文件的方法
        assert filename != null;
        try {
            File file = new File(filename);
            if (file.exists()) { //如果该文件存在
                FileInputStream fis = new FileInputStream(file);
                scanner = new Scanner(new BufferedInputStream(fis), "UTF-8");
                scanner.useLocale(Locale.ENGLISH);
            }
            else
                throw new IllegalArgumentException(filename + "doesn't exist.");
        }
        catch (IOException ioe) { //无法打开该文件
            throw new IllegalArgumentException("Could not open " + filename, ioe);
        }
    }
}
```

4、测试通过文件读取图的信息

传入testG1.txt
```
testG1.txt
13 13   //13个节点和13条临边
0 5     //节点0和节点5有一条临边
4 3
0 1
9 12
6 4
5 4
0 2
11 12
9 10
0 6
7 8
9 11
5 3

testG2.txt
6 8    //6个节点和8条临边
0 1    //0和1之间有一条临边
0 2
0 5
1 2
1 3
1 4
3 4
3 5
```

```java
// 测试通过文件读取图的信息
public class Main {

    public static void main(String[] args) {

        // 使用两种图的存储方式读取testG1.txt文件
        String filename = "testG1.txt";
        SparseGraph g1 = new SparseGraph(13, false);  //g1有13个节点，无向图
        ReadGraph readGraph1 = new ReadGraph(g1, filename);
        System.out.println("test G1 in Sparse Graph:");
        g1.show();

        System.out.println();

        DenseGraph g2 = new DenseGraph(13, false);
        ReadGraph readGraph2 = new ReadGraph(g2 , filename );
        System.out.println("test G1 in Dense Graph:");
        g2.show();

        System.out.println();

        // 使用两种图的存储方式读取testG2.txt文件
        filename = "testG2.txt";
        SparseGraph g3 = new SparseGraph(6, false);
        ReadGraph readGraph3 = new ReadGraph(g3, filename);
        System.out.println("test G2 in Sparse Graph:");
        g3.show();

        System.out.println();

        DenseGraph g4 = new DenseGraph(6, false);
        ReadGraph readGraph4 = new ReadGraph(g4, filename);
        System.out.println("test G2 in Dense Graph:");
        g4.show();
    }
}

```
输出结果
```
test G1 in Sparse Graph:
vertex 0:	5	1	2	6	
vertex 1:	0	
vertex 2:	0	
vertex 3:	4	5	
vertex 4:	3	6	5	
vertex 5:	0	4	3	
vertex 6:	4	0	
vertex 7:	8	
vertex 8:	7	
vertex 9:	12	10	11	
vertex 10:	9	
vertex 11:	12	9	
vertex 12:	9	11	

test G1 in Dense Graph:
false	true	true	false	false	true	true	false	false	false	false	false	false	
true	false	false	false	false	false	false	false	false	false	false	false	false	
true	false	false	false	false	false	false	false	false	false	false	false	false	
false	false	false	false	true	true	false	false	false	false	false	false	false	
false	false	false	true	false	true	true	false	false	false	false	false	false	
true	false	false	true	true	false	false	false	false	false	false	false	false	
true	false	false	false	true	false	false	false	false	false	false	false	false	
false	false	false	false	false	false	false	false	true	false	false	false	false	
false	false	false	false	false	false	false	true	false	false	false	false	false	
false	false	false	false	false	false	false	false	false	false	true	true	true	
false	false	false	false	false	false	false	false	false	true	false	false	false	
false	false	false	false	false	false	false	false	false	true	false	false	true	
false	false	false	false	false	false	false	false	false	true	false	true	false	

test G2 in Sparse Graph:
vertex 0:	1	2	5	
vertex 1:	0	2	3	4	
vertex 2:	0	1	
vertex 3:	1	4	5	
vertex 4:	1	3	
vertex 5:	0	3	

test G2 in Dense Graph:
false	true	true	false	false	true	
true	false	true	true	true	false	
true	true	false	false	false	false	
false	true	false	false	true	true	
false	true	false	true	false	false	
true	false	false	true	false	false
```




# 图的遍历： 

**相当于在漆黑的夜里，你只能看清你站的位置和你前面的路，但你不知道每条路能够通向哪里。**搜索的任务就是，给出初始位置和目标位置，要求找到一条到达目标的路径。 

图的遍历就是要找出图中所有的点，一般有以下两种方法：

- 深度优先就是，从初始点出发，不断向前走，如果碰到死路了，就往回走一步，尝试另一条路，直到发现了目标位置。这种不撞南墙不回头的方法，即使成功也不一定找到一条好路，但好处是需要记住的位置比较少。
- 广度优先就是，从初始点出发，把所有可能的路径都走一遍，如果里面没有目标位置，则尝试把所有两步能够到的位置都走一遍，看有没有目标位置；如果还不行，则尝试所有三步可以到的位置。这种方法，一定可以找到一条最短路径，但需要记忆的内容实在很多，要量力而行。

### 1. 深度优先遍历：(Depth First Search, DFS)

基本思路：深度优先遍历图的方法是，从图中某顶点 v 出发

1. 访问顶点 v
2. 从 v 的未被访问的邻接点中选取一个顶点 w，从 w 出发进行深度优先遍历
3. 重复上述两步，直至图中所有和v有路径相通的顶点都被访问到

伪码实现：

```java
//伪码实现，类似于树的先序遍历
public void DFS(Vertex v){
    visited[v] = true;   //遍历此节点
    for(v 的每个邻接点 W){
        if(!visited[W]){ //如果没有遍历这个节点。
	    DFS(W);          //则递归去遍历它
	    }
    }
}
```

#### 连通分量

无向图G的极大连通子图称为G的最强连通分量(Connected Component)。
  注意：
　　① 任何连通图的连通分量只有一个，即是其自身
    　② 非连通的无向图有多个连通分量。
【例】下图中的G4是非连通图，它有两个连通分量H1和H2。

![](<https://img-blog.csdn.net/20170505161156256>)

**求无权图的联通分量**

```java
// 求无权图的联通分量
public class Components {

    Graph G;                    // 图的引用
    private boolean[] visited;  // 记录dfs的过程中节点是否被访问
    private int ccount;         // 记录联通分量个数
    private int[] id;           // 每个节点所对应的联通分量标记

    // 图的深度优先遍历
    void dfs( int v ){

        visited[v] = true; //遍历此节点
        id[v] = ccount;    

        for( int i: G.adj(v) ){//循环和adj()函数遍历图中v这个节点所有相连节点
            if( !visited[i] )
                dfs(i);
        }
    }

    // 构造函数, 求出无权图的联通分量。
    public Components(Graph graph){

        // 算法初始化
        G = graph;
        visited = new boolean[G.V()]; //根据图中节点数量给visited开空间
        id = new int[G.V()]; 
        ccount = 0;
        for( int i = 0 ; i < G.V() ; i ++ ){ //通过循环初始化visited，将每个元素都设为false
            visited[i] = false;
            id[i] = -1;
        }

        // 求图的联通分量
        for( int i = 0 ; i < G.V() ; i ++ ) //
            if( !visited[i] ){ //如果该节点没有被访问过(visited[i]为false)
                dfs(i);        //dfs()它可以将i和与i相连接的所有节点遍历一遍，没遍历的节点一定在另外的一个联通分量中
                ccount ++;
            }
    }

    // 返回图的联通分量个数
    int count(){
        return ccount;
    }

    // 查询点v和点w是否联通
    boolean isConnected( int v , int w ){
        assert v >= 0 && v < G.V();
        assert w >= 0 && w < G.V();
        return id[v] == id[w];
    }
}
```

#### 深度优先搜索寻路

```java
import java.util.Vector;
import java.util.Stack;

public class Path {

    private Graph G;   // 图的引用
    private int s;     // 起始点
    private boolean[] visited;  // 记录dfs的过程中节点是否被访问
    private int[] from;         // 记录路径, from[i]表示查找的路径上i的上一个节点

    // 图的深度优先遍历
    private void dfs( int v ){
        visited[v] = true;
        for( int i : G.adj(v) )
            if( !visited[i] ){
                from[i] = v;
                dfs(i);
            }
    }

    // 构造函数, 寻路算法, 寻找图graph从s点到其他点的路径
    public Path(Graph graph, int s){

        // 算法初始化
        G = graph;
        assert s >= 0 && s < G.V();    //先判断s是否越界
        visited = new boolean[G.V()];  //visited的长度定义为图G中节点个数
        from = new int[G.V()];         //from长度也定义为图G中节点个数
        for( int i = 0 ; i < G.V() ; i ++ ){ //初始默认所有节点都没有被遍历，前一个节点为-1
            visited[i] = false;
            from[i] = -1;
        }
	this.s = s;                    
	
        // 寻路算法
        dfs(s);
    }

    // 查询从s点到w点是否有路径
    boolean hasPath(int w){
        assert w >= 0 && w < G.V();
        return visited[w];     //如果w被遍历了，证明s和w之间有路径（同一个连通分量）
    }

    // 查询从s点到w点的路径, 存放在vec中
    Vector<Integer> path(int w){

        assert hasPath(w) ;

        Stack<Integer> s = new Stack<Integer>();
        // 通过from数组逆向查找到从s到w的路径, 存放到栈中
        int p = w;           //设置指针p指向当前节点w
        while( p != -1 ){    //不是起始节点。因为起始节点的前一个节点是-1，而当p==-1时，证明起始节点已经压入栈中，则退出循环
            s.push(p);       //将当前节点压入栈中
            p = from[p];     //指针p指向当前节点的前一个节点
        }

        // 从栈中依次取出元素, 获得顺序的从s到w的路径
        Vector<Integer> res = new Vector<Integer>();
        while( !s.empty() )   //只要栈不为空，则出栈
            res.add( s.pop() );

        return res;
    }

    // 打印出从s点到w点的路径
    void showPath(int w){

        assert hasPath(w) ;        

        Vector<Integer> vec = path(w);  
        for( int i = 0 ; i < vec.size() ; i ++ ){ //
            System.out.print(vec.elementAt(i));
            if( i == vec.size() - 1 )  //遍历到了最后一个节点
                System.out.println();
            else                       
                System.out.print(" -> ");
        }
    }
}
```

测试寻路算法

传入文件testG.txt
```
7 8   //该图有7个节点8条临边
0 1   //节点0和节点1之间有一条临边
0 2   
0 5
0 6
3 4
3 5
4 5
4 6
```
```java
public class Main {

    // 测试寻路算法
    public static void main(String[] args) {

        String filename = "testG.txt";
        SparseGraph g = new SparseGraph(7, false);
        ReadGraph readGraph = new ReadGraph(g, filename);
        g.show();
        System.out.println();

        Path path = new Path(g,0);
        System.out.println("Path from 0 to 6 : ");
        path.showPath(6);
    }
}
```
结果表示
```
vertex 0:	1	2	5	6	
vertex 1:	0	
vertex 2:	0	
vertex 3:	4	5	
vertex 4:	3	5	6	
vertex 5:	0	3	4	
vertex 6:	0	4	

Path from 0 to 6 : 
0 -> 5 -> 3 -> 4 -> 6
```

###  2. 广度优先搜索：(Breadth First Search, BFS)--无权图最短路径

广度优先搜索，可以被形象地描述为 "浅尝辄止"，它也需要借助一个**队列**以保持遍历过的顶点顺序，以便按出队的顺序再去访问这些顶点的邻接顶点。 

**实现思路：**

1. 顶点 s 入队列,标记s已被访问，把s的顺序标记为0
2. 开始循环，当队列非空时则继续执行，否则结束
3. 出队列取得队头节点并记作 v
4. 查找顶点 v 的第一个邻接顶点 i。(通过循环迭代adj()方法)
5. 若 v 的邻接顶点 i 未被访问过的，则把节点 i 加入队列，标记节点 i 已被访问， 记录路径，即把v存进from[i]表示查找的路径上i的上一个节点，并在遍历次序加一。
6. 查找顶点v 的另一个新的邻接顶点 i`，转到步骤 5 入队列，直到顶点 v 的所有未被访问过的邻接点处理完。转到步骤 2，结束循环

要理解深度优先和广度优先搜索，首先要理解搜索步，一个完整的搜索步包括两个处理 

1. 获得当前位置上，有几条路可供选择
2. 根据选择策略，选择其中一条路，并走到下个位置



```java
import java.util.Vector;
import java.util.Stack;
import java.util.LinkedList;
import java.util.Queue;

public class ShortestPath {

    private Graph G;   // 图的引用
    private int s;     // 起始点
    private boolean[] visited;  // 记录dfs的过程中节点是否被访问
    private int[] from;         // 记录路径, from[i]表示查找的路径上i的上一个节点
    private int[] ord;          // 记录路径中节点的次序。ord[i]表示i节点在路径中的次序。


    // 构造函数, 寻路算法, 寻找图graph从s点到其他点的路径
    public ShortestPath(Graph graph, int s){

        // 算法初始化
        G = graph;
        assert s >= 0 && s < G.V();

        visited = new boolean[G.V()];  //是否已经遍历
        from = new int[G.V()];
        ord = new int[G.V()];
        for( int i = 0 ; i < G.V() ; i ++ ){
            visited[i] = false;
            from[i] = -1;
            ord[i] = -1;
        }
        this.s = s;

        // 无向图最短路径算法, 从s开始广度优先遍历整张图
        Queue<Integer> q = new LinkedList<Integer>();

        q.add(s);               //把点s加入队列
        visited[s] = true;      //点s已经遍历
        ord[s] = 0;             //记录点s遍历的顺序
        while( !q.isEmpty() ){  //如果队列不为空
            int v = q.remove(); //把点s移除队列记录为v
            for( int i : G.adj(v) ) //通过循环和adj函数遍历与v相邻的节点i
                if( !visited[i] ){  //如果i没有被遍历
                    q.add(i);       //就把i加进队列
                    visited[i] = true; //遍历节点i
                    from[i] = v;    //把v存进from[i]表示查找的路径上i的上一个节点
                    ord[i] = ord[v] + 1;  //次序加一
                }
        }
    }

    // 查询从s点到w点是否有路径
    public boolean hasPath(int w){
        assert w >= 0 && w < G.V(); 
        return visited[w];
    }

    // 查询从s点到w点的路径, 存放在vec中
    public Vector<Integer> path(int w){

        assert hasPath(w) ;

        Stack<Integer> s = new Stack<Integer>();
        // 通过from数组逆向查找到从s到w的路径, 存放到栈中
        int p = w;
        while( p != -1 ){
            s.push(p);
            p = from[p];
        }

        // 从栈中依次取出元素, 获得顺序的从s到w的路径
        Vector<Integer> res = new Vector<Integer>();
        while( !s.empty() )
            res.add( s.pop() );

        return res;
    }

    // 打印出从s点到w点的路径
    public void showPath(int w){

        assert hasPath(w) ;

        Vector<Integer> vec = path(w);
        for( int i = 0 ; i < vec.size() ; i ++ ){
            System.out.print(vec.elementAt(i));
            if( i == vec.size() - 1 )
                System.out.println();
            else
                System.out.print(" -> ");
        }
    }

    // 查看从s点到w点的最短路径长度
    // 若从s到w不可达，返回-1
    public int length(int w){
        assert w >= 0 && w < G.V();
        return ord[w];
    }
}
```

**测试无权图最短路径算法**
导入文件
```
//testG.txt
7 8   //7个节点8个临边
0 1   //0和1之间有一条临边
0 2
0 5
0 6
3 4
3 5
4 5
4 6

//testG1.txt
13 13 //13个节点和13条临边
0 5   //0和5之间有一条临边
4 3
0 1
9 12
6 4
5 4
0 2
11 12
9 10
0 6
7 8
9 11
5 3
```
开始测试
```java
public class Main {

    // 测试无权图最短路径算法
    public static void main(String[] args) {

        String filename = "testG.txt";
        SparseGraph g = new SparseGraph(7, false);
        ReadGraph readGraph = new ReadGraph(g, filename);
        g.show();

        // 比较使用深度优先遍历和广度优先遍历获得路径的不同
        // 广度优先遍历获得的是无权图的最短路径
        Path dfs = new Path(g,0);
        System.out.print("DFS : ");
        dfs.showPath(6);

        ShortestPath bfs = new ShortestPath(g,0);
        System.out.print("BFS : ");
        bfs.showPath(6);

        System.out.println();

        filename = "testG1.txt";
        SparseGraph g2 = new SparseGraph(13, false);
        ReadGraph readGraph2 = new ReadGraph(g2, filename);
        g2.show();

        // 比较使用深度优先遍历和广度优先遍历获得路径的不同
        // 广度优先遍历获得的是无权图的最短路径
        Path dfs2 = new Path(g2,0);
        System.out.print("DFS : ");
        dfs2.showPath(3);

        ShortestPath bfs2 = new ShortestPath(g,0);
        System.out.print("BFS : ");
        bfs.showPath(3);
    }
}
```
输出结果
```
vertex 0:	1	2	5	6	
vertex 1:	0	
vertex 2:	0	
vertex 3:	4	5	
vertex 4:	3	5	6	
vertex 5:	0	3	4	
vertex 6:	0	4	
DFS : 0 -> 5 -> 3 -> 4 -> 6
BFS : 0 -> 6

vertex 0:	5	1	2	6	
vertex 1:	0	
vertex 2:	0	
vertex 3:	4	5	
vertex 4:	3	6	5	
vertex 5:	0	4	3	
vertex 6:	4	0	
vertex 7:	8	
vertex 8:	7	
vertex 9:	12	10	11	
vertex 10:	9	
vertex 11:	12	9	
vertex 12:	9	11	
DFS : 0 -> 5 -> 4 -> 3
BFS : 0 -> 5 -> 3
```

## 最短路径算法 (Shortest Path Algorithm)

1. 无权图：

问题：在图中找到某一个顶点到其它所有点的距离

对于初始点 v 来说，某个点的 d 代表该点到初始点的距离。

基本步骤：

1. 将所有点的距离 d 设为无穷大
2. 挑选初始点 s，将 ds 设为 0，将 shortest 设为 0
3. 找到所有距离为 d 为 shortest 的点，查找他们的邻接链表的下一个顶点 w，如果 dw 的值为无穷大，则将 dw 设为 shortest + 1
4. 增加 shortest 的值，重复步骤 3，直到没有顶点的距离值为无穷大



![img](https://pic3.zhimg.com/80/v2-f38af68ed88323fab87de68cf56be212_hd.png)

\2. 有权图：



在有权图中，常见的最短路径算法有 Dijkstra 算法 Floyd 算法

**迪杰斯特拉 Dijkstra 算法：Dijkstra 算法适用于权值为正的的图**

Dijkstra 算法属于单源算法，即只能求出某点到其它点最短距离，并不能得出任意两点之间的最短距离。

算法步骤：

1. 将所有边初始化为无穷大
2. 选择一个开始的顶点，添加到优先队列中
3. 对于该点的所有邻接顶点进行判断，如果到该点的距离小于原先的值，则将该值进行更新
4. 将该点所有邻接顶点添加到优先队列中
5. 从优先队列中挑选出一个路径值最小的顶点，将其弹出，作为新的顶点，重复步骤 3，4，5
6. 直到所有点都被处理过一次

例如：

![img](https://pic2.zhimg.com/80/v2-26c8c539a7197a62e4a356cd9dfda891_hd.png)

首先选取 v0 作为起始点，添加到优先队列中，将v0弹出，然后对 v0 邻接点进行判断，由于一开始所有边都为无穷大，那么 <v0, v1> 和 <v0, v3> 都更新，值为 2 和 1，按路径大小升序将v3、v1添加到优先队列。

![img](https://pic2.zhimg.com/80/v2-8396dfb4df2801cd3d10b25f99528bbd_hd.png)

之后将 v3 弹出，对所有 v3 邻接点进行值的更新，并将所有邻接点按路径大小升序添加到优先队列中，若遇到值相同，则无所谓其先后顺序

![img](https://pic2.zhimg.com/80/v2-624a0f575b509298722914b9849061ed_hd.png)

![img](https://pic1.zhimg.com/80/v2-861048c05e812564557b0a68b584e5ec_hd.png)

重复这样的过程，直到所有的点都被处理过，则算法终止，这样最后可以得出从 v0 到其它 v1~v6 节点的距离。

Dijkstra 算法适合于权值为正的情况下，若权值为负则不能使用，因为出现死循环。这时候我们需要计算每个顶点被处理的次数，当某个顶点已经处理过的话，就跳出该循环。

佛洛伊德 Floyd 算法：可以求出任意两点的最短距离

Floyd 算法是一个经典的动态规划算法。用通俗的语言来描述的话，首先我们的目标是寻找从点 i 到点 j 的最短路径。

从任意节点 i 到任意节点 j 的最短路径不外乎 2 种可能：

1. 是直接从 i 到 j
2. 是从 i 经过若干个节点 k 到 j

所以，我们假设 Dis(i,j) 为节点 u 到节点 v 的最短路径的距离，对于每一个节点 k，我们检查Dis(i,k) + Dis(k,j) < Dis(i,j) 是否成立，如果成立，证明从 i 到 k 再到 j 的路径比 i 直接到 j 的路径短，我们便设置 Dis(i,j) = Dis(i,k) + Dis(k,j)，这样一来，当我们遍历完所有节点 k，Dis(i,j) 中记录的便是 i 到 j 的最短路径的距离。

时间复杂度： O(n^3)

```java
for(int k=0; k<n; k++) { 
    for(i=0; i<n; i++) {
         for(j=0; j<n; j++)
             if(A[i][j]>(A[i][k]+A[k][j])) {
                  A[i][j]=A[i][k]+A[k][j];
                  path[i][j]=k;
             } 
    }
} 
```

## 最小生成树 (Minimum Spanning Trees MST)

例如：要在 n 个城市之间铺设光缆，主要目标是要使这 n 个城市的任意两个之间都可以通信，但铺设光缆的费用很高，且各个城市之间铺设光缆的费用不同，因此另一个目标是要使铺设光缆的总费用最低。这就需要找到带权的最小生成树。

特点：

1. 该树是连通的
2. 权值之和最小
3. 边数比顶点个数少 1

存在个数：最小生成树在一些情况下可能会有多个

1. 当图的每一条边的权值都相同时，该图的所有生成树都是最小生成树
2. 如果图的每一条边的权值都互不相同，那么最小生成树将只有一个

比如：

![img](https://pic2.zhimg.com/80/v2-a5c14efbff6176183849d33a2041db31_hd.png)

生成最小生成树的算法一般有两种，分别是 Prim 算法和 Kruskal 算法

**1. 普里姆算法 (Prim 算法):**

算法步骤：

1. 输入：一个加权连通图，其中顶点集合为 V，边集合为 E
2. 初始化：Vnew = {x}，其中 x 为集合 V 中的任一节点(起始点)，Enew = {} 为空
3. 在集合 E 中选取权值最小的边 <u, v>，其中 u 为集合 Vnew 中的元素，而 v 不在 Vnew 集合当中，并且 v∈V (如果存在有多条满足前述条件即具有相同权值的边，则可任意选取其中之一）
4. 将 v 加入集合 Vnew 中，将 <u, v> 边加入集合 Enew 中
5. 重复步骤 3、4 直到 Vnew = V

时间复杂度：O(V^2)

**2. Kruskal 算法：需要一个集合用来升序存储所有边**

算法步骤：

1. 先构造一个只含有 n 个顶点，而边集为空的子图
2. 从边集 E 中选取一条权值最小的边，若该条边的两个顶点分属不同的树，则将其加入子图。(也就是说，将这两个顶点分别所在的两棵树合成一棵树) 反之，若该条边的两个顶点已落在同一棵树上，则不可取，而应该取下一条权值最小的边再试之
3. 重复步骤 2，直到所有点连通

时间复杂度：O( ElogV )

例如：



![img](https://pic1.zhimg.com/80/v2-67ebcdf2a0cb6da002a0f2ca2d00ee30_hd.jpg)

在对所有边进行排序之后，我们得到一个边集合，从边集合中取出最小权的边 AD



![img](https://pic1.zhimg.com/80/v2-2b9696d89cae9dbac1f19b669d31ef24_hd.jpg)

剩下的边中寻找。我们找到了 CE。这里边的权重也是 5，依次类推我们找到了 6,7,7



![img](https://pic3.zhimg.com/80/v2-3ac7535fbf1707fee787b89f6145afee_hd.jpg)

尽管现在长度为 8 的边是最小的未选择的边。但是他们已经连通了





![img](https://pic4.zhimg.com/80/v2-3782ccd91886fb4c78efa271ed0595bf_hd.jpg)

最后就剩下 EG 和 FG 了。当然我们选择了 EG



