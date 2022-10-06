# 上海汉得Java岗面试小结

# 汉得面试

## 1、Java1.8有什么新特性吗？

### 1.1、接口

> 1）接口中jdk1.7只能有普通方法和抽象方法==子类是抽象类的时候可以实现也可以不实现==。而jdk1.8相对于jdk1.7新增了 **静态方法和默认方法，静态方法是不能被重写的。默认方法是可以被重写也可以不重写的**

### 1.2、 foreach

> 2）==forEach( )方法是Java8新增的一个遍历集合【数组也可以】元素的方法==，它被定义在`Java.lang.Iterable` 接口中。List、Map、Set、Stream等接口都实现了这个方法，所以可以直接使用这些类的实例化对象调用forEach方法遍历元素。

![image-20220912195612578](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220912195612578.png)

![image-20220912195641890](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220912195641890.png)

### 1.3、Arraylist 

> **3）Arraylist 初始化**

饿汉式和懒汉式。

jdk8延迟分配内存空间，只有等调用add方法的时候才会分配最小容量 = 10 。

**JDK7中的ArrayList对象的创建类似于单例模式中的饿汉式，而JDK8中的ArrayList对象的的创建类似于单例模式中的懒汉式，延迟了数组的创建，不会在一开始创建对象时，就为底层数组开辟内存空间，节省了内存空间资源**。

ArrayList集合首次调用add()方法添加元素时，将minCapacity最小所需容量直接置为常量DEFAULT_CAPACITY为10，只会生效一次，接下来会接着进行ensureExplicitCapacity(minCapacity)方法的调用，调用此方法是为了"确保当前底层数组容量"是否足够对新元素的添加,如果不够，则需要进行grow(int minCapacity）真正实现对底层数组的扩容。

### 1.4、hashmap

**4）hashmap** 

> jdk7是一开就创建容量为16，jdk8是先创建{ }==当调用put方法时才初始化容量为16==
> jdk7是数组+链表 ==jdk8是数组+链表+红黑树（链表长度大于8且节点数组长度大于64 转化成红黑树）==



## 2、Java集合初始容量和扩容

**List** 元素是有序的、可重复。

**Set** 元素无序的、不可重复。

==长度 * 加载因子 = 阈值==

**加载因子**：表示某个阀值，用0~1之间的小数来表示，当已有元素占比达到这个阀值后。

> 加载因子的系数小于等于1，意指  即当 元素个数 超过 ==容量长度 * 加载因子的系数== 时，进行扩容。



|   **Class**   | **初始大小** | **加载因子** | **扩容倍数** | **底层实现**                                                 | **Code**                                                     | **是否线程安全** | **同步方式**                                                 |
| :-----------: | ------------ | ------------ | ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------- | ------------------------------------------------------------ |
|   ArrayList   | 10           | 1            | 1.5倍        | Object数组                                                   | int newCapacity = oldCapacity + (oldCapacity >> 1); ">>"右移符号，所以是除以2，所以新的容量是就的1.5倍 ==Arrays.copyOf 调用 System.arraycopy 扩充数组== | 线程不安全       | -                                                            |
|    Vector     | 10           | 1            | 2倍          | Object数组                                                   | int newCapacity = oldCapacity + ((capacityIncrement > 0) ? capacityIncrement : oldCapacity); capacityIncrement默认为0，所以是加上本身，也就是2*oldCapacity，2倍大小 Arrays.copyOf 调用 System.arraycopy 扩充数组 | 线程安全         | synchronized                                                 |
|    HashSet    | 16           | 0.75f        | 2倍          | HashMap<E,Object>                                            | add方法实际调用HashMap的方法put                              | 线程不安全       |                                                              |
|    HashMap    | 16           | 0.75f        | 2倍          | resize方法                                                   | `void addEntry(int hash, K key, V value, int bucketIndex) {   if ((size >= threshold) && (null != table[bucketIndex])) {     resize(2 * table.length);     hash = (null != key) ? hash(key) : 0;     bucketIndex = indexFor(hash, table.length);   }    createEntry(hash, key, value, bucketIndex); }` | 线程不安全       |                                                              |
|   Hashtable   | 11           | 0.75f        | 2倍+1        | Hashtable.Entry数组                                          | int newCapacity = (oldCapacity << 1) + 1; if (newCapacity - MAX_ARRAY_SIZE > 0) {   if (oldCapacity == MAX_ARRAY_SIZE)     return;   newCapacity = MAX_ARRAY_SIZE; } | 线程安全         | 所有的元素操作都是 synchronized 修饰的                       |
| StringBuilder | 16           | 1            | 2倍+2        |                                                              | value = Arrays.copyOf<br />(value,newCapacity<br />,minimumCapacity) << coder); | 线程不安全       |                                                              |
| StringBuffer  | 16           | 1            | 2倍+2        | 在进行字符串append添加的时候，会先计算添加后字符串大小，传入一个方法：==ensureCapacityInternal== 这个方法进行是否扩容的判断，需要扩容就调用==expandCapacity==方法进行扩容 |                                                              | 线程安全         | `StringBuffer重写了length()和capacity()、append等方法，在他们的方法上面都有synchronized 关键字实现线程同步` |

