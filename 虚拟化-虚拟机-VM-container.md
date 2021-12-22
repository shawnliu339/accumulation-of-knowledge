# 虚拟化
将一台物理机，虚拟化为多台虚拟机，可以充分利用资源。
以下的比喻非常的容易理解。

上世纪90年代，一台服务器只能安装一个操作系统，而一种服务基本只能运行在特定的操作系统上。  
因此，出现了一家公司的邮件系统，网络系统，应用系统都需要运行在不同服务器上的情况。  
但是，一台服务器的资源往往是盈余的，就出先来资源浪费。

---

该文章讲的非常好
https://www.redhat.com/zh/topics/virtualization/what-is-virtualization#:~:text=%E5%88%B0%E7%BA%A2%E5%B8%BD%EF%BC%9F-,%E6%A6%82%E8%BF%B0,-%E8%99%9A%E6%8B%9F%E5%8C%96%E6%98%AF

---

如果，一台服务器(计算机硬件)可以安装多个系统，并且，每个操作系统可以同时运行那该多好。

因此，虚拟化技术应运而生，并得到了广泛应用。  
Hypervisor建立在物理计算机，或者操作系统最顶层之上，直接接管物理资源。  
然后，在hypervisor之上建立独立的vm(guest)。  
这样每台vm都是完全独立的，每台vm都可以安装自己独立的操作系统，然后运行只有在特定操作系统之上才能运行的程序。

## 常用的虚拟化类型
### 1. 服务器虚拟化(Server Virtualization)
将硬件资源(cpu, memory, disk sotrage, network)虚拟化，然后，在此之上建立虚拟机(vm)。  
一般是将hypervisor建立在硬件或者操作系统之上(host)，然后，在hypervisor之上建立vm(guest)。

### 2. 操作系统虚拟化(Operating System Level Virtualization)
在op上建立instance，instance还被称作container，然后，在container上建立单独的环境运行app。  
常见的技术有docker。

## 虚拟机(VM)和容器(Container)
相信读完上一节，已经大概能够理解虚拟机和容器的区别了。
### 容器的常见应用
尝尝用于封装应用(APP)
### 虚拟机的常见应用
自己很难用语言概括，虚拟机更像是在封装整个硬件资源和运行环境，更加偏向infrustructure(インフラ)，感觉软件工程师平时很少会碰到。

---
关于容器和虚拟机，这篇文章写的很容易理解。
https://www.redhat.com/zh/topics/containers/containers-vs-vms

---


## 参考
虚拟化一文全解: https://www.redhat.com/zh/topics/virtualization