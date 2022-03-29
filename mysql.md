# MySQL
## Table Type
mysql的table type大概可以分为两类，  
tansaction-safe(TST)和non-transaction-safe(NTST)。  
如名字TST支持事务管理，支持外键，InnoDB是常用的TST table。  
MyISAM是常见的NTST table。

## Data Type
### Optimized Storage
限制column的size可以起到优化存储的效果。  
例如，123456按int存储只需使用4byte的空间，  
但是，若使用string则需要占用7byte的空间。  
需要注意的是：  
不要过度优化，因为，改变column的size有时需要rebuild整个表，  
如果，表中存储的data很多，这可能会花费大量的时间。  
因此，在设置column的size的时候，可以适度给出充裕的空间，以防止后期空间不足的问题。
* Numeric: 除decimal和bit根据长度决定存储空间外，其余的tinyint，int(4byte)等等numeric类型都是使用固定的存储空间。
* String And Binary Data: 不定长，根据字符串长度动态决定存储容量。

## Index
* Index和Key的区别：Index is a list of key.
* Null is undefined，所以，Null不等于Null，所以，unique index可以存多个Null值

## The Query Optimizer
explain用于查询query的性能。  
optimization的详细显示方法：  
```
SET optimizer_trace='enabled=on';
explain query;
SELECT * FROM information_schema.OPTIMIZER_TRACE;
```
然后检查cost的使用时间，即可知道MySQL的优化细节。

## 优化query
### 抓取了你实际不会用到的数据
1. 是否抓取了不需要的row。
2. 在join的时候是否用`select * `抓取了所有column。最好，只select需要的column。
3. 重复抓取

### Mysql检查了过多数据

## 关于MySQL的Cache
由于现在的系统越来越注重高并发，因此，Mysql的cache功能已在5.7中被废除，并将在8.0中移除。

## Partitioning
https://dev.mysql.com/doc/refman/8.0/en/partitioning-overview.html  
一般情况下，MySQL会将同一张表的数据，存储在文件系统上的同一位置(例如同一目录下)。而partitioning可以将同一张表中的数据通过指定的规则，存储在文件系统上的不同位置(例如不同目录下)。

horizontal partitioning: 同一张表中的行存储在不同的位置。  
vertical partitioning: 同一张表中的列存储在不同的位置。  
MySQL8.0只支持horizontal partitioning。

使用Partitioning的优势：  
* 与将数据存储在单一硬盘上相比，partitioning可以让更多的数据存入一张表中。
* 可以更加容易的删除不用的数据。因为，一个partition中的数据会存储在一起，因此，删除一个partition可以一次性删除相关数据而不影响到其他数据。
* 可以优化query。不相关的partition数据会被自动排除，只扫描相关的partition中的数据。甚至，在query中也可以直接指定需要查询的partition，直接忽略其他partition。
