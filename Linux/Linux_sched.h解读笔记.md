E:\share\linux-5.11\kernel\sched\sched.h

```c
struct rq; // 进程队列
```

### struct task_struct 解读

TCB的定义在 `linux-5.11\include\linux\sched.h` 中

整个定义有 738 行



调度模块的主函数 

```c
// F:\linux-5.4.32\kernel\sched\core.c
static void __sched notrace __schedule(bool preempt)
```

```c
// F:\linux-5.4.32\kernel\sched\core.c
asmlinkage __visible void __sched schedule(void)
{
	struct task_struct *tsk = current;

	sched_submit_work(tsk);
	do {
		preempt_disable(); // 关闭抢占
		__schedule(false);
		sched_preempt_enable_no_resched();
	} while (need_resched());
	sched_update_worker(tsk);
}
EXPORT_SYMBOL(schedule);
```

