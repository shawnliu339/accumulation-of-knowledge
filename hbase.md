# HBase Archtecture
1. youtube video: https://youtu.be/VRD775iqAko
  这个视频解释的非常好理解
2. 文章内容： http://www.uml.org.cn/bigdata/201810181.asp
  更加详细的内容可以通过该文章理解

## MEMO
1. HBase适合少写多读的业务需求。
2. 设计时应该尽量减少Column Family的数量。不同的Column Family会被分割到不同的HFile中，因此，同一个rowkey中的数据若存储在两个不同的CF中，在检索一行时就会检索所有的两个HFile，这会增加检索的成本。（HBase In Action4.1.4）
3. 在Get检索时，可以通过限制RowKey，Column Family和Timestamp来缩小HFile的范围。Column Qualifier不能够缩小检索的HFile的范围，但是，可以减少返回的结果数量，以降低Client或者网络成本。
4. 应当尽量在一次检索请求中涵盖所有的数据结果，而不是通过多次请求获得所需的数据结果。因为，HBase不支持跨行事务，应当尽量避免在Client组建自己含有事务的代码逻辑。
5. 每一个RegionServer的Region应该尽量控制在100以内。（官方文档）
6. 长表(tall table)性能优于宽表(wide table)。（HBase In Action4.1.3）
7. hot-spot是指对某一区域的数据进行频繁访问，因而造成某一区域容易过载。例如，将timestamp作为rowkey进行存储，多数情况都是对最近的数据感兴趣，因此，hbase的底部Region就会被频繁访问，造成过载。但是，如果将timestamp转换为hashkey或者md5再作为rowkey，则可将数据均匀的存储在不同的Region。起到良好的负载均衡作用。（HBase In Action4.1.3）