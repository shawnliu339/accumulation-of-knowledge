- [CDN(Content Delivery Network)](#cdncontent-delivery-network)
  - [概念](#概念)
    - [1. 如下图ASIS为无CDN架构:](#1-如下图asis为无cdn架构)
    - [2. 如上图TOBE为有CDN架构：](#2-如上图tobe为有cdn架构)
    - [3. 动态网站的cdn加速](#3-动态网站的cdn加速)
  - [工作中的实际使用](#工作中的实际使用)
    - [减少响应时间](#减少响应时间)
    - [操作界面](#操作界面)

# CDN(Content Delivery Network)
## 概念
CDN全称为内容分发网络，其本质就是一个大型的缓存系统。  
CDN主要解决以下2个问题：
* 加快响应速度
* 减少服务器压力

### 1. 如下图ASIS为无CDN架构:  
①browser通过网址，访问DNS服务器  
②DNS服务器返回origin server的实际的ip地址  
③browser通过返回的实际ip地址访问origin server  

这会带来两个很显著的问题：  
1. 如果用户在中国，origin server在美国，那么响应速度一定会很慢。
2. 用户直接访问origin server，如果，请求过多origin server可能会直接宕机。

![cdn](./assets/img/cdn.png)  
### 2. 如上图TOBE为有CDN架构：  
①browser通过网址，访问DNS服务器  
②DNS服务器返回origin server的cname地址（cname地址实质就是cdn服务器的地址，例如：cache.cdn-example.com）
③browser通过cname地址访问cdn服务器，cdn服务器会将请求自动转发给最近的cdn服务器。
④cdn服务器会将缓存的信息返回给用户。

⑤cdn服务器会根据缓存的ttl，自动从origin server拉取新数据，更新原有数据。

可以看到browser不在直接访问origin server，而是访问cdn服务器。  
而cdn服务器又会分散部署在各个国家，browser的请求会根据用户的位置，自动分配给就近的cdn服务器进行处理，因此，可以有效的解决由于距离过远而造成的响应时间过程的问题。  

同时，browser不再直接访问origin server，而是访问cdn的缓存服务器，因此，origin server的负担会大大降低。

### 3. 动态网站的cdn加速
主要原理是通过cdn节点进行路径最优化。 

例如，用户在电信的网络下，服务器在联通的网络下，  
如果没有cdn，用户的请求需要先传输到总路由器，然后，再转发给联通的网络，最后，传输到联通的服务器上。  

用户(北京电信) -> 供应商的总路由器(不同网络间传输需要经历无数的路由) -> 服务器(上海联通)   

如果，传输的路由过多或者距离过远，就会造成延迟。

**CDN服务器通过路径最优解决该问题**  
用户(北京电信) -> CDN北京电信节点 -> CDN北京联通节点(同一网络间传输) -> 服务器(联通)  
cdn北京的节点往往在一个机房，这样电信和联通间不同的网路的传输，可以在同一机房内的路由器进行，大大减少了传输时间，减少了延迟。  
另外，虽然页面是动态生成，但是，图片等信息仍是静态文件，因此，可以通过cdn缓存，仍旧能够减少传输时间。

## 工作中的实际使用
### 减少响应时间
服务器在东京，但是，用户主要在印尼，泰国和台湾。  
因此，用户访问服务器的响应时间较长。  
加入akamai的cdn后，响应时间大大减少。

### 操作界面
没有实际配过，但是，估计和dns的配置页面类似。  
主要配以下内容：
* cname: dns处会将自己服务器的domain和该cname配对。实质就是自己服务器和cdn服务器的mapping。
* domain name: CDN供应商提供的domain
* origin: 原始服务器domain
