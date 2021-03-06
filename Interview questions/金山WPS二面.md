# 金山wps服务端二面面经

作者：offer捡漏
链接：https://www.nowcoder.com/discuss/55642
来源：牛客网



协程和线程，比如协程底层的实现是怎样的 

 java和go的垃圾回收机制 

#  vector和list的扩容机制各自怎样 

**相同点：**
 1、ArrayList和Vector都是继承了相同的父类和实现了相同的接口
  （extends AbstractList implements List, Cloneable, Serializable, RandomAccess）
 2、底层都是数组（Object[]）实现的
 3、初始默认长度都为**10**。

**不同点：**
 1、同步性（*Synchronization*）：
  Vector中的public方法多数添加了synchronized关键字、以确保方法同步、也即是Vector线程安全、ArrayList线程不安全。
 2、扩容（*Resize*）：
  ArrayList以1.5倍的方式在扩容、Vector 当扩容容量增量大于0时、新数组长度为原数组长度+扩容容量增量、否则新数组长度为原数组长度的2倍。
 3、性能（*Performance*）：
  由于第一点的原因、在性能方便通常情况下ArrayList的性能更好、而Vector存在synchronized 的锁等待情况、需要等待释放锁这个过程、所以性能相对较差。
 4、快速失败（*fail-fast*）：
  什么是fail-fast:参见另一边博文： // TODO
  Vector 的 elements 方法返回的 Enumeration 不是 快速失败（fail-fast）的。而ArrayList是快速失败（fail-fast） 

# 锁机制是底层怎样实现的，为什么加锁释放锁本身可以是安全的 

#  [redis]()如何做到这种性能的(扯到i/o多路复用，然后进入select、poll和epoll的区别，我解释的时候说到mmap,面试官纠正我说select本身也可以用mmap优化这并不是本质区别，然后引导我到比如有100w个注册fd，如何发现就绪的fd问题上来)

(1)select==>时间复杂度O(n)

它仅仅知道了，有I/O事件发生了，却并不知道是哪那几个流（可能有一个，多个，甚至全部），我们只能无差别轮询所有流，找出能读出数据，或者写入数据的流，对他们进行操作。所以**select具有O(n)的无差别轮询复杂度**，同时处理的流越多，无差别轮询时间就越长。

(2)poll==>时间复杂度O(n)

poll本质上和select没有区别，它将用户传入的数组拷贝到内核空间，然后查询每个fd对应的设备状态， **但是它没有最大连接数的限制**，原因是它是基于链表来存储的.

(3)epoll==>时间复杂度O(1)

**epoll可以理解为event poll**，不同于忙轮询和无差别轮询，epoll会把哪个流发生了怎样的I/O事件通知我们。所以我们说epoll实际上是**事件驱动（每个事件关联上fd）**的，此时我们对这些流的操作都是有意义的。**（复杂度降低到了O(1)）**

select，poll，epoll都是IO多路复用的机制。I/O多路复用就通过一种机制，可以监视多个描述符，一旦某个描述符就绪（一般是读就绪或者写就绪），能够通知程序进行相应的读写操作。**但select，poll，epoll本质上都是同步I/O，因为他们都需要在读写事件就绪后自己负责进行读写，也就是说这个读写过程是阻塞的**，而异步I/O则无需自己负责进行读写，异步I/O的实现会负责把数据从内核拷贝到用户空间。 

epoll跟select都能提供多路I/O复用的解决方案。在现在的Linux内核里有都能够支持，其中epoll是Linux所特有，而select则应该是POSIX所规定，一般操作系统均有实现

