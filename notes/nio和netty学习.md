NIO和Netty学习

curl -I  http://xxx  可以打印出来 报文头

wrk 命令 可以压请求（指定好请求的连接）

wrk  -c 10(并发) -t 10（线程） -d 60（时间（秒））

window系统有专门的的压测的接口的工具

输出 content length 是避免 连接被重置的关键

因为 http的报文默认 没有content length  他不知道报文题的长度是多少，就会发生这个问题（没记太好，还要自己百度）

CUP密集计算/业务处理

IO操作与等待/网络，磁盘，数据库（这里意味着要等待对方回应）![image-20210117195557882](D:\my_study\my_study\picture\image-20210117195557882.png)

![image-20210117200156482](D:\my_study\my_study\picture\image-20210117200156482.png)

同步异步：是通信模式

阻塞，非阻塞是线程处理方式。

异步模型是一定不会阻塞线程的

![image-20210117201600979](D:\my_study\my_study\picture\image-20210117201600979.png)

![image-20210117201739941](D:\my_study\my_study\picture\image-20210117201739941.png)

fd:文件描述符

优化好的话，netty可以单机处理100万的请求

SEDA模型	

异步IO相当于叫外卖， 同步非阻塞IO相当于叫美团买菜，然后自己炒 或者 同步非阻塞 需要在内核准备好后，自己去复制数据，异步非阻塞是 内核帮你准备好数据后，还给复制好。

![image-20210117205346290](D:\my_study\my_study\picture\image-20210117205346290.png)

IO模型与语言无关

关于java的网络,有个比喻形象的总结
例子：有一个养鸡的农场，里面养着来自各个农户（Thread）的鸡（Socket），每家农户都在农场中建立了自己的鸡舍（SocketChannel）

1、BIO：Block IO，每个农户盯着自己的鸡舍，一旦有鸡下蛋，就去做捡蛋处理；

2、NIO：No-BlockIO-单Selector，农户们花钱请了一个饲养员（Selector），并告诉饲养员（register）如果哪家的鸡有任何情况  
（下蛋）均要向这家农户报告（selectkeys）;

3、NIO：No-BlockIO-多Selector，当农场中的鸡舍(Selector)逐渐增多时，一个饲养员巡视（轮询）一次所需时间就会不断地加长，这  
样农户知道自己家的鸡有下蛋的情况就会发生较大的延迟。怎么解决呢？没错，多请几个饲养员（多Selector），每个饲养员分配管理鸡舍，这  
样就可以减轻一个饲养员的工作量，同时农户们可以更快的知晓自己家的鸡是否下蛋了；

4、Epoll模式：如果采用Epoll方式，农场问题应该如何改进呢？其实就是饲养员不需要再巡视鸡舍，而是听到哪间鸡舍(Selector)的鸡
打鸣了（活跃连接），就知道哪家农户的鸡下蛋了；

5、AIO：AsynchronousI/O,鸡下蛋后，以前的NIO方式要求饲养员通知农户去取蛋，AIO模式出现以后，事情变得更加简单了，取蛋工作由
饲养员自己负责，然后取完后，直接通知农户来拿即可，而不需要农户自己到鸡舍去取蛋。



mina和netty是一个作者

java 是直接用操作系统的线程，：裸线程，很裸很重（就像没人直接用jdbc一样，jdbc就是很裸很重）

管道+过滤的模式

HTTP出现乱码，一定是有编码不一致（有两处编码在http的head里，一处使我们返回报文体的编码）

http的Range头  单点续传

（短信网关实现？）Vertx Spring 5 webflux 都是基于netty是实现的

CDMA联通。。。

现在各种和web和网络相关的库，服务器，底层基本上是基于netty的

websocket实际上是借用了**Http底层的** tcp协议

