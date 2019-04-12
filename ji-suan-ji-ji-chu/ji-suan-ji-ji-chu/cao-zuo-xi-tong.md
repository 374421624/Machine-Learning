# 操作系统

## [进程与线程](http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html)

对于有线程系统：进程是资源分配的独立单位；线程是资源调度的独立单位 

对于无线程系统：进程是资源调度、分配的独立单位

[进程的三种状态](http://blog.chinaunix.net/uid-23883288-id-3028968.html)：

* 就绪状态：进程已获得除处理机以外的所需资源，等待分配处理机资源
* 运行状态：占用处理机资源运行，处于此状态的进程数小于等于CPU数
* 阻塞状态： 进程等待某种条件，在条件满足之前无法执行 

## [进程间通信方式](https://blog.csdn.net/yufaw/article/details/7409596)

命名管道：一种半双工的通信方式，它允许无亲缘关系进程间的通信

* 优点：可以实现任意关系的进程间的通信
* 缺点：长期存于系统中，使用不当容易出错；缓冲区有限

无名管道：一种半双工的通信方式，只能在具有亲缘关系的进程间使用（父子进程）

* 优点：简单方便
* 缺点：局限于单向通信；只能创建在它的进程以及其有亲缘关系的进程之间；缓冲区有限

信号量（Semaphore）：一个计数器，可以用来控制多个线程对共享资源的访问

* 优点：可以同步进程
* 缺点：信号量有限

信号（Signal）：一种比较复杂的通信方式，用于通知接收进程某个事件已经发生

消息队列（Message Queue）：是消息的链表，存放在内核中并由消息队列标识符标识

* 优点：可以实现任意进程间的通信，并通过系统调用函数来实现消息发送和接收之间的同步，无需考虑同步问题，方便
* 缺点：信息的复制需要额外消耗 CPU 的时间，不适宜于信息量大或操作频繁的场合

共享内存（Shared Memory）：映射一段能被其他进程所访问的内存，这段共享内存由一个进程创建，但多个进程都可以访问

* 优点：无须复制，快捷，信息量大
* 缺点：
  1. 通信是通过将共享空间缓冲区直接附加到进程的虚拟地址空间中来实现的，因此进程间的读写操作的同步问题；利用内存缓冲区直接交换信息，内存的实体存在于计算机中，只能同一个计算机系统中的诸多进程共享，不方便网络通信

套接字（Socket）：可用于不同及其间的进程通信

* 优点：
  1. 传输数据为字节级，传输数据可自定义，数据量小效率高；传输数据时间短，性能高；适合于客户端和服务器端之间信息实时交互；可以加密,数据安全性强
* 缺点：需对传输的数据进行解析，转化成应用级的数据。

## [线程间通信方式](http://www.cnblogs.com/lebronjames/archive/2010/08/11/1797702.html)

锁机制：包括互斥锁/量（mutex）、读写锁（reader-writer lock）、自旋锁（spin lock）、条件变量（condition）

* 互斥锁/量（mutex）：提供了以排他方式防止数据结构被并发修改的方法。
* 读写锁（reader-writer lock）：允许多个线程同时读共享数据，而对写操作是互斥的。
* 自旋锁（spin lock）与互斥锁类似，都是为了保护共享资源。互斥锁是当资源被占用，申请者进入睡眠状态；而自旋锁则循环检测保持着是否已经释放锁。
* 条件变量（condition）：可以以原子的方式阻塞进程，直到某个特定条件为真为止。对条件的测试是在互斥锁的保护下进行的。条件变量始终与互斥锁一起使用。

信号量机制\(Semaphore\)

* 无名线程信号量
* 命名线程信号量

信号机制\(Signal\)：类似进程间的信号处理

屏障（barrier）：屏障允许每个线程等待，直到所有的合作线程都达到某一点，然后从该点继续执行。

线程间的通信目的主要是用于线程同步，所以线程没有像进程通信中的用于数据交换的通信机制

## 进程与线程对比

进程之间的私有和共享的资源：

* 私有：地址空间、堆、全局变量、栈、寄存器
* 共享：代码段，公共数据，进程目录，进程 ID

线程之间的私有和共享的资源：

* 私有：线程栈，寄存器，程序寄存器
* 共享：堆，地址空间，全局变量，静态变量

| 对比维度 | 多进程 | 多线程 | 总结 |
| :---: | :---: | :---: | :---: |
| 数据共享、同步 | 数据共享复杂，需要用 IPC；数据是分开的，同步简单 | 因为共享进程数据，数据共享简单，但也是因为这个原因导致同步复杂 | 各有优势 |
| 内存、CPU | 占用内存多，切换复杂，CPU 利用率低 | 占用内存少，切换简单，CPU 利用率高 | 线程占优 |
| 创建销毁、切换 | 创建销毁、切换复杂，速度慢 | 创建销毁、切换简单，速度很快 | 线程占优 |
| 编程、调试 | 编程简单，调试简单 | 编程复杂，调试复杂 | 进程占优 |
| 可靠性 | 进程间不会互相影响 | 一个线程挂掉将导致整个进程挂掉 | 进程占优 |
| 分布式 | 适应于多核、多机分布式；如果一台机器不够，扩展到多台机器比较简单 | 适应于多核分布式 | 进程占优 |

| 优劣 | 多进程 | 多线程 |
| :---: | :---: | :---: |
| 优点 | 编程、调试简单，可靠性较高 | 创建、销毁、切换速度快，内存、资源占用小 |
| 缺点 | 创建、销毁、切换速度慢，内存、资源占用大 | 编程、调试复杂，可靠性较差 |

所以：需要频繁创建销毁的优先用线程；需要进行大量计算的优先使用线程；强相关的处理用线程，弱相关的处理用进程；可能要扩展到多机分布的用进程，多核分布的用线程

## Linux 内核的同步

在现代操作系统里，同一时间可能有多个内核执行流在执行，因此内核其实象多进程多线程编程一样也需要一些同步机制来同步各执行单元对共享数据的访问。尤其是在多处理器系统上，更需要一些同步机制来同步不同处理器上的执行单元对共享的数据的访问。

### 原子操作

所谓原子操作，就是该操作绝不会在执行完毕前被任何其他任务或事件打断，也就说，它的最小的执行单位，不可能有比它更小的执行单位，因此这里的原子实际是使用了物理学里的物质微粒的概念。

原子操作需要硬件的支持，因此是架构相关的，其API和原子类型的定义都定义在内核源码树的include/asm/atomic.h文件中，它们都使用汇编语言实现，因为C语言并不能实现这样的操作。

原子操作主要用于实现资源计数，很多引用计数\(refcnt\)就是通过原子操作实现的。

### 信号量（semaphore）

Linux内核的信号量在概念和原理上与用户态的System V的IPC机制信号量是一样的，但是它绝不可能在内核之外使用，因此它与System V的IPC机制信号量毫不相干。

信号量在创建时需要设置一个初始值，表示同时可以有几个任务可以访问该信号量保护的共享资源，初始值为1就变成互斥锁（Mutex），即同时只能有一个任务可以访问信号量保护的共享资源。一个任务要想访问共享资源，首先必须得到信号量，获取信号量的操作将把信号量的值减1，若当前信号量的值为负数，表明无法获得信号量，该任务必须挂起在该信号量的等待队列等待该信号量可用；若当前信号量的值为非负数，表示可以获得信号量，因而可以立刻访问被该信号量保护的共享资源。当任务访问完被信号量保护的共享资源后，必须释放信号量，释放信号量通过把信号量的值加1实现，如果信号量的值为非正数，表明有任务等待当前信号量，因此它也唤醒所有等待该信号量的任务。

### 读写信号量（rw\_semaphore）

读写信号量对访问者进行了细分，或者为读者，或者为写者，读者在保持读写信号量期间只能对该读写信号量保护的共享资源进行读访问，如果一个任务除了需要读，可能还需要写，那么它必须被归类为写者，它在对共享资源访问之前必须先获得写者身份，写者在发现自己不需要写访问的情况下可以降级为读者。读写信号量同时拥有的读者数不受限制，也就说可以有任意多个读者同时拥有一个读写信号量。如果一个读写信号量当前没有被写者拥有并且也没有写者等待读者释放信号量，那么任何读者都可以成功获得该读写信号量；否则，读者必须被挂起直到写者释放该信号量。如果一个读写信号量当前没有被读者或写者拥有并且也没有写者等待该信号量，那么一个写者可以成功获得该读写信号量，否则写者将被挂起，直到没有任何访问者。因此，写者是排他性的，独占性的。

读写信号量有两种实现，一种是通用的，不依赖于硬件架构，因此，增加新的架构不需要重新实现它，但缺点是性能低，获得和释放读写信号量的开销大；另一种是架构相关的，因此性能高，获取和释放读写信号量的开销小，但增加新的架构需要重新实现。在内核配置时，可以通过选项去控制使用哪一种实现。

### 自旋锁（spinlock）

自旋锁与互斥锁有点类似，只是自旋锁不会引起调用者睡眠，如果自旋锁已经被别的执行单元保持，调用者就一直循环在那里看是否该自旋锁的保持者已经释放了锁，"自旋"一词就是因此而得名。由于自旋锁使用者一般保持锁时间非常短，因此选择自旋而不是睡眠是非常必要的，自旋锁的效率远高于互斥锁。

信号量和读写信号量适合于保持时间较长的情况，它们会导致调用者睡眠，因此只能在进程上下文使用（\_trylock的变种能够在中断上下文使用），而自旋锁适合于保持时间非常短的情况，它可以在任何上下文使用。如果被保护的共享资源只在进程上下文访问，使用信号量保护该共享资源非常合适，如果对共巷资源的访问时间非常短，自旋锁也可以。但是如果被保护的共享资源需要在中断上下文访问（包括底半部即中断处理句柄和顶半部即软中断），就必须使用自旋锁。

自旋锁保持期间是抢占失效的，而信号量和读写信号量保持期间是可以被抢占的。自旋锁只有在内核可抢占或SMP的情况下才真正需要，在单CPU且不可抢占的内核下，自旋锁的所有操作都是空操作。

跟互斥锁一样，一个执行单元要想访问被自旋锁保护的共享资源，必须先得到锁，在访问完共享资源后，必须释放锁。如果在获取自旋锁时，没有任何执行单元保持该锁，那么将立即得到锁；如果在获取自旋锁时锁已经有保持者，那么获取锁操作将自旋在那里，直到该自旋锁的保持者释放了锁。

无论是互斥锁，还是自旋锁，在任何时刻，最多只能有一个保持者，也就说，在任何时刻最多只能有一个执行单元获得锁。

### 大内核锁（BKL，Big Kernel Lock）

大内核锁本质上也是自旋锁，但是它又不同于自旋锁，自旋锁是不可以递归获得锁的，因为那样会导致死锁。但大内核锁可以递归获得锁。大内核锁用于保护整个内核，而自旋锁用于保护非常特定的某一共享资源。进程保持大内核锁时可以发生调度，具体实现是：在执行schedule时，schedule将检查进程是否拥有大内核锁，如果有，它将被释放，以致于其它的进程能够获得该锁，而当轮到该进程运行时，再让它重新获得大内核锁。注意在保持自旋锁期间是不运行发生调度的。

需要特别指出，整个内核只有一个大内核锁，其实不难理解，内核只有一个，而大内核锁是保护整个内核的，当然有且只有一个就足够了。

还需要特别指出的是，大内核锁是历史遗留，内核中用的非常少，一般保持该锁的时间较长，因此不提倡使用它。从2.6.11内核起，大内核锁可以通过配置内核使其变得可抢占（自旋锁是不可抢占的），这时它实质上是一个互斥锁，使用信号量实现。

### 读写锁（rwlock）

读写锁实际是一种特殊的自旋锁，它把对共享资源的访问者划分成读者和写者，读者只对共享资源进行读访问，写者则需要对共享资源进行写操作。这种锁相对于自旋锁而言，能提高并发性，因为在多处理器系统中，它允许同时有多个读者来访问共享资源，最大可能的读者数为实际的逻辑CPU数。写者是排他性的，一个读写锁同时只能有一个写者或多个读者（与CPU数相关），但不能同时既有读者又有写者。

在读写锁保持期间也是抢占失效的。

如果读写锁当前没有读者，也没有写者，那么写者可以立刻获得读写锁，否则它必须自旋在那里，直到没有任何写者或读者。如果读写锁没有写者，那么读者可以立即获得该读写锁，否则读者必须自旋在那里，直到写者释放该读写锁。

### 大读者锁（brlock-Big Reader Lock）

大读者锁是读写锁的高性能版，读者可以非常快地获得锁，但写者获得锁的开销比较大。大读者锁只存在于2.4内核中，在2.6中已经没有这种锁（提醒读者特别注意）。它们的使用与读写锁的使用类似，只是所有的大读者锁都是事先已经定义好的。这种锁适合于读多写少的情况，它在这种情况下远好于读写锁。

大读者锁的实现机制是：每一个大读者锁在所有CPU上都有一个本地读者写者锁，一个读者仅需要获得本地CPU的读者锁，而写者必须获得所有CPU上的锁。

### 读-拷贝修改\(RCU，Read-Copy Update\)

RCU\(Read-Copy Update\)，顾名思义就是读-拷贝修改，它是基于其原理命名的。对于被RCU保护的共享数据结构，读者不需要获得任何锁就可以访问它，但写者在访问它时首先拷贝一个副本，然后对副本进行修改，最后使用一个回调（callback）机制在适当的时机把指向原来数据的指针重新指向新的被修改的数据。这个时机就是所有引用该数据的CPU都退出对共享数据的操作。

RCU也是读写锁的高性能版本，但是它比大读者锁具有更好的扩展性和性能。 RCU既允许多个读者同时访问被保护的数据，又允许多个读者和多个写者同时访问被保护的数据（注意：是否可以有多个写者并行访问取决于写者之间使用的同步机制），读者没有任何同步开销，而写者的同步开销则取决于使用的写者间同步机制。但RCU不能替代读写锁，因为如果写比较多时，对读者的性能提高不能弥补写者导致的损失。

### 顺序锁（seqlock）

顺序锁也是对读写锁的一种优化，对于顺序锁，读者绝不会被写者阻塞，也就说，读者可以在写者对被顺序锁保护的共享资源进行写操作时仍然可以继续读，而不必等待写者完成写操作，写者也不需要等待所有读者完成读操作才去进行写操作。但是，写者与写者之间仍然是互斥的，即如果有写者在进行写操作，其他写者必须自旋在那里，直到写者释放了顺序锁。

这种锁有一个限制，它必须要求被保护的共享资源不含有指针，因为写者可能使得指针失效，但读者如果正要访问该指针，将导致OOPs。

如果读者在读操作期间，写者已经发生了写操作，那么，读者必须重新读取数据，以便确保得到的数据是完整的。

这种锁对于读写同时进行的概率比较小的情况，性能是非常好的，而且它允许读写同时进行，因而更大地提高了并发性。

## [死锁问题](https://www.cnblogs.com/balingybj/p/4782032.html)

在两个或者多个并发进程中，如果每个进程持有某种资源而又等待其它进程释放它或它们现在保持着的资源，在未改变这种状态之前都不能向前推进，称这一组进程产生了死锁。通俗的讲就是两个或多个进程无限期的阻塞、相互等待的一种状态。

死锁产生的四个条件（有一个条件不成立，则不会产生死锁）

* 互斥条件：一个资源一次只能被一个进程使用
* 请求与保持条件：一个进程因请求资源而阻塞时，对已获得资源保持不放
* 不剥夺条件：进程获得的资源，在未完全使用完之前，不能强行剥夺
* 循环等待条件：若干进程之间形成一种头尾相接的环形等待资源关系 

上面介绍了死锁发生时的四个必要条件，只要破坏这四个必要条件中的任意一个条件，死锁就不会发生。这就为我们解决死锁问题提供了可能。一般地，解决死锁的方法分为死锁的预防，避免，检测与恢复三种，当然还一种是忽略该问题，例如鸵鸟算法，该算法可以应用在极少发生死锁的情况下。

### 死锁预防

死锁的预防是保证系统不进入死锁状态的一种策略。它的基本思想是要求进程申请资源时遵循某种协议，从而打破产生死锁的四个必要条件中的一个或几个，保证系统不会进入死锁状态。

（1）破坏互斥条件。即允许进程同时访问某些资源。但是，有的资源是不允许被同时访问的，像打印机等等，这是由资源本身的属性所决定的。所以，这种办法并无实用价值。

（2）破坏不可剥夺条件。即允许进程强行从占有者那里夺取某些资源。就是说，当一个进程已占有了某些资源，它又申请新的资源，但不能立即被满足时，它必须释放所占有的全部资源，以后再重新申请。它所释放的资源可以分配给其它进程。这就相当于该进程占有的资源被隐蔽地强占了。这种预防死锁的方法实现起来困难，会降低系统性能。

（3）破坏请求与保持条件。可以实行资源预先分配策略。即进程在运行前一次性地向系统申请它所需要的全部资源。如果某个进程所需的全部资源得不到满足，则不分配任何资源，此进程暂不运行。只有当系统能够满足当前进程的全部资源需求时，才一次性地将所申请的资源全部分配给该进程。由于运行的进程已占有了它所需的全部资源，所以不会发生占有资源又申请资源的现象，因此不会发生死锁。但是，这种策略也有如下缺点：

* （1）在许多情况下，一个进程在执行之前不可能知道它所需要的全部资源。这是由于进程在执行时是动态的，不可预测的；

  （2）资源利用率低。无论所分资源何时用到，一个进程只有在占有所需的全部资源后才能执行。即使有些资源最后才被该进程用到一次，但该进程在生存期间却一直占有它们，造成长期占着不用的状况。这显然是一种极大的资源浪费；

  （3）降低了进程的并发性。因为资源有限，又加上存在浪费，能分配到所需全部资源的进程个数就必然少了。

（4）破坏循环等待条件，实行资源有序分配策略。采用这种策略，即把资源事先分类编号，按号分配，使进程在申请，占用资源时不会形成环路。所有进程对资源的请求必须严格按资源序号递增的顺序提出。进程占用了小号资源，才能申请大号资源，就不会产生环路，从而预防了死锁。这种策略与前面的策略相比，资源的利用率和系统吞吐量都有很大提高，但是也存在以下缺点：

* （1）限制了进程对资源的请求，同时给系统中所有资源合理编号也是件困难事，并增加了系统开销；

  （2）为了遵循按编号申请的次序，暂不使用的资源也需要提前申请，从而增加了进程对资源的占用时间。

### 死锁避免

上面我们讲到的死锁预防是排除死锁的静态策略，它使产生死锁的四个必要条件不能同时具备，从而对进程申请资源的活动加以限制，以保证死锁不会发生。下面我们介绍排除死锁的动态策略--死锁的避免，它不限制进程有关申请资源的命令，而是对进程所发出的每一个申请资源命令加以动态地检查，并根据检查结果决定是否进行资源分配。就是说，在资源分配过程中若预测有发生死锁的可能性，则加以避免。这种方法的关键是确定资源分配的安全性。

#### 安全序列

我们首先引入安全序列的定义：所谓系统是安全的，是指系统中的所有进程能够按照某一种次序分配资源，并且依次地运行完毕，这种进程序列 $$\{P_1,P_2,\dots,P_n\}$$ 就是安全序列。如果存在这样一个安全序列，则系统是安全的；如果系统不存在这样一个安全序列，则系统是不安全的。

安全序列 $$\{P_1,P_2,\dots,P_n\}$$ 是这样组成的：若对于每一个进程 $$P_i$$ ，它需要的附加资源可以被系统中当前可用资源加上所有进程 $$P_j$$ 当前占有资源之和所满足，则 $$\{P_1,P_2,\dots,P_n\}$$ 为一个安全序列，这时系统处于安全状态，不会进入死锁状态。

虽然存在安全序列时一定不会有死锁发生，但是系统进入不安全状态（四个死锁的必要条件同时发生）也未必会产生死锁。当然，产生死锁后，系统一定处于不安全状态。

#### 银行家算法

这是一个著名的避免死锁的算法，是由Dijstra首先提出来并加以解决的。

一个银行家如何将一定数目的资金安全地借给若干个客户，使这些客户既能借到钱完成要干的事，同时银行家又能收回全部资金而不至于破产，这就是银行家问题。这个问题同操作系统中资源分配问题十分相似：银行家就像一个操作系统，客户就像运行的进程，银行家的资金就是系统的资源。

一个银行家拥有一定数量的资金，有若干个客户要贷款。每个客户须在一开始就声明他所需贷款的总额。若该客户贷款总额不超过银行家的资金总数，银行家可以接收客户的要求。客户贷款是以每次一个资金单位（如1万RMB等）的方式进行的，客户在借满所需的全部单位款额之前可能会等待，但银行家须保证这种等待是有限的，可完成的。

银行家算法允许死锁必要条件中的互斥条件，占有且申请条件，不可抢占条件的存在，这样，它与预防死锁的几种方法相比较，限制条件少了，资源利用程度提高了。

这是该算法的优点。其缺点是：

* （1）算法要求客户数保持固定不变，这在多道程序系统中是难以做到的。

  （2）算法保证所有客户在有限的时间内得到满足，但实时客户要求快速响应，所以要考虑这个因素。

  （3）由于要寻找一个安全序列，实际上增加了系统的开销。

### 死锁检测与恢复

一般来说，由于操作系统有并发，共享以及随机性等特点，通过预防和避免的手段达到排除死锁的目的是很困难的。这需要较大的系统开销，而且不能充分利用资源。为此，一种简便的方法是系统为进程分配资源时，不采取任何限制性措施，但是提供了检测和解脱死锁的手段：能发现死锁并从死锁状态中恢复出来。因此，在实际的操作系统中往往采用死锁的检测与恢复方法来排除死锁。常利用资源分配图、进程等待图来协助这种检测。

死锁检测与恢复是指系统设有专门的机构，当死锁发生时，该机构能够检测到死锁发生的位置和原因，并能通过外力破坏死锁发生的必要条件，从而使得并发进程从死锁状态中恢复出来。一旦在死锁检测时发现了死锁，就要消除死锁，使系统从死锁状态中恢复过来。

（1）最简单，最常用的方法就是进行系统的重新启动，不过这种方法代价很大，它意味着在这之前所有的进程已经完成的计算工作都将付之东流，包括参与死锁的那些进程，以及未参与死锁的进程。

（2）撤消进程，剥夺资源。终止参与死锁的进程，收回它们占有的资源，从而解除死锁。这时又分两种情况：一次性撤消参与死锁的全部进程，剥夺全部资源；或者逐步撤消参与死锁的进程，逐步收回死锁进程占有的资源。一般来说，选择逐步撤消的进程时要按照一定的原则进行，目的是撤消那些代价最小的进程，比如按进程的优先级确定进程的代价；考虑进程运行时的代价和与此进程相关的外部作业的代价等因素。

此外，还有进程回退策略，即让参与死锁的进程回退到没有发生死锁前某一点处，并由此点处继续执行，以求再次执行时不再发生死锁。虽然这是个较理想的办法，但是操作起来系统开销极大，要有堆栈这样的机构记录进程的每一步变化，以便今后的回退，有时这是无法做到的。

## [分页和分段](https://blog.csdn.net/wangrunmin/article/details/7967293)

段是信息的逻辑单位，它是根据用户的需要划分的，因此段对用户是可见的 ；页是信息的物理单位，是为了管理主存的方便而划分的，对用户是透明的。

段的大小不固定，有它所完成的功能决定；页大大小固定，由系统决定

段向用户提供二维地址空间；页向用户提供的是一维地址空间

段是信息的逻辑单位，便于存储保护和信息的共享，页的保护和共享受到限制。

## 进程调度策略

### FCFS\(先来先服务\)

先来先服务\(FCFS\)调度算法是一种最简单的调度算法，该算法既可用于作业调度，也可用于进程调度。当在作业调度中采用该算法时，每次调度都是从后备作业队列中选择一个或多个最先进入该队列的作业，将它们调入内存，为它们分配资源、创建进程，然后放入就绪队列。在进程调度中采用FCFS算法时，则每次调度是从就绪队列中选择一个最先进入该队列的进程，为之分配处理机，使之投入运行。该进程一直运行到完成或发生某事件而阻塞后才放弃处理机。

### 短作业\(进程\)优先

短作业\(进程\)优先调度算法SJ\(P\)F，是指对短作业或短进程优先调度的算法。它们可以分别用于作业调度和进程调度。短作业优先\(SJF\)的调度算法是从后备队列中选择一个或若干个估计运行时间最短的作业，将它们调入内存运行。而短进程优先\(SPF\)调度算法则是从就绪队列中选出一个估计运行时间最短的进程，将处理机分配给它，使它立即执行并一直执行到完成，或发生某事件而被阻塞放弃处理机时再重新调度。

### 优先权调度

为了照顾紧迫型作业，使之在进入系统后便获得优先处理，引入了最高优先权优先\(FPF\)调度算法。此算法常被用于批处理系统中，作为作业调度算法，也作为多种操作系统中的进程调度算法，还可用于实时系统中。当把该算法用于作业调度时，系统将从后备队列中选择若干个优先权最高的作业装入内存。当用于进程调度时，该算法是把处理机分配给就绪队列中优先权最高的进程，这时，又可进一步把该算法分成如下两种。

#### 1\) 非抢占式优先权算法

在这种方式下，系统一旦把处理机分配给就绪队列中优先权最高的进程后，该进程便一直执行下去，直至完成；或因发生某事件使该进程放弃处理机时，系统方可再将处理机重新分配给另一优先权最高的进程。这种调度算法主要用于批处理系统中；也可用于某些对实时性要求不严的实时系统中。

#### 2\) 抢占式优先权调度算法

在这种方式下，系统同样是把处理机分配给优先权最高的进程，使之执行。但在其执行期间，只要又出现了另一个其优先权更高的进程，进程调度程序就立即停止当前进程\(原优先权最高的进程\)的执行，重新将处理机分配给新到的优先权最高的进程。因此，在采用这种调度算法时，是每当系统中出现一个新的就绪进程 $$i$$ 时，就将其优先权 $$P_i$$ 与正在执行的进程 $$j$$ 的优先权 $$P_j$$ 进行比较。如果 $$P_i\leq P_j$$ ，原进程 $$P_j$$ 便继续执行；但如果是 $$P_i> P_j$$ ，则立即停止 $$P_j$$ 的执行，做进程切换，使 $$i$$ 进程投入执行。显然，这种抢占式的优先权调度算法能更好地满足紧迫作业的要求，故而常用于要求比较严格的实时系统中，以及对性能要求较高的批处理和分时系统中。

### 高响应比优先

在批处理系统中，短作业优先算法是一种比较好的算法，其主要的不足之处是长作业的运行得不到保证。如果我们能为每个作业引入前面所述的动态优先权，并使作业的优先级随着等待时间的增加而以速率a 提高，则长作业在等待一定的时间后，必然有机会分配到处理机。该优先权的变化规律可描述为：

\(1\) 如果作业的等待时间相同，则要求服务的时间愈短，其优先权愈高，因而该算法有利于短作业。

\(2\) 当要求服务的时间相同时，作业的优先权决定于其等待时间，等待时间愈长，其优先权愈高，因而它实现的是先来先服务。

\(3\) 对于长作业，作业的优先级可以随等待时间的增加而提高，当其等待时间足够长时，其优先级便可升到很高，从而也可获得处理机。简言之，该算法既照顾了短作业，又考虑了作业到达的先后次序，不会使长作业长期得不到服务。因此，该算法实现了一种较好的折衷。当然，在利用该算法时，每要进行调度之前，都须先做响应比的计算，这会增加系统开销。

### 时间片轮转

在早期的时间片轮转法中，系统将所有的就绪进程按先来先服务的原则排成一个队列，每次调度时，把CPU 分配给队首进程，并令其执行一个时间片。时间片的大小从几ms 到几百ms。当执行的时间片用完时，由一个计时器发出时钟中断请求，调度程序便据此信号来停止该进程的执行，并将它送往就绪队列的末尾；然后，再把处理机分配给就绪队列中新的队首进程，同时也让它执行一个时间片。这样就可以保证就绪队列中的所有进程在一给定的时间内均能获得一时间片的处理机执行时间。换言之，系统能在给定的时间内响应所有用户的请求。

### 多级反馈

前面介绍的各种用作进程调度的算法都有一定的局限性。如短进程优先的调度算法，仅照顾了短进程而忽略了长进程，而且如果并未指明进程的长度，则短进程优先和基于进程长度的抢占式调度算法都将无法使用。而多级反馈队列调度算法则不必事先知道各种进程所需的执行时间，而且还可以满足各种类型进程的需要，因而它是目前被公认的一种较好的进程调度算法。在采用多级反馈队列调度算法的系统中，调度算法的实施过程如下所述。

\(1\) 应设置多个就绪队列，并为各个队列赋予不同的优先级。第一个队列的优先级最高，第二个队列次之，其余各队列的优先权逐个降低。该算法赋予各个队列中进程执行时间片的大小也各不相同，在优先权愈高的队列中，为每个进程所规定的执行时间片就愈小。例如，第二个队列的时间片要比第一个队列的时间片长一倍，……，第i+1个队列的时间片要比第i个队列的时间片长一倍。

\(2\) 当一个新进程进入内存后，首先将它放入第一队列的末尾，按FCFS原则排队等待调度。当轮到该进程执行时，如它能在该时间片内完成，便可准备撤离系统；如果它在一个时间片结束时尚未完成，调度程序便将该进程转入第二队列的末尾，再同样地按FCFS原则等待调度执行；如果它在第二队列中运行一个时间片后仍未完成，再依次将它放入第三队列，……，如此下去，当一个长作业\(进程\)从第一队列依次降到第_n_队列后，在第_n_ 队列便采取按时间片轮转的方式运行。

\(3\) 仅当第一队列空闲时，调度程序才调度第二队列中的进程运行；仅当第1～\(i-1\)队列均空时，才会调度第i队列中的进程运行。如果处理机正在第i队列中为某进程服务时，又有新进程进入优先权较高的队列\(第1～\(i-1\)中的任何一个队列\)，则此时新进程将抢占正在运行进程的处理机，即由调度程序把正在运行的进程放回到第i队列的末尾，把处理机分配给新到的高优先权进程。

## Source

{% embed url="https://github.com/huihut/interview" %}

{% embed url="https://zhuanlan.zhihu.com/p/23755202" %}

{% embed url="https://blog.csdn.net/luyafei\_89430/article/details/12971171" %}