## 3、线程池

==执行逻辑：==

    1. 当线程数小于核心线程数时，创建线程。
    2. 当线程数大于等于核心线程数，且任务队列未满时，将任务放入任务队列。
    3. 当线程数大于等于核心线程数，且任务队列已满
        -1 若线程数小于最大线程数，创建线程
        -2 若线程数等于最大线程数，抛出异常，拒绝任务

**corePoolSize**【1】:指定了线程池中的线程数量，它的数量决定了添加的任务是开辟新的线程去执行，还是放到workQueue任务队列中去；

**maximumPoolSize**【Integer.MAX_VALUE】:指定了线程池中的最大线程数量，这个参数会根据你使用的workQueue任务队列的类型，决定线程池会开辟的最大线程数量；

**keepAliveTime**【60s】:当线程池中空闲线程数量超过corePoolSize时，多余的线程会在多长时间内被销毁；

allowCoreThreadTimeout【false】：

**queueCapacity**【Integer.MAX_VALUE】：任务队列容量（阻塞队列）

**timeUnit**:keepAliveTime的单位【TimeUnit.MILLISECONDS;】

**workQueue**:任务队列，被添加到线程池中，但尚未被执行的任务；它一般分为直接提交队列、有界任务队列、无界任务队列、优先任务队列几种；【**直接提交队列、数组有界任务队列、链表无界任务队列、Comparator优先任务队列**】

> new LinkedBlockingQueue<>(maximumPoolSize)

**threadFactory**:线程工厂，用于创建线程，一般用默认即可；

**handler**:拒绝策略；当任务太多来不及处理时，如何拒绝任务；RejectedExecutionHandler  

```java
1、corePoolSize：核心线程数
    * 核心线程会一直存活，即使没有任务需要执行
    * 当线程数小于核心线程数时，即使有线程空闲，线程池也会优先创建新线程处理
    * 设置allowCoreThreadTimeout=true（默认false）时，核心线程会超时关闭
2、queueCapacity：任务队列容量（阻塞队列）
    * 当核心线程数达到最大时，新任务会放在队列中排队等待执行
3、maxPoolSize：最大线程数
    * 当线程数>=corePoolSize，且任务队列已满时。线程池会创建新线程来处理任务
    * 当线程数=maxPoolSize，且任务队列已满时，线程池会拒绝处理任务而抛出异常
4、 keepAliveTime：线程空闲时间
    * 当线程空闲时间达到keepAliveTime时，线程会退出，直到线程数量=corePoolSize
    * 如果allowCoreThreadTimeout=true，则会直到线程数量=0
5、allowCoreThreadTimeout：允许核心线程超时
6、rejectedExecutionHandler：任务拒绝处理器
    * 两种情况会拒绝处理任务：
        - 当线程数已经达到maxPoolSize，切队列已满，会拒绝新任务
        - 当线程池被调用shutdown()后，会等待线程池里的任务执行完毕，再shutdown。如果在调用shutdown()和线程池真正shutdown之间提交任务，会拒绝新任务
    * 线程池会调用rejectedExecutionHandler来处理这个任务。如果没有设置默认是AbortPolicy，会抛出异常
    * ThreadPoolExecutor类有几个内部实现类来处理这类情况：
        - 【AbortPolicy】 丢弃任务，抛运行时异常
        - CallerRunsPolicy 执行任务
        - DiscardPolicy 忽视，什么都不会发生
        - DiscardOldestPolicy 从队列中踢出最先进入队列（最后一个执行）的任务
    * 实现RejectedExecutionHandler接口，可自定义处理器
    
```



