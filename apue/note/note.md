###chapter1:
* 调用fork创建一个新进程，新进程是调用进程的一个副本，调用进程为&quot;父进程&quot;，创建的进程为“子进程”，fork对父进程返回子进程的进程id,对子进程返回0。fork创建一个新进程，被调用一次，返回两次(父进程一次子进程一次)。

###chapter8:
* fork并没有真正的返回两次，它依然返回了一次，只是OS对fork进行的操作使得我们看起来它返回了两次而已。系统调用fork()创建新进程后，在进程表里新建了一个新的表项，生成的子进程志父进程完全是相对独立的进程，生成子进程后会给父进程返回一个下整数。子进程在之后被调度，会得到一个为0的返回&#20540;，这个过程是两个进程来自于同一程序的两次执行。
* fork之前的printf("xxxxx")没有加换行符会将输出内容重复输出一遍的原因：
因为这是**带缓存**的I/O，而缓存类型如果是连到终端设备，就是**行缓存**的，反之是全缓存。因为是行缓存，又因为这里没有了换行符，又又因为在fork之前一没有换行符二没有什么能让缓冲区满的语句，所以缓存中的数据在fork之前**不会输出到终端**，所以复制给子进程的缓存中就包含了before fork。
* 若子进程返回终止状态前父进程就已终止，那么子进程就会被init进程收养。（一个进程终止时，内核会检查所有的进程，判断它是不是该进程的子进程，如果是，则把那个进程的父进程id改为1->init进程的id）。
* zombie进程：父进程未对其进行善后处理的进程。
* 进程正常终止或异常终止时，内核会向其父进程发送**SIGCHLD**信号。