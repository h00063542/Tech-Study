# 第一章 策略、方法和方法论
## 两种分析方法
1. 自顶向下：从应用性能监控－》分析找出性能问题
2. 自底向上：从最底层CPU、内存、磁盘等监控－》分析找出性能问题

# 第二章 操作系统性能监控
## 定义：性能监控、性能分析、性能调优
## CPU使用率
* 概念：用户态使用率、系统态使用率、每时钟指令数、每指令时钟周期
* 监控CPU使用率：
** GNOME（Linux 图形化）、vmstat（Linux us／sy／id）
** top命令

## CPU调度程序运行队列：分辨系统是否满负荷
* 如果很长时间内，运行队列长度一直都超过虚拟处理器个数的1倍就需要关注
* 监控工具：vmstat 第一列r

## 内存使用率
* 监控：Linux vmstat si和so 分别代表内存页面换入和换出
* 锁竞争:
** 让步式上下文切换：执行线程主动释放CPU，切换耗时80000个时钟周期，大于5%的可用时钟周期，说明可能遇到锁竞争
** 抢占式上下文切换：线程分配的时间片用尽，被迫释放CPU或者被其他优先级高的线程抢占
** 监控：pidstat -w cswch/s代表切换数
** 优化思路：减少线程跨处理器切换，可以将应用分配自定的处理器组  linux taskset命令

## 网络IO使用率
* 监控：netstat、nicstat（可以查看网络使用率）

## 磁盘IO使用率
* 监控：iostat

## 其他工具
* sar：收集性能统计数据

# 第三章 JVM概览
## HotSpot  基本架构

* VM运行时
** 32位VM／64位VM 压缩指针参数（-XX:+UseCompressedOops）
** 职责：命令行解析、VM生命周期管理、类加载、字节码解释、异常处理、同步、线程管理、Java本地接口、VM致命错误处理、C++堆管理
*** 命令行选项：标准选型、非标准选项（以－X为前缀）、非稳定选项（以－XX为前缀），选项名钱＋或－代表true或false
*** VM生命周期：java
**** 解析命令行选项：比如 -client或-server 他们决定加载那个JIT
**** 设置堆的大小和JIT：如果没有明确设置堆和JIT，启动器自动优化设置
**** 设置环境变了LD_LIBRARY_PATH和CLASSPATH
**** 如果有－jar选项，找jar中manifest中的Main-Class 否则从命令行读取Main-Class
**** 使用标准JNI方法JNI_CreateJavaVM在新创建的线程中创建HotSpot VM
**** 创建好HotSpot VM，加载java Main-Class，调用main方法
**** 执行完main，调用DetachCurrentThread、DestoryJavaVM

* JIT编译器
* 内存管理器