> 		==任务队列==
>
> 	1. 直接提交：如果达到maximumPoolSize设置的最大值，则根据你设置的handler执行拒绝策略。
> 	2. 有界任务：先考虑你设置的等待队列长度的界限，如果超过了这个界限，再以maximumPoolSize为上限。
> 	3. 无界任务：队列无限大，也就是说maximumPoolSize没意义了。
> 	4. 优先任务：值越小优先级越高。PriorityBlockingQueue



## 4、JVM 内存模型

**线程私有区域:程序计数器、Java虚拟机栈、本地方法栈
线程共享区域:Java堆、方法区、运行时常量池**

程序计数器：程序计数器是每个线程所私有的。记录着当前线程所执行的字节码的行号指示器。执行native本地方法时，程序计数器的值为空。

Java虚拟机栈：由于每个线程正在执行的方法可能不同，因此每个线程都会有一个自己的Java栈，互不干扰。

							Java栈中存放的是一个个的栈帧，每个栈帧对应一个被调用的方法。

本地方法栈：为执行本地方法（Native Method）服务的。

堆：Java中的堆是用来存储对象本身的以及数组。堆是被所有线程共享的，在JVM中只有一个堆。

方法区：方法区在JVM中也是一个非常重要的区域，它与堆一样，是被线程共享的区域。==在方法区中，存储了每个类的信息（包括类的名称、方法信息、字段信息）、静态变量、常量以及编译器编译后的代码等。==

运行时常量池：它是每一个类或接口的常量池的运行时表示形式。

## 5、Java 封装的常见的数据结构，说说各自的特点

- 哈希表(Hash)

是一种可以通过关键码值（Key-Value）直接访问的数据结构，可以实现快速查询、插入、删除。

- 队列(Queue)

队列是一种特殊的线性表，它只允许在表的前端进行删除操作，而在表的后端进行插入操作。

- 树(Tree)

树是一种非线性结构，由n(n>0)个有限结点组成有层次关系的集合。

二叉树：每个结点最多含有2个子树。



==完全二叉树：叶子节点出现在最后一层或者倒数第二层，不能再往上，且数据向左靠齐。==



==满二叉树：==每个结点都有左右子树。（特殊的完全二叉树）



==二叉查找树：==

任意结点的左子树不为空，左子树所有结点的值均小于根结点的值。
任意结点的右子树不为空，右子树所有结点的值均大于根节点的值。
任意结点的左右子树也是一颗二叉查找树。



平衡二叉树：也称AVL树，当且仅当任何结点的两棵子树的高度差不大于1的二叉树。==Java中HashMap的红黑树就是平衡二叉树==！

> 在一棵平衡二叉树中，节点的平衡因子只能取 0 、1 或者 -1 ，分别对应着左右子树等高，左子树比较高，右子树比较高。



B树：一种对读写优化的自平衡二叉树，在数据库的索引中常见的BTREE就是自平衡二叉树。



B+树：B+树是应文件系统所需而产生的B树的变形树。

