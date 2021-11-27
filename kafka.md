# Kafka
- [Kafka](#kafka)
  - [Broker](#broker)
  - [Event key的作用(producer中的设定)](#event-key的作用producer中的设定)
  - [Consumer](#consumer)
    - [Consumer Group](#consumer-group)
    - [Consumer Offset](#consumer-offset)
  - [工作中遇到的问题](#工作中遇到的问题)
  - [Reference](#reference)

## Broker
broker就是一个server，多个broker组成一个kafka cluster。  
每个broker中存储一部分topic的partition。  
partition可以有replica。  
一个partition只能有一个leader partition，  
只有该leader partition可以用于接收数据。  
其他partition称为sync partition，  
他们只从leader partition同步数据，但是，不接受数据。
![kafka-architecture](/assets/img/kafka-architecture.png)


## Event key的作用(producer中的设定)
相同key的event会被生产到相同topic的partition中，  
因此，设定event key也可以保证同一个consumer不会从不同的partition中消费同一个key的event。  
另外，同一个partition中的event可以保证消费的顺序一定和写入顺序相同。  

例如：  
现有一个topic，其下有三个partition,event的key为id，value也为id。  
那么，按时间循序产生的以下event流a,b,c,d,a,b,e,c,d  
会被按以下顺序写入partition  
```
0: a, d, a, d  
1: b, b,  
2: c, e, c,  
```
可以看到，所有key为a的event都保证一定会被写入partition0,  
所有key为b的event都保证一定会被写入partition1,  
所有key为c的event都保证一定会被写入partition2,  
其他以此类推。  

假如，我们不给event设定key值，那么，event会被随机写入任意partition。  
仍然用以上的例，则可能生成以下的结果。
```
0: a, d, b
1: a, b, c
2: c, d, e
```
可以看到相同id的event会被分发到不同的partition中，  
因此，无法保证相同id的event消费时的顺序和写入顺序相同。  
在对时间发生顺序有要求系统中，这样的设计是不可的。


## Consumer
一个consumer可以消费多个partition，  
但是，一个consumer group内的不同的consumer不可以消费同一个partition。

### Consumer Group
![consumer-group](/assets/img/kafka-consumer-group.png)
用于给consumer分组。  
注意，一个group中的consumer不可以超过partition的数量。  
(因为，多个consumer不能消费同一个partition，因此，consumer超过了partition的数量，多出的consumer也无法工作。)  

### Consumer Offset
注意，同一个consumer group中的consumer共用一个offset，  
因此，已经被消费的event不会被其他的consumer再次消费。

## 工作中遇到的问题
通过decaton去重的时候，为什么需要给event赋予event key。

在工作中遇到这样的问题，从一个topic消费用户的id(id可重复)，然后，通过该id去另一台服务器获取用户信息，并将该信息存入数据库。  
由于id会出现重复，如果，不去重而直接向另一台服务器请求数据的话，不仅增加了另一台服务器的负担，而且，consumer服务器的效率也不够高。因此，应该在请求数据前进行去重操作。  

decaton提供了去重的compaction功能，该功能很简单，就是consumer消费时延迟几秒钟，待消费的数据积累到一定程度后进行去重，然后，再将处理后的数据传给processor继续处理。  

同事给出建议，在向topic生产数据的时候，一定要将每一个event的用户id作为event key。这样才能更好的发挥decaton的效果。  

理由如下：  
例如，我们有以下的用户id事件流需要produce，a,a,b,b,c,c。  
如果，我们不给event设定event key，那这些id可能会被按以下顺序写入partition。  
```
0: a, b, c
1: a, b, 
```
假设，我们有2个consumer消费以上的topic，那decaton无法实现compaction功能。  
另一台服务器仍会收到两个id为a的请求。

但是，如果我们给event赋予event key。  
那么相同key的event会被写入相同的partition。
```
0: a, a, c
1: b, b,
```
那么，当consumer进行消费时，不同的consumer(一般在不同的服务器的不同线程上)一定不会消费到同一个用户id，因此，至少可以避免，由于不同consumer出现的重复id。  
之后，同一个consumer在等待几秒后，很有可能会消费到相同的id，这时就可以通过decaton的compaction功能对其进行去重，这样在向另一台服务器请求信息时，一个id只需要一次。

## Reference
https://www.youtube.com/watch?v=lh_tjm0yPz4&list=PLt1SIbA8guusxiHz9bveV-UHs_biWFegU&index=6