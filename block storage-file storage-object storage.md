- [Block Storage / File Storage / Object Storage](#block-storage--file-storage--object-storage)
  - [Block Storage](#block-storage)
  - [File Storage](#file-storage)
  - [Object Storage](#object-storage)
    - [Object Storage 4 Components](#object-storage-4-components)
  - [3种Storage的比较](#3种storage的比较)
  - [Reference](#reference)

# Block Storage / File Storage / Object Storage
## Block Storage
Amazon的EBS(Elastic Block Storage)

## File Storage
Amazon的EFS(Elastic File Storage)

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

## 3种Storage的比较
| Category | Block Stroage                                   | File Storage                                                  | Object Storage                        | Memo                                                                                                                                           | 
| -------- | ----------------------------------------------- | ------------------------------------------------------------- | ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | 
| Use case | Workloads require IOPS(DBMS, Elasticsearch, ..) | Share moderate size contents(bigger than 32K) between servers | Media like picture, photo, video etc. | 处理数量众多的小文件(大小小于32k，数量多于1000万)时可以考虑BS，而不是，FS或者OS。<br><br>4k random IO超过1000 IOPS的时候，应该考虑BS而不是FS。 | 
|          |                                                 |                                                               |                                       |                                                                                                                                                | 
|          |                                                 |                                                               |                                       |      

2000 IOPS 4k ramdom IO: 每秒可以进行2000次，4k大小的数据的读写。

## Reference
1. 大体介绍了block storage, file storage, object storage:  
https://www.youtube.com/watch?v=O-XBhVv2pgE

2. Object Storage介绍：  
https://www.youtube.com/watch?v=ZfTOQJlLsAs