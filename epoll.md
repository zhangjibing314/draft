# epoll
## linux I/O
linux 操作系统接收到的数据后需要经历两个阶段:  
    1. 将数据缓存到页缓存中;  
    2. 将页缓存中的数据拷贝到用户空间

* linux 的网络模式方案：
    * 阻塞 IO:程序阻塞，直到内核完成才返回；
    * 非阻塞 IO:内核处理完成与否都会直接返回相应结果，用户程序需要不停访问是否处理完成。
    * IO 多路复用：一个应用程序可以同时监听多个文件描述符。
    * 信号驱动 IO：
    * 异步 IO:用户程序不会被内核阻塞，当内核完成操作后可以通过signal 通知应用程序。

## I/O 多路复用
select, poll, epoll 都是 IO 多路复用机制，一个进程可以通过这种机制同时监听多个描述符。 select, poll, epoll 都是 同步 IO。

* select
```c
#include <sys/select.h>

int select(int nfds, fd_set *_Nullable restrict readfds,
        fd_set *_Nullable restrict writefds,
        fd_set *_Nullable restrict exceptfds,
        struct timeval *_Nullable restrict timeout);

void FD_CLR(int fd, fd_set *set);
int  FD_ISSET(int fd, fd_set *set);
void FD_SET(int fd, fd_set *set);
void FD_ZERO(fd_set *set);
```

* poll
```c
#include <poll.h>

int poll(struct pollfd *fds, nfds_t nfds, int timeout);

struct pollfd {
    int   fd;         /* file descriptor */
    short events;     /* requested events */
    short revents;    /* returned events */
};

```

* epoll
```c
#include <sys/epoll.h>

epoll_create(...)
epoll_ctl(...)
epoll_wait(...)

```

## I think
### question
* 同步、异步、阻塞、非阻塞之间的联系是什么？
    * 同步就是你让我做什么我就做什么，好没好的需要看我返回的结果。
        * 同步阻塞：等我做好的再给你结果。
        * 同步非阻塞：好没好的我先返回结果再说。
    * 异步就是你让我做的事情我正在做，做好了通知你。
        * 异步阻塞：(有这种情况吗？？？)
        * 异步非阻塞：我知道你要我做的了，做好了自然会通知你。