所有的非终端结点可以看成是索引部分，结点中仅含有其子树根结点中最大（或最小）关键字
所有的叶子结点中包含了全部关键字的信息，及指向含有这些关键字记录的指针，且叶子结点本身依关键字的大小自小而大的顺序链接。
有m个子树的中间节点包含有m个元素（B树中是k-1个元素），每个元素不保存数据，只用来索引。



**Java8中HashMap的红黑树：**
实质上就是 平衡二叉树 ，通过颜色约束二叉树的平衡：

1）每个节点都只能是红色或者黑色

2）根节点是黑色

3）每个叶节点（NIL 节点，空节点）是黑色的。

4）如果一个节点是红色的，则它两个子节点都是黑色的。也就是说在一条路径上不能出现相邻的两个红色节点。

5）从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点。



- 堆(Heap)
    - 堆可以被看成一个树的数组对象，具有如下特点：
    - 堆是一颗完全二叉树。
    - 大根堆，小根堆。



- 数组(Array)

数组是一种 **线性表** 的数据结构， **连续的空间** 存储 **相同类型** 的数据。

优点：查询速度快。

缺点：数组在创建时大小确定，无法扩容。

- 栈(Stack)

栈可以类比为水桶，只有一端能够进出，遵循的先进后出的规则。栈先进的元素进入栈底，读元素的时候从栈顶取元素。

- 链表(Linked List)

链表是一种线性表的链式存储方式，链表的内存是不连续的。

查找慢，插入和删除快。单向、双向和循环。

- 图(Graph)

一个图就是一些顶点的集合，这些顶点通过一系列边结对（连接）。顶点用圆圈表示，边就是这些圆圈之间的连线。顶点之间通过边连接。节点之间的关系是任意的，图中任意两个数据元素之间都有可能相关。

## 6、事务的特点、mysql事务隔离级别

原子性【要么成功，要么失败】、

一致性【数据库的完整性约束没有被破坏】、

隔离性【在同一时间仅有一个事务请求用于同一数据。】、

持久性【事务一旦完成，数据是不可逆的】

> 针对未提交成功，可能会回滚的数据，有脏读。

> 针对更新操作，有可重复读和不可重复读。

> 针对插入操作，有幻读。

1. 读未提交（READ UNCOMMITTED）
2. 读提交 （READ COMMITTED）
3. 可重复读 （REPEATABLE READ）
4. 串行化 （SERIALIZABLE）

MySQL 的隔离级别是 ==REPEATABLE-READ==，也就是可重复读，这也是 MySQL 的默认级别。

**修改隔离级别的语句是：set [作用域 SESSION 或者 GLOBAL] transaction isolation level [事务隔离级别]**

{ READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | serializable } 

读未提交：任何事务对数据的修改都会第一时间暴露给其他事务，即使事务还没有提交。==没有事务==

读提交：同一事务不同时刻读到的数据值可能不一致。没有解决不可重复读的问题。==有事务，但是互相干扰，解决了脏读的问题==

可重复读 **MVVC (多版本并发控制)** ：==事务不会读到其他事务对已有数据的修改，即使其他事务已提交。==也就是说，事务开始时读到的已有数据是什么，在事务提交前的任意时刻，这些数据的值都是一样的。**对已有数据的修改 事务之间不会相互影响，解决了不可重复读的问题**

> 可重复读是在事务开始的时候生成一个当前事务全局性的快照，而读提交则是每次执行语句的时候都重新生成一次快照。

串行化：它将事务的执行变为顺序执行，相当于单线程，读的时候加共享锁，也就是其他事务可以并发读，但是不能写。写的时候加排它锁，其他事务不能并发写也不能并发读。

==MySQL 把行锁和间隙锁【在数据库中会为索引维护一套B+树，用来快速定位行记录。】合并在一起，解决了并发写和幻读的问题，这个锁叫做 Next-Key锁。==



## 7、TCP三次握手 四次挥手

为什么是三次？

==由于 TCP 的报头设计，可以将请求功能和确认功能放在一个数据包里，通过 Flag 同时置位 SYN 和 ACK 。==

