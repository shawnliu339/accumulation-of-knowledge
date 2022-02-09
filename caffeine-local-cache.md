# Caffeine - Local Cache
## 工作中遇到的使用场景
读多写少，数据变化不频繁的数据可以进行缓存。
在工作中遇到的比较典型的案例，有两个：
1. 统计最后完成时间的list可以通过local cache缓存。  
   因为，统计的完成时间一天只更新一次，  
   而且，每个用户在第一次访问页面是都一定会读取该列表。  
   因此，是一个典型的读多写少，数据不频繁的案例。  
   通过缓存可以加快首页的加载时间。
2. Interceptor中从数据库读取category，并根据category判断账号是否有访问api的权限。  
   高并发的场景下，Interceptor处理每一个request都需要去数据库取category信息，压力过大。  
   而且category信息也属于变化频率较低的数据，  
   因此，该场景也非常适合使用local cache。  
   减少api读取时间，降低数据库服务器的压力。

## 比较好的参考博客
关于caffeine cache的介绍。新手向，简单易懂：
https://segmentfault.com/a/1190000038665523

文章中认为caffeine的一个最重要的的优势就是提供了时间淘汰机制。
而Google的concurrentLinkedHashMap只能等到cache达到上限，才最清理缓存。

