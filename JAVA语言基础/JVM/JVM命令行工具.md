### JVM命令行工具

> jps,jsat,jinfo,jmap,jhat,jstack

#### 一、jps

- 约等于 ps -ef|grep java
- 实战使用
  - jps -l 输出主类的全名，如果进程执行的是Jar包则输出Jar路径
  - jps -v 输出虚拟机进程启动时JVM参数

#### 二、jstat

- 可以打印状态信息，比如**gc信息**

![](E:\读书笔记\readingNotes\JAVA语言基础\JVM\img\a.png)



```powershell
 jstat -gc 2764 250 20   //2764表示进程id ，250表示250毫秒打印一次 ，20表示一共打印20次
 S0U：from幸存区的使用大小
 S1U：to幸存区的使用大小
 EC：伊甸园区的大小
 EU：伊甸园区的使用大小
 OC：老年代大小
 OU：老年代使用大小
 MC：方法区大小
 MU：方法区使用大小
 CCSC:压缩类空间大小
 CCSU:压缩类空间使用大小
 YGC：年轻代垃圾回收次数
 YGCT：年轻代垃圾回收消耗时间
 FGC：老年代垃圾回收次数
 FGCT：老年代垃圾回收消耗时间
 GCT：垃圾回收消耗总时间
```

#### 四、jinfo

打印一个进程的各种参数信息，包括一些**系统默认参数**

#### 五、jmap

打印堆快照信息

- -XX: +HeapDumpOnOutOfMemoryError 参数，可以让虚拟机在 OOM 异常出现之后自动生成 dump 文件

- jmap常用命令

  - -heap

    输出堆内存使用信息

  - -dump

    - 生成 Java 堆转储快照。格式为：-dump:  format=b, file= <filename>

      ```powershell
      windows： jmap -dump:format=b,file=d:\a.bin 1234
      mac:      jmap -dump:format=b,file=/Users/daniel/deskTop
      ```

      

  - -histo   more分页去查看

    - 显示堆中具体的**对象占用内存信息**，包括类、实例数量、合计容量
    - 可用来查看哪个类产生了大对象

    B ：byte

    C :  char

    I ：Int

#### 六、jhat

用来分析快照文件（可配合jmap使用）

- jhat 的分析功能相对来说比较简陋

#### 七、jstack

用于生成线程快照

jstack -l 3500

- 显示线程状态，运行在哪行代码等

- 可以发现死锁