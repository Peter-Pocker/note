PTR (Page-Table Register)

页表寄存器，存放页表首地址及长度

TLB (Translation Look aside Buffer)

联想寄存器；高速缓冲寄存器

存放当前访问的一些页表项

![image-20220125104015715](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220125104015715.png)

### 系统接口

![image-20220125103853106](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220125103853106.png)

![image-20220125103931477](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220125103931477.png)

![image-20220125104751683](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220125104751683.png)

![image-20220125104911135](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220125104911135.png)

![image-20220125111049007](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220125111049007.png)

![image-20220125111108196](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220125111108196.png)

![image-20220125111132053](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220125111132053.png)

![image-20220125111205543](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\image-20220125111205543.png)

#### SLOB 分配器

对于小型的嵌入式系统来说，存在一个 slab 模拟层，名为 SLOB。这个 slab 的替代品在小型嵌入式 Linux 系统中具有优势，但是即使它保存了 512KB 内存，依然存在碎片和难于扩展的问题。在禁用 CONFIG_SLAB 时，内核会回到这个 SLOB 分配器中


#### mmap 过程

应用程序调用`mmap()`，磁盘上的数据会通过`DMA`被拷贝的内核缓冲区，接着操作系统会把这段内核缓冲区与应用程序共享，这样就不需要把内核缓冲区的内容往用户空间拷贝。应用程序再调用`write()`,操作系统直接将内核缓冲区的内容拷贝到`socket`缓冲区中，这一切都发生在内核态，最后，`socket`缓冲区再把数据发到网卡去。

### 内存共享

1、进程间需要共享的数据被放在一个叫做IPC共享内存区域的地方，所有需要访问该共享区域的进程都要把该共享区域映射到本进程的地址空间中去。
2、系统V共享内存通过shmget获得或创建一个IPC共享内存区域，并返回相应的标识符。
3、内核在保证shmget获得或创建一个共享内存区，初始化该共享内存区相应的shmid_kernel结构体
4、内核在特殊文件系统shm中，创建并打开一个同名文件，并在内存中建立起该文件的相应dentry及inode结构，新打开的文件不属于任何一个进程（任何进程都可以访问该共享内存区）

所有这一切都是系统调用`shmget`完成的。