#  问一下有没有看过[redis]()的[源码]()，问[redis]()除了缓存还有什么应用场景扯到负载均衡，[redis]()集群时[客户端]()分片怎么实现的，还引导我到Gossip通信(被虐)。

 [redis]()的数据结构问了下[redis]()的散列，hget hset命令等
 [redis]()怎么实现持久化的，开始扯snapshot和aof，持久化数据太多的时候[redis]()怎么优化它的io的。
 面试官:既然扯到缓存, 让你用java或者go来做一个缓存你怎么设计，然后说到用这种语言和用c来写的不同，扯到内存管理上不同带来的影响，面试官即刻说道那你怎么优化，后面没想出具体的方案，说了下我的思路即以哪个点做切入点，面试官好像还挺满意。 
 说一下rabbitmq和kafka的不同点，rabbit实现消息可靠性的原理是怎样的，rabiitmq在实习[项目]()中的运用
 给个白纸，画一下你设计的高并发网络模型，画了个reactor，然后问到accecpt用单线程吗，要解决的是百万级并发怎么优化？怎么平衡各线程池的核心线程数比例blabla.... 

  解释一下你所理解的http协议，扯到RPC的通信协议，然后面试官拿dubbo尬聊了一会，(T_T楼主对dubbo了解不深) 

  http和https的区别，blabla.... 面试官：你知道信道这个概念吗解释一下吧，我:.... 

  chrome可以控制台看到http报文的数据，为什么看到的不是加密后的呢，你能画个图解释一下整条https连接的图过程，就是数据报是怎么传过来的，你在chrome看到的数据是在哪个环节 

 面试官:我问问数据库方面吧，我:好。。。(紧绷) 
 char和varchar存储上的差异在哪，varchar最大长度限制多少，这两个类型在建索引的时候会有什么要注意的问题 
 innoDB和myISAM的区别，那你平时怎么去配置mysql的... 
 存储过程解释一下，它和事务的区别是什么 
 你有多少种办法复制一个关系表 

  了解一下别的，问了个剑指上那个统计二进制1个数的题目，说完问了怎么用bitmap来做



# Token认证和Session认证的区别，为什么Token认证能解决跨域问题

貌似看这两种登录验证没什么差别，但是其实差别很大，token的验证机制比session的验证机制更加灵活方便。

与session验证项目token的优点

1. 支持跨域访问: Cookie是不允许垮域访问的，这一点对Token机制是不存在的，前提是传输的用户认证信息通过HTTP头传输.
2. 无状态(也称：服务端可扩展行):Token机制在服务端不需要存储session信息，因为Token 自身包含了所有登录用户的信息，只需要在客户端的cookie或本地介质存储状态信息.
3. 更适用CDN: 可以通过内容分发网络请求你服务端的所有资料（如：javascript，HTML,图片等），而你的服务端只要提供API即可.
4. 去耦: 不需要绑定到一个特定的身份验证方案。Token可以在任何地方生成，只要在你的API被调用的时候，你可以进行Token生成调用即可.
5. 适用接口跨平台: 当你的客户端是一个原生平台（iOS, Android，Windows 8等）时，Cookie是不被支持的（你需要通过Cookie容器进行处理），这时采用Token认证机制就会简单得多。
6. CSRF:因为不再依赖于Cookie，所以你就不需要考虑对CSRF（跨站请求伪造）的防范。
7. 性能: 一次网络往返时间（通过数据库查询session信息）总比做一次HMACSHA256计算 的Token验证和解析要费时得多.
8. 不需要为登录页面做特殊处理: 如果你使用Protractor 做功能测试的时候，不再需要为登录页面做特殊处理.
9. 基于标准化:你的API可以采用标准化的 JSON Web Token (JWT). 这个标准已经存在多个后端库（.NET, Ruby, Java,Python, PHP）和多家公司的支持（如：Firebase,Google, Microsoft）

虽然token有这么多好处，但是有缺点就是对程序员的要求也会更高，session相对来说编写起来简单，而token的体系稍微复杂一些，最好可以使用现成的框架来搭建。
这里推荐一开始说的spingboot-security，博主现在也还在研究这个框架的使用，有啥问题可以留言一起讨论。

## MySQL中的几种索引，然后写了几个SQL问问会不会使用这个索引

普通索引：仅加速查询

唯一索引：加速查询 + 列值唯一（可以有null）

主键索引：加速查询 + 列值唯一（不可以有null）+ 表中只有一个

组合索引：多列值组成一个索引，专门用于组合搜索，其效率大于索引合并

全文索引：对文本的内容进行分词，进行搜索