```
1、 SYN：同步连接序号，TCP SYN报文就是把这个标志设置为1，来请求建立连接；
2、 ACK：请求/应答状态。0为请求，1为应答；
```

![preload](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/2mkookb1q4.png)

![TCP_3_HANDSHAKE](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/01-three-handshake.png)

TCP为什么可靠：因为它保证了传送数据包的顺序。顺序是用一个序列号来保证的。响应包内也包括一个序列号，表示接收方准备好这个序列号的包。在TCP传送一个数据包时，它会把这个数据包放入重发队列中，同时启动计时器，如果收到了关于这个包的确认信息，便将此数据包从队列中删除，如果在计时器超时的时候仍然没有收到确认信息，则需要重新发送该数据包。另外，TCP通过数据分段中的序列号来保证所有传输的数据可以按照正常的顺序进行重组，从而保障数据传输的完整。

**客户端发送一个TCP的SYN标志位置1的包指明客户打算连接的服务器的端口，以及初始序号X，保存在包头的序列号(Sequence Number)字段里。**

![image-20220920201138405](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220920201138405.png)

**服务器发回确认包(ACK)应答。即SYN标志位和ACK标志位均为1同时，将确认序号(Acknowledgement Number)设置为客户的序列号加1，即 ack = X+1。**

![image-20220920201219572](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220920201219572.png)

**客户端再次发送确认包(ACK) SYN标志位为0,ACK标志位为1，并且把服务器发来ACK的序号字段+1，放在确定字段中发送给对方，并且在数据段放 ISN+1**

![image-20220920201243729](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220920201243729.png)



![preview](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/v2-f91390b2a3c308cdc661178ae9a93331_r.jpg)

## 8、InnoDB与MyISAM区别

**InnoDB支持事务，MyISAM不支持，对于InnoDB每一条SQL语言都默认封装成事务，自动提交**

**InnoDB支持外键，而MyISAM不支持。**

 **InnoDB是聚集索引，使用B+Tree作为索引结构，数据文件是和（主键）索引绑在一起的（表数据文件本身就是按B+Tree组织的一个索引结构）**

**InnoDB支持表、行(默认)级锁，而MyISAM支持表级锁**

**InnoDB表必须有唯一索引（如主键）**

## 9、== 和 equals 的区别，在什么情况下需要重写equals方法 

正常情况下，Java的基本数据类型和包装数据类型都已经重写的Object类的 equals 方法和 hashCode 方法。

==判断两个对象在逻辑上是否相等，如根据类的成员变量来判断两个类的实例是否相等==，而==继承Object中的equals方法只能判断两个引用变量是否是同一个对象。==这样我们往往需要重写equals()方法。

==想要比较引用类型的值，就重写equals==

> 加入到hashset中的自定义类的对象，为确保他们不重复，需要对他们的类重写equals()和hashcode()的方法。
>
> 如果不重写equals，相同内容不同引用的对象会被当做不同的对象被加入到hashset中。



## 10、Spring 源码知道多少

### bean生命周期

实例化 -> 属性赋值 -> 初始化 -> 销毁

