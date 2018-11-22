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