# B+树了解吗

B+树包含两种节点，一种是非叶子节点(索引节点)，一种是叶子节点。

B+树与B树，最大的不同是**B+树的非叶子节点不保存数据**，只用于索引，所有数据都保存在叶子节点

非叶子节点最多有M − 1 M-1*M*−1个关键字，阶数M M*M*同时限制了叶子结点最多存储M − 1 M-1*M*−1个记录。

索引节点中的key都按照从小到大的顺序排列，对于内部结点中的一个key，左树中的所有key都**小于**它，右子树中的key都**大于等于**它。叶子结点中的记录也按照key的大小排列。

每个叶子节点都存有相邻叶子节点的指针，叶子节点本身依关键字的大小自小而大顺序链接**（范围查找特性）

# 如何实现单点登录（业务*2）

# 用图画一下Token授权的流程图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318203609498.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjIzNzc1Mg==,size_16,color_FFFFFF,t_70)

# 介绍一下分布式架构

# 讲解一下RPC协议

RPC是指远程过程调用，也就是说两台服务器A，B，一个应用部署在A服务器上，想要调用B服务器上应用提供的函数/方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。

- 首先，要解决通讯的问题，主要是通过在客户端和服务器之间建立TCP连接，远程过程调用的所有交换的数据都在这个连接里传输。连接可以是按需连接，调用结束后就断掉，也可以是长连接，多个远程过程调用共享同一个连接。
- 第二，要解决寻址的问题，也就是说，A服务器上的应用怎么告诉底层的RPC框架，如何连接到B服务器（如主机或IP地址）以及特定的端口，方法的名称名称是什么，这样才能完成调用。比如基于Web服务协议栈的RPC，就要提供一个endpoint URI，或者是从UDDI服务上查找。如果是RMI调用的话，还需要一个RMI Registry来注册服务的地址。
- 第三，当A服务器上的应用发起远程过程调用时，方法的参数需要通过底层的网络协议如TCP传递到B服务器，由于网络协议是基于二进制的，内存中的参数的值要序列化成二进制的形式，也就是序列化（Serialize）或编组（marshal），通过寻址和传输将序列化的二进制发送给B服务器。
- 第四，B服务器收到请求后，需要对参数进行反序列化（序列化的逆操作），恢复为内存中的表达方式，然后找到对应的方法（寻址的一部分）进行本地调用，然后得到返回值。
- 第五，返回值还要发送回服务器A上的应用，也要经过序列化的方式发送，服务器A接到后，再反序列化，恢复为内存中的表达方式，交给A服务器上的应用

# 知道zookeeper是怎么实现的吗

# 如何实现当前在线人数（统计半小时内的活跃人数）（业务*3）

# 用图跟我介绍一下IoC容器（面试官表示没了解过IoC）

# 平时有看什么书籍吗





