https://zhuanlan.zhihu.com/p/448034877  
https://www.bilibili.com/video/BV1Lq4y1f7uW/

# 1、 单线程 epoll

```cpp
// 一栋楼用户寄快递
epoll_create(int size);  // 凤巢总盒子大小
epoll_ctl(); // 用户位置发生了改变
epoll_wait(fd, evt, evt_size, freq); 

while(1) {
    int event_num = epoll_wait();
    for (int i = 0; i < event_num; i++) {
        if (epollin) {
            int ret = 0;
#if ET
            // LT
            while(ret != -1) {  // ET，循环读
                ret = read(event[i].data.fd, buffer, length, 0);
            }
            push_to_workqueue(func)
            query_sql();

            loginfo();
#elif
            ret = read(event[i].data.fd, buffer, length, 0)
            send();
#endif
        }
    }
}
```

# 2、网络 io 多线程

* 1. 单线程处理accept，多线程处理 recv/send， 1s 内发生大量并发请求  --> 1:N

listen->accept->recv->send

* 2. 多线程处理accept，多线程处理 recv/send,  --> M:N

对于多线程只监听一个端口，只有一个listenfd，会有问题！！！

进程a里面：fd=5  
进程b里面：fd=5，不是一个东西，fd 是进程所属的东西


# 3. Reactor 模式也叫 Dispatcher 模式，我觉得这个名字更贴合该模式的含义，即 I/O 多路复用监听事件，收到事件后，根据事件类型分配（Dispatch）给某个进程 / 线程。

reactor 是为了把网络和业务分离开来，它是封装好了的epoll：

fd，recvbuffer，sendbuffer