```java
getBean` --> `doGetBean` --> `createBean` --> `doCreateBean`
```

两种方式、xml bean 配置 和 注解 @Bean

首先通过 BeanDefinitionRegistry 的registry方法 拿到一个 BeanDefinition对象

通过beanName和classType就可知道需要的对象的哪个。

之后通过populateBean方法完成属性赋值（autowireByName和autowireByType @Resource）

最后使用InitializingBean 完成实例化。

DisposableBean destroy 销毁

```java
ApplicationContext ac = new FileSystemXmlApplicationContext("applicationContext.xml"); 
ac.getBean("userService");
```

### bean 作用域

singleton	在spring IoC容器仅存在一个Bean实例，Bean以单例方式存在，bean作用域范围的默认值。
prototype	每次从容器中调用Bean时，都返回一个新的实例，即每次调用getBean()时，相当于执行newXxxBean()。
request	每次HTTP请求都会创建一个新的Bean，该作用域仅适用于web的Spring WebApplicationContext环境。
session	同一个HTTP Session共享一个Bean，不同Session使用不同的Bean。该作用域仅适用于web的Spring WebApplicationContext环境。
application	限定一个Bean的作用域为ServletContext的生命周期。该作用域仅适用于web的Spring WebApplicationContext环境。

## 11、Spring 中用到的设计模式，在哪用到了

工厂模式：Spring使用工厂模式可以通过 `BeanFactory` 或 `ApplicationContext` 创建 bean 对象。

单例模式：线程池、缓存、日志等全局只有一个实例，**Spring中bean的默认作用域就是singleton(单例)的** scope="singleton"还有request和session

代理模式：动态代理，说白了，其实就是把创建代理对象的过程，用反射来实现，==代理类此时就不需要去实现被代理对象的接口了，因此也就和被代理对象解耦了。==动态代理，不需要把代理类写死了，这样代理的功能就不是固定的了。注意。这里反射出来的代理对象，被代理对象仍然是创建出来然后当作参数传入的。InvocationHandler和Proxy
InvocationHandler：是一个接口，动态代理类需要实现这个接口，然后调用Proxy来创建被代理对象和调用代理功能。里面有一个调用处理的抽象方法：Object invoke(Object proxy, 方法 method, Object[] args)；

适配器模式：在Spring MVC中，`DispatcherServlet` 根据请求信息调用 `HandlerMapping`，解析请求对应的 `Handler`。解析到对应的 `Handler`（也就是我们平常说的 `Controller` 控制器）后，开始由`HandlerAdapter` 适配器处理。`HandlerAdapter` 作为期望接口，具体的适配器实现类用于对目标类进行适配，`Controller` 作为需要适配的类。==这样如果增加一个controller，就不用修改原有代码的逻辑，只需要新增一个对应的适配器类就可以了，符合开闭原则。最后找到那个适配器类统一调用handle方法就可以完成controller的作用。==

## 12、hadoop是做什么的，kafka消息队列知道多少，docker怎么用的

### hadoop

- 分布式文件系统 HDFS(Hadoop Distributed File System)
- 分布式计算系统 MapReduce
- 分布式资源管理系统 YARN

HDFS是以分布式进行存储的文件系统，主要负责集群数据的存储和读取。主要包括一个NameNode, 一个Secondary NameNode备份, 多个DataNode。

MapReduce

实体一：**客户端**，用来提交MapReduce作业。

实体二：**JobTracker**，用来协调作业的运行。

实体三：**TaskTracker**，用来处理作业划分后的任务。

实体四：**HDFS**，用来在其它实体间共享作业文件。



**YARN 负责将系统资源分配给在 Hadoop 集群中运行的各种应用程序，并调度要在不同集群节点上执行的任务。**

YARN 总体上是 master/slave 结构，在整个资源管理框架中，ResourceManager 为 master，NodeManager 是 slave。

ResourceManager是Master上一个独立运行的进程，负责集群统一的资源管理、调度、分配等等；

NodeManager是Slave上一个独立运行的进程，负责上报节点的状态；

ApplicationMaster相当于这个Application的监护人和管理者，负责监控、管理这个Application的所有Attempt在cluster中各个节点上的具体运行，同时负责向Yarn ResourceManager申请资源、返还资源等；

Container是yarn中分配资源的一个单位，包涵内存、CPU等等资源，YARN以Container为单位分配资源；

-----

**Docker**是一个容器化平台，它以容器的形式将你的应用程序及所有的依赖项打包在一起，以确保你的应用程序在任何环境中无缝运行。

镜像（Image）：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。
容器（Container）：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
仓库（Repository）：仓库可看成一个代码控制中心，用来保存镜像。

`Docker 是轻量级的沙盒，在其中运行的只是应用，虚拟机里面还有额外的系统。`

| docker images | 列出所有镜像 |
| ------------- | ------------ |
| docker ps     | 列出所有容器 |

## 13、hash索引知道多少

待补充。。。

## 14、简单说说 I/O流

IO流主要分为两大类，**字节流和字符流。字节流可以处理任何类型的数据，如图片，视频等，字符流只能处理字符类型的数据。**

待补充。。。

## 15、Java集合

为了保存数量不确定的数据，以及保存具有映射关系的数据（也被称为关联数组），Java 提供了集合类，也称为容器，位于 java.util 包下。

![img](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/20191124140050653.png)



Iterator接口和Iterable接口没有实现和继承的关系，这两个接口的关系在于Iterable可以返回一个Iterator实例对象。

> （1）forEach()方法：解决 for 循环依赖下标的问题，为高效遍历更多的数据结构提供了支持。
>
> （2）spliterator()方法：提供了一个可以并行遍历元素的迭代器，以适应现在cpu多核时代并行遍历的需求。
>
> （3）iterator()方法：可以返回一个Iterator实例对象。
>
> 	其中1和2是Java1.8之后新加的方法。

```java
	default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }
