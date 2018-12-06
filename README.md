# note
jvm分析工具：可以使用virtualVM监控本地和远程。
内存dump可以使用如下两种方式：
 1. JVM启动时增加两个参数，出现 OOME 时生成堆 dump: 
-XX:+HeapDumpOnOutOfMemoryError
生成堆文件地址：
-XX:HeapDumpPath=/home/test/jvmlogs/ 

 2. 发现程序异常前通过执行指令，直接生成当前JVM的dmp文件，15434是指JVM的进程号
 jmap -dump:format=b,file=serviceDump.dat    15434 
 由于第一种方式是一种事后方式，需要等待当前JVM出现问题后才能生成dmp文件，实时性不高，第二种方式在执行时，JVM是暂停服务的，所以对线上的运行会产生影响。所以建议第一种方式。
 如果dump文件过大，需要使用更专业的dmp文件分析工具【Memory Analyzer (MAT)】
 
 线程dump
 jstack -l pid > jstack.log
 
 还可以使用spring-boot-actuator与spring-boot-admin来监控指定端点并进行可视化展现


 ZAB协议并不是Paxos算法的一个典型实现，在讲解ZAB和Paxos之间的区别之前，我们首先来看下两者的联系。

两者都存在一个类似于Leader进程的角色，由其负责协调多个Follow进程的运行。
Leader进程都会等待超过半数的Follower做出正确的反馈后，才会将一个提案进行提交。
在ZAB协议中，每个Proposal中都包含了一个epoch值，用来代表当前Leader周期，在Paxos算法中，同样存在这样一个标识，只是名字变成了Ballot。
        在Paxos算法中，一个新选举产生的主进程会进行两个阶段的工作。第一阶段被称为读阶段，在这个阶段中，这个新的主进程会通过和所有其他进程进行通信的方式来收集上一个主进程的提案，并将他们提交。第二阶段被称为写阶段，在这个阶段，当前主进程开始提出他自己的提案。在Paxos算法设计的基础上，ZAB协议额外添加了一个同步阶段。在同步阶段之前，ZAB协议也存在一个和Paxos算法中的读阶段非常类似的过程，称为发现（Discovery）阶段。在同步阶段中，新的Leader会确保存在过半的Follower已经提交了之前Leader周期中的所有事务Proposal。这一同步阶段的引入，能够有效地保证Leader在新的周期中提出事务Proposal之前，所有的进程都已经完成了对之前所有事务Proposal的提交。一旦完成同步阶段后，那么ZAB就会执行和Paxos算法类似的写阶段。

        总的来讲，ZAB协议和Paxos算法的本质区别在于，两者的设计目标不太一样。ZAB协议主要用于构建一个高可用的分布式数据主备系统，例如ZooKeeper，而Paxos算法则是用于构建一个分布式的一致性状态机系统。
