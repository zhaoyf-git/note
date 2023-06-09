### Cache
cache种类：一级cache(L1)、二级cache(L2)、三级cache(L3)、TLBcache
cache地址映射类型：
1.全关联型cache（cache利用率最高，但使用条件很高，目前仅有英特尔TLBcache使用）
2.直接关联型cache（命中率最低，但匹配速度最快，最好实现）
3.组关联型cache(最常用)：前两者折中方式。在这种方式下内存会被分组，组外采用直接关联型，组内使用全关联型
cache的写策略：内存被夹在到cache后，被写会内存的时刻
直写：cache只要修改，同步写会内存（总线使用率高，效率低）
回写：cache line 添加一个dirty标志，当cache line被改写dirty被置1，下次cache line在被修改发现dirty为1则现将数据写回内存，在修改cache line。（多核存在缓存一致性问题）
cache一致性问题：
cache对齐；
cache一致性：解决方案是一致性协议MESI
MESI协议状态：
修改态（Modified）：
独占态（Exclusive）：
共享态（Shared）：
失效态（Invalid）：
dpdk解决cache一致性问题：
1.数据结构的定义：采用cache line对齐，每个核定义一份结构避免共享竞争
2.网卡多队列，为每个核使用单独的收发队列；

cache预取：程序运行存在时间局部性和空间局部性，cache预取也分为硬件预取和软件预取

### 内存屏障
内存屏障”（memory barrier）是一种陌生，甚至有些诡异的技术。实际上，他常被用在操作系统内核中，用于实现同步机制、驱动程序等。利用它，能实现高效的无锁数据结构，提高多线程程序的性能表现。

内存乱序访问：
编译时，编译器优化进行指令重排而导致内存乱序访问；
运行时，多CPU间交互引入内存乱序访问。
**内存屏障包括两类：编译器屏障和CPU内存屏障**