自我介绍（个人擅长的领域、个人突出的地方） 

 （笔试里面的一道题）数据库里有10000000条用户信息，需要给每位用户发送信息（必须发送成功），要求节省内存（主键索引、分区技术、异步处理） 

 HTTP请求方法（GET、HEAD、POST、PUT、DELETE、CONNECT、OPTIONS、TRACE）

 ![img](https://images2018.cnblogs.com/blog/1418466/201808/1418466-20180810112625596-2103906128.png)

 GET与POST请求区别（根据笔试题的回答提问），POST请求运用，GET幂等的理解，GET请求URL显示，GET请求URL中为什么有“？”（例如：https://www.nowcoder.com/discuss/post?type=2），访问“http://www.docer.com/?from=nav _wps”后怎么显示，也就是空格的显示（出现“http://www.docer.com/?from=nav%20_wps”） 

#  说说RESTful架构 

#  说说字典树 

**（1）字典树（Trie树）**

　　Trie是个简单但实用的数据结构，通常用于实现字典查询。我们做即时响应用户输入的AJAX搜索框时，就是Trie开始。本质上，Trie是一颗存储多个字符串的树。相邻节点间的边代表一个字符，这样树的每条分支代表一则子串，而树的叶节点则代表完整的字符串。和普通树不同的地方是，相同的字符串前缀共享同一条分支。还是例子最清楚。给出一组单词，inn, int, at, age, adv, ant, 我们可以得到下面的Trie：

![img](https://pic002.cnblogs.com/images/2011/348136/2011111020144018.gif)

可以看出：

- 每条边对应一个字母。
- 每个节点对应一项前缀。叶节点对应最长前缀，即单词本身。
- 单词inn与单词int有共同的前缀“in”, 因此他们共享左边的一条分支，root->i->in。同理，ate, age, adv, 和ant共享前缀"a"，所以他们共享从根节点到节点"a"的边。
- 查询非常简单。比如要查找int，顺着路径i -> in -> int就找到了。
- 搭建Trie的基本算法也很简单，无非是逐一把每则单词的每个字母插入Trie。插入前先看前缀是否存在。如果存在，就共享，否则创建对应的节点和边。比如要插入单词add，就有下面几步：
  1. 考察前缀"a"，发现边a已经存在。于是顺着边a走到节点a。
  2. 考察剩下的字符串"dd"的前缀"d"，发现从节点a出发，已经有边d存在。于是顺着边d走到节点ad
  3. 考察最后一个字符"d"，这下从节点ad出发没有边d了，于是创建节点ad的子节点add，并把边ad->add标记为d。

#  平时怎么写数据库的模糊查询（由字典树扯到模糊查询，前缀查询，例如“abc%”，还是索引策略的问题） 

#  面向对象编程的理解 

#  平时怎么写面向对象编程（聊了面向接口编程） 

#  socket编程，怎么实现信息传输，还是多人聊天室的问题（[项目]()的坑） 

#  MySQL事务隔离等级，MySQL默认的事务隔离等级 

| 事务隔离级别                 | 脏读 | 不可重复读 | 幻读 |
| ---------------------------- | ---- | ---------- | ---- |
| 读未提交（read-uncommitted） | 是   | 是         | 是   |
| 不可重复读（read-committed） | 否   | 是         | 是   |
| 可重复读（repeatable-read）  | 否   | 否         | 是   |
| 串行化（serializable）       | 否   | 否         | 否   |

mysql默认的事务隔离级别为repeatable-read

#  MySQL事务特性

1、原子性（Atomicity）：事务开始后所有操作，要么全部做完，要么全部不做，不可能停滞在中间环节。事务执行过程中出错，会回滚到事务开始前的状态，所有的操作就像没有发生一样。也就是说事务是一个不可分割的整体，就像化学中学过的原子，是物质构成的基本单位。

2、一致性（Consistency）：事务开始前和结束后，数据库的完整性约束没有被破坏 。比如A向B转账，不可能A扣了钱，B却没收到。

3、隔离性（Isolation）：同一时间，只允许一个事务请求同一数据，不同的事务之间彼此没有任何干扰。比如A正在从一张银行卡中取钱，在A取钱的过程结束前，B不能向这张卡转账。

4、持久性（Durability）：事务完成后，事务对数据库的所有更新将被保存到数据库，不能回滚。

# innoDB与myisam区别

# 聚簇索引与非聚簇索引

聚簇索引：将数据存储与索引放到了一块，找到索引也就找到了数据

非聚簇索引：将数据存储于索引分开结构，索引结构的叶子节点指向了数据的对应行，myisam通过key_buffer把索引先缓存到内存中，当需要访问数据时（通过索引访问数据），在内存中直接搜索索引，然后通过索引找到磁盘相应数据，这也就是为什么索引不在key buffer命中时，速度慢的原因

# 事务四大特征

# 事务隔离级别，分别可以解决什么问题





1、项目相关的技术和内容
直接根据经历回答就行，问的比较细
2、java的jvm内存管理，GC，类加载模型问题
根据平时学的回答就行，不会问的特别细
3、java的hashmap原理，什么时候链表转红黑树
照着学过的回答就行
4、如何处理跨域问题
照着学过的回答就行
5、数据库mysql原子性原理，数据库三范式
照着学过的回答就行

