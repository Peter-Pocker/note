##### 线程创建

`int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine) (void *), void *arg);`

　　　　参数:
　　　　　　@thread 获取线程ID
　　　　　　@attr 设置线程属性 NULL：默认属性
　　　　　　@start_routine 线程执行的函数
　　　　　　@arg 给线程执行函数传递的参数

返回值:
　　成功返回0, 失败返回错误码

##### 线程退出

(1)线程函数返回 (return)
(2)pthread_exit
(3)pthread_cancel
(4)进程结束，这个进程中所有的线程都退出

`void pthread_exit(void *retval);`
功能:用来退出一个线程
参数:
@retval 返回一个地址值


`int pthread_join(pthread_t thread, void **retval);`
功能:等待一个线程退出，将退出线程未释放的资源释放掉
参数:
@thread 需要等待退出线程的ID
@retval 获得线程退出返回的值

##### 将线程标记分离

分离状态的线程在结束的时候，系统自动回收它的资源

`int pthread_detach(pthread_t thread);`
@thread 线程ID

 线程互斥锁

　　功能:对共享资源实现互斥访问,保证访问的完整性

　　如果使用互斥锁，每个线程在访问共享资源，都必须先获得互斥锁，然后在访问共享资源，如果无法获得互斥锁
　　，则表示有其他线程正在访问共享资源，此时没有获得锁的线程会阻塞，直到其他线程释放锁，能再次获得锁

1.定义互斥锁[全局，让每个线程都可以访问到]
pthread_mutex_t lock;

2.初始化互斥锁

//[线程创建之前]动态初始化,属性使用默认 NULL
int pthread_mutex_init(pthread_mutex_t *restrict mutex,const pthread_mutexattr_t *restrict attr);

//静态初始化定义初始化
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

3.获得互斥锁
int pthread_mutex_lock(pthread_mutex_t *mutex);//操作共享资源之前加锁
int pthread_mutex_trylock(pthread_mutex_t *mutex);

4.释放锁
int pthread_mutex_unlock(pthread_mutex_t *mutex);//操作共享资源结束的时候

5.销毁锁
int pthread_mutex_destroy(pthread_mutex_t *mutex);//不需要再次使用锁的时候

 线程间同步

　　同步:相互之间配合完成一件事情
　　互斥:保证访问共享资源的完整性(有你没我)

　　POSIX 线程中同步:使用信号量实现

　　信号量 : 表示一类资源，它的值表示资源的个数

对资源访问:
　　p操作(申请资源) [将资源的值 - 1]
　　....
　　V操作(释放资源) [将资源的值 + 1]

1.定义信号量
sem_t sem ;

2.初始化信号量
int sem_init(sem_t *sem, int pshared, unsigned int value);
参数:
　　@sem 信号量
　　@pshared 0:线程间使用
　　@value 初始化的信号量的值
返回值:
　　成功返回0,失败返回-1

3.P操作
　　int sem_wait(sem_t *sem);

4.V操作
　　int sem_post(sem_t *sem);

### 参考

[浅谈一下linux线程 - 白伟碧一些小心得 - 博客园 (cnblogs.com)](https://www.cnblogs.com/bwbfight/p/9304626.html)