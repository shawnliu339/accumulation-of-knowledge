- [MySQL Isolation](#mysql-isolation)
  - [1. 关于四种事务隔离级别的介绍：](#1-关于四种事务隔离级别的介绍)
  - [2. 阅读官方文档后的摘要：](#2-阅读官方文档后的摘要)
    - [consistent read模式：](#consistent-read模式)
    - [MVCC(multiversion concurrency control)](#mvccmultiversion-concurrency-control)
  - [Reference:](#reference)

# MySQL Isolation
## 1. 关于四种事务隔离级别的介绍：
关于事务隔离级别可以看下面网址的介绍，很容易理解。  
Ref：https://developer.ibm.com/zh/technologies/databases/articles/os-mysql-transaction-isolation-levels-and-locks/  
但是，要注意上述网址中关于幻读的解释是不正确的。

幻读是指两个进程，进程A读取一个范围的数据后，进程B在进程A所取范围内insert了一条数据，这时候进程A再次读取该范围的时候，会出现本来不该出现的数据。

网上多数说MySQL InnoDB的READ COMMITTED会出现幻读。REPEATABLE READ由于MVCC（文章后面有解释）的控制，所以不会出现幻读。但是，实质上是错误的，后面会说明。  

1. READ COMMITTED  
   * 会出现不可重复读(non-repeatable read)：  
   a. 开启两个事务。事务1和事务2。  
   b. 事务1读取表中所有数据。  
   c. 事务2向表中插入数据并提交。  
   d. 事务1再次读取表中数据，这时和b会不同。
   * 会出现幻读(Phantom Read):  
   假设表中有id=1，id=3的数据。  
   a. 开启两个事务。事务1和事务2.  
   b. 事务1中使用select for update(或者: for share)查询id在1 ~ 3之间的结果。这时会出id为1和3的结果。  
   c. 事务2中将id=2插入到表中，并提交。  
   d. 事务1再次通过select查询id为1 ~ 3的结果，这时候出现id=2的结果。

   明明对select加了锁，但是，仍旧出现了幻读。这是由于 read commited 级别的隔离不会为 for update 和 for share 自动加 next-key lock 。  
   因此，select查询id为1到3的结果时，只有存在数据的id1和3会加锁。但是，还不存在的id2不会加锁。因此，另一个事务可以插入id为2的数据，造成幻读。  
   而 repeatable read 级别会自动对 for update 和 for share 自动加上 next-key lock 。因此，id为2的不存在数据，也在之后不可被插入，因此，不会出现幻读。
   > A next-key lock is an index-record lock plus a gap lock on the gap preceding the index record.

2. REPEATABLE READ  
   官方文档上写明，repeatable read不能防止幻读。  
   repeatable read不可防止幻读:  
   https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_repeatable_read  
   幻读的说明处也写明repeatable read不可防止幻读:  
   https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_phantom  
   我只能理解为，幻读不是select中不出现就可以，而是，也不可以真正的被插入到表中。因为，该行在官方文档中称为幻行(Phantom Rows):  
   https://dev.mysql.com/doc/refman/8.0/en/innodb-next-key-locking.html
   
3. SERIALIZABLE
   Mysql通过行锁解决这个问题，被select的范围和记录都会被锁定，因此，即使是select也会给行加锁，若要对加了读取锁的行进行修改，必须要等待读取锁释放。同一行的读取锁不会相冲突，可以读取。

个人认为，隔离级别除了网上常说的读的区别外，在 locking read ， update 和 delete 上也有明显的区别。  
最明显的就是 read comitted 只加 record-index lock，而不加 next-key lock 。因此，如果选择一个范围的数据进行操作的时候， read commited 只会上锁搜索到的结果。但是， repeatable read 会对整个范围的数据都上锁，即使，是该范围内还不存在的数据，也会通过给他们的index上锁，以保证未来不会被插入。

## 2. 阅读官方文档后的摘要：
阅读官方文档后，发现一些概念可以帮助更加透彻的理解MySQL的事务隔离级别。因此，摘录在下：

### consistent read模式：  
通过MVCC(multiversion concurrency control)返回事务(transaction)开始时刻的表的快照(snapshot)，该快照不会受其他事务的影响而发生变化。

在MySQL的 read commited 和 repeatable read 隔离级别中，如果执行 select 语句，会自动启用 consistent read 。

* Repeatable read ：  
  会在事务开始后的第一个 select 操作后启用 consistent read 。因此，当前事务的 select ，不会受其他事务中写操作的影响。
* Read comitted ：  
  会在事务开始后的每次 select 操作后重置consistent read 。因此，当前事务的 select 会反映其他事务中已提交(commit)的写操作。


### MVCC(multiversion concurrency control)
通过保存snapshot以保证在同一个事物中的表的信息是始终一致的。注意MVCC只能解决dirty read和unrepeatable read问题。但是，不能解决幻读问题。因为，幻读问题需要通过锁来解决。但是，MVCC并不存在对锁的控制。  

下面通过简化版的MVCC来解释其原理。InnoDB通过为每行数据保存两个隐藏的field（created version, deletion version）来实现MVCC。  
以Repeatable Read级别的select为例：  
InnoDB会在Select的同时，对每一行进行检测，以保证2点：  
1. 所有的created version必须小于等于当前的transaction version，这样可以保证所有行都是存在于transaction开始之前的。
2. 所有行的deletion version必须是没有定义，或者必须大于当前的transaction version（前一个transaction中删除的行，会小于当前的transaction version）。这可以保证检索出的行，至少都是在当前transaction中没有被删除的。

通过以上的方式可以在不加锁的情况，解决脏读和不可重复读问题，获得正确的select结果。注意InnoDB只有Read Committed和Repeatable Read级别下才有MVCC模式。

## Reference: 
1. consistent read: https://dev.mysql.com/doc/refman/8.0/en/glossary.html#glos_consistent_read
2. MVCC: https://dev.mysql.com/doc/refman/8.0/en/innodb-multi-versioning.html
3. Phantom Rows: https://dev.mysql.com/doc/refman/8.0/en/innodb-next-key-locking.html
4. Consistent Nonlocking Reads: https://dev.mysql.com/doc/refman/8.0/en/innodb-consistent-read.html
5. Isolation Level: https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html