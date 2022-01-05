# Block Storage / File Storage / Object Storage
## Block Storage
## File Storage
## Object Storage
无需mount device。object storage通过api读写数据。  
在实际应用中基本用于存储照片，视频等 **无需极高性能(low performance)** 的数据。  
以及一次写入多次读取的数据(once write and many times to read)。  
最常见应用例就是各种云存储，例如：iCloud，dropbox，Google drive都是object storage实现的。

### Object Storage 4 Components
* id: 用于唯一定义该数据的id
* data: 数据本身
* metadata: 关于数据本身的信息的数据。例如：数据生成时间，数据创建者，数据大小，数据类型等。metadata常用于查找，索引object。
* attribute: 文件权限等信息

Object Storage的Object会存入Bucket。日常通过API对Bucket进行操作，例如，创建，删除，复制Object。



## Reference
1. 大体介绍了block storage, file storage, object storage:  
https://www.youtube.com/watch?v=O-XBhVv2pgE

2. Object Storage介绍：  
https://www.youtube.com/watch?v=ZfTOQJlLsAs