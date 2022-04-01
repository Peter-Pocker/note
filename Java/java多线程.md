### Java 线程生命周期

![image-20220116110136972](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220116110136972.png)

#### 线程状态

定义在`java.lang.Thread`中的枚举`State`描述了Java中线程的六种状态

1. NEW

   还未开始执行的线程

2. RUNNABLE

   在 JVM 中，就绪或正在执行

3. BLOCKED

4. WAITING

5. TIMED_WAITING

6. TERMINATED

   线程执行完成

##### BLOCKED, WAITING, TIMED_WAITING 的区别

- [x] 还没理解

```Java
/* Defined in java.lang.Thread, in the enum State */
        /**
         * Thread state for a thread blocked waiting for a monitor lock.
         * A thread in the blocked state is waiting for a monitor lock
         * to enter a synchronized block/method or
         * reenter a synchronized block/method after calling
         * {@link Object#wait() Object.wait}.
         */
	// 也就是说BLOCKED的意思是：1.因为等待进入synchronized代码段而阻塞；2.调用wait方法之后而进入synchronized代码段
        BLOCKED,

        /**
         * Thread state for a waiting thread.
         * A thread is in the waiting state due to calling one of the
         * following methods:
         * <ul>
         *   <li>{@link Object#wait() Object.wait} with no timeout</li>
         *   <li>{@link #join() Thread.join} with no timeout</li>
         *   <li>{@link LockSupport#park() LockSupport.park}</li>
         * </ul>
         *
         * <p>A thread in the waiting state is waiting for another thread to
         * perform a particular action.
         *
         * For example, a thread that has called {@code Object.wait()}
         * on an object is waiting for another thread to call
         * {@code Object.notify()} or {@code Object.notifyAll()} on
         * that object. A thread that has called {@code Thread.join()}
         * is waiting for a specified thread to terminate.
         */
	// 1.调用了无超时的wait方法；2。调用了无超时的join方法；3.调用了LockSupport.park
	// 处于WAITING状态的线程等待其他线程执行某种特定方法来将之唤醒
        WAITING,

        /**
         * Thread state for a waiting thread with a specified waiting time.
         * A thread is in the timed waiting state due to calling one of
         * the following methods with a specified positive waiting time:
         * <ul>
         *   <li>{@link #sleep Thread.sleep}</li>
         *   <li>{@link Object#wait(long) Object.wait} with timeout</li>
         *   <li>{@link #join(long) Thread.join} with timeout</li>
         *   <li>{@link LockSupport#parkNanos LockSupport.parkNanos}</li>
         *   <li>{@link LockSupport#parkUntil LockSupport.parkUntil}</li>
         * </ul>
         */
	// 有限期阻塞的线程
        TIMED_WAITING,
```

### 创建线程的四种方法

#### 一、实现 Runnable 接口

需要实现 `run` 方法，然后将实现`Runnable`接口的类的对象作为参数来构造一个`Thread`对象，再调用`Thread`对象的`start`方法使线程变为新建状态

```java
/* run方法在Runnable接口中的声明 */
public void run();
```

#### 二、继承 Thread 类

需要覆盖`run`方法（因为Thread在实现时实现了run方法），然后调用一个Thread对象的`start`方法来启动线程

> 本质上讲，继承Thread和实现Runnable两种方法是一样的

#### 三、通过 Callable 和 Future 创建线程

1. 定义类实现`Callable`接口（重载`call`方法）并实例化一个对象
2. 将该对象作为参数构造一个`FutureTask`对象，再将该`FutureTask`对象作为参数构造一个`Thread`对象
3. 调用Thread对象的`start`方法新建线程

Callable接口中定义了唯一的一个方法call()，而FutureTask类实际上实现了一个名为RunnableFuture的接口，而RunnableFuture接口实际上继承了Runnable接口

> 问题：为什么Callable接口中声明的call()方法不带abstract关键字，而Runnable接口中的run()方法带abstract关键字呢
>
> 答：接口中的方法都是隐式声明为abstract的

> 总而言之，创建线程离不开Thread类。
>
> 而在实例化Thread的时候，要向Thread的构造函数传递至少一个参数（源代码中形式参数名叫 target ），这个参数在Thread类的定义中是一个Runnable类型的变量，也就是说，要向Thread构造函数传入一个实现了Runnable接口的类的对象

#### 四、通过线程池启动多线程

通过Executor 的工具类可以创建三种类型的普通线程池

##### （1）FixThreadPool 固定大小的线程池

 适用于为了满足资源管理需求而需要限制当前线程数量的场合。使用于负载比较重的服务器。

##### （2）SingleThreadPoolExecutor 单线程池

需要保证顺序执行各个任务的场景 

##### （3）CashedThreadPool 缓存线程池

当提交任务速度高于线程池中任务处理速度时，缓存线程池会不断的创建线程。适用于提交短期的异步小程序，以及负载较轻的服务器

### 多线程中的关键字及其用法

###### synchronized 关键字

被修饰的代码段或方法同时只能被一个线程执行

###### synchronized (object) 用法

可以理解为**对象锁**

将该修饰符修饰对象方法中的一个代码块时，可以限定：对于对象object的方法的调用时，被synchronized(object) 修饰的代码块同时只能被一个线程执行，并且，只有当这个线程执行完所有有synchronized修饰的方法代码块之后，其他线程才能执行该段。

refer to [(12条消息) 对synchronized(this)的一些理解_Java高知-CSDN博客](https://blog.csdn.net/tjcyjd/article/details/21601133)

###### volatile 关键字

被修饰的变量被更改后马上写回内存

当一个共享变量被volatile修饰时，它会保证修改的值会立即被更新到主存，当有其他线程需要读取时，它会去内存中读取新值，从而保证数据的可见性。普通的共享变量 不能保证可见性，因为普通共享变量被修改之后，会先写入高速缓存，而什么时候被写入主存是不确定的

refer to [Java并发编程：volatile关键字解析 - Matrix海子 - 博客园 (cnblogs.com)](https://www.cnblogs.com/dolphin0520/p/3920373.html)

#### Thread 方法



### Java多线程的锁

在Java中，对锁的操作主要由`Lock`接口定义，位置是java.base\java\util\concurrent\locks\ock.class

#### 对锁的操作

```java
/* Lock 接口中定义了如下方法 */
// 尝试获取锁，得不到就睡眠
void lock();
// 一直尝试获取，直到获得或被中断
void lockInterruptibly() throws InterruptedException;
// 尝试获取锁，并立即返回是否能获得锁
boolean tryLock();
// 在不被中断的情况下，在一定时间内等待获取锁
boolean tryLock(long time, TimeUnit unit) throws InterruptedException;
// 释放锁
void unlock();
// 
Condition newCondition();
```

#### 锁类型

##### 可重入锁 ReentrantLock

##### 自旋锁

线程得不到该锁就“忙等待”

偏向锁/轻量级锁/重量级锁

##### 分段锁

##### 乐观锁/悲观锁

##### 互斥锁/[读写锁](https://so.csdn.net/so/search?from=pc_blog_highlight&q=读写锁)

##### 独享锁/共享锁

##### 公平锁/非公平锁

是否按照申请的先后顺序分配锁

##### 什么是CAS

CAS,compare and swap
