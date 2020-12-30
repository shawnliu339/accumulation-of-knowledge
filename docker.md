# docker
## Docker中的概念
1. Docker可以定义Image，用于产生Container。
2. Container为Image的实例，每一个Container都是独立的file system，app就运行在container上。
3. Docker可以通过Volume来共享不同container间的文件。
  * Volume实际存储在mountpoint所指的路径下。
    由于Docker command运行在一个小的虚拟机上，因此，要想查看mountpoint，需要先登录Docker command所在的虚拟机才可查看。
4. Docker可以通过network进行通讯

## Docker与VM的区别
Docker直接运行在host的内核上，因此，很轻量速度很快。
而VM需要运行在额外的hypervisor上，因此，会很重。

## Reference
Docker Architecture: https://docs.docker.com/get-started/overview/#docker-architecture