```

**关于forEach方法：**

>  只要想使用foreach循环遍历，就必须正确地实现Iterable接口

foreach循环==遍历对象的时候底层是使用迭代器进行迭代的==，其最终被**编译器转为了对Iterator.next()的调用**。

foreach循环==遍历数组的时候将其转型成为对每一个数组元素的循环引用==。



----------



# 项目

主要提供包含但不限于==文章发布、标签管理、分类管理、最新最热文章、评论和归档==等功能模块。



==jwt 可以生成 一个加密的token，做为用户登录的令牌，当用户登录成功之后，发放给客户端。== 当请求需要登录的资源或者接口的时候，将token携带，后端验证token是否合法。

首先在登陆之前在redis数据库中对数据进行查询，看是否存在该条数据，如果不存在的话，就去数据库查找，然后在查找到之后，在正常登录的时候将数据存储到redis中，当然这个存储信息的键值对也就是在redis查询的那个数据，然后==下次如果再次执行访问的时候，在redis中就有了此数据，进而提高了访问的效率。==细节：存储用户的登录信息，存储在redis中的时候使用的是hash数据结构，【hash数据结构其实就是，对应的键值对的值是一个字典类型。】此时就可以将用户携带的唯一标识作为值的键，将用户的其他某个信息作为该键的值存储起来。



**使用ThreadLocal替代Session** 保存用户信息。

**可以在同一线程中很方便的获取用户信息，不需要频繁的传递session对象。**

在登录业务代码中，当用户登录成功时，**生成一个登录凭证存储到redis中**，**将凭证中的字符串保存在cookie中返回给客户端**。
使用一个拦截器拦截请求，从cookie中获取凭证字符串与redis中的凭证进行匹配，获取用户信息，**将用户信息存储到ThreadLocal中，在本次请求中持有用户信息，即可在后续操作中使用用户信息。**

==UUID==

**线程池**更新文章阅读次数，==线程安全==

```java
@Configuration @EnableAsync  开启线程池ThreadPoolTaskExecutor
    创建一个service利用此注解将当前方法放入到线程池中使用 比如@Async("asyncServiceExecutor")
    加载文章详情和文章阅读数更新，这两个业务流程需要分开执行，互不影响。
```

==七牛云== 服务器加载图片。

==SpringCache 缓存文章内容==。

需要返回给前台的数据，不直接将pojo返回给前端，而创建一个==包VO（View Object）视图对象。==

采用 handler，同样是通过AOP实现的，①用来对@Controller注解的方法抛出异常。②集成HandlerInterceptor接口，==在用户登录之前进行preHandle==，主要是验证Token的信息。

从技术上来说，==spring security==中提供的权限管理功能主要有两种类型：

基于==过滤器==的权限管理(FilterSecurityInterceptor)。
基于==AOP==的权限管理(MethodSecurityInterceptor)。
基于过滤器的权限管理主要用来==拦截HTTP请求==，拦截下来之后，根据HTTP请求地址进行权限校验。
基于AOP的权限管理则主要用来处理==方法级别==的权限问题。当需要调用某一个方法时，通过AOP将操作拦截下来，然后判断用户是否具备相关的权限，如果具备，则允许方法调用；否则禁止方法调用。

```
在spring security中，当用户登录成功后，当前登录用户信息将保存在Authentication对象中，该对象中有一个getAuthorities方法，用来返回当前对象所具备的权限信息，也就是已经授予当前登录用户的权限，getAuthorities方法返回值是Collection<? extends GrantedAuthority>，即集合中存放的是GrantedAuthority的子类，当需要进行权限判断的时候，就会调用该方法获取用户的权限，进而做出判断。
```



![image-20220918081755033](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220918081755033.png)



mybatis-plus直接调用增删改查

Hikari 数据库连接池

apache shiro认证授权

layui

bootstrap

PageHelper 分页 物理分页和结果集分页 在插件的拦截方法内拦截待执行的sql，然后重写sql

- 权限（Permission） = 资源（Resource） + 操作（Privilege）
- 角色（Role） = 权限的集合（a set of low-level permissions）
- 用户（User） = 角色的集合（high-level roles）

echarts常用图表：折线图、柱状度、饼图、地图、树形结构

## 1、ThreadLocal

**使用ThreadLocal替代Session** 保存用户信息。

**可以在同一线程中很方便的获取用户信息，不需要频繁的传递session对象。**

在登录业务代码中，当用户登录成功时，**生成一个登录凭证存储到redis中**，**将凭证中的字符串保存在cookie中返回给客户端**。
使用一个拦截器拦截请求，从cookie中获取凭证字符串与redis中的凭证进行匹配，获取用户信息，**将用户信息存储到ThreadLocal中，在本次请求中持有用户信息，即可在后续操作中使用用户信息。**

-----

new 一个ThreadLocal 对象TL，这个对象是强应用指向ThreadLocal ，通过set方法把参数传到TL，通过ThreadLocalMap存储一个Entry对象【k,v键值对】。其中的**k 弱引用指向TL**，v强引用指向你开辟的内存。最后要调用==remove方法==，把这条记录从内存里删掉。

## 2、spring

让开发者专心于业务逻辑

面向接口，实现高内聚低耦合

约定大于配置

springboot自动装配

starter就相当于一个jar包，当运行的时候，对于扫描到的配置，会自动加载这个jar包，生成需要的对象

控制反转、容器、DI【@Autowired】

容器：存储对象，map结构存储，singleObject获取，每个对象的bean生命周期都是由容器管理的

bean通过反射实现，BeanFactory

![image-20220918081726448](https://raw.githubusercontent.com/SAH01/wordpress-img/master/imgs/image-20220918081726448.png)



# 其他

面试官您好，我是来自石家庄铁道大学软件工程系的学生杨传伟，很荣幸来参加此次面试。

我好奇心强，善于独立思考并解决问题。擅长Java语言，在日常编码中能够使用“分而治之”的思想解决实际问题。沟通表达能力
较好，曾担任学院主持人。有良好的规范代码编写习惯。长期保持写技术博客的习惯，在博客园累计发表500
+篇随笔，阅读量12W+。英语较好，可以独立完成英文文档等的阅读。

**缺点**：可能有时候会性急一点，这一点如果利用好可能对克服困难有一定的帮助，我在努力调整。

**加班**：不反感加班，但是更喜欢高效完成任务，避免不必要的加班。

**规划**：希望首先从技术从底层入手，未来的规划可能更倾向于去带一个完整的项目，事无巨细，不去完整的独立的领导一个项目，我觉的作为一个IT人是有遗憾的。

**录用之后**：首先去了解工作内容，熟悉工作流程。对公司使用的技术和现有的项目要尽快上手，把这些和自己的能力有机结合，发挥出最大效益。

**其他问题**：公司食宿、公司的产品以及项目组，公司对新人的培养方案，公司的晋升制度、薪资和福利补贴。