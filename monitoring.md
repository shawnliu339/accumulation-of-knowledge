# Monitoring
**keywords: monitoring，监控，kibana，prometheus** 
- [Monitoring](#monitoring)
  - [1. Monitoring做哪些事情？](#1-monitoring做哪些事情)
  - [2. Monitoring演变以及分类](#2-monitoring演变以及分类)
    - [2.1 演变](#21-演变)
    - [2.2 Monitoring的分类](#22-monitoring的分类)
      - [2.2.1 Profiling](#221-profiling)
      - [2.2.2 Tracing](#222-tracing)
      - [2.2.3 Logging](#223-logging)
      - [2.2.4 Metrics](#224-metrics)
  - [3. Reference](#3-reference)
## 1. Monitoring做哪些事情？
* Alerting:
* Debuging:
* Trending:  
除了解决bug，很多时候我们希望了解系统随时间变化的被使用状况。这种使用方式称为Trending。通过Trending我们可以更好的设计系统，例如：capacity planning。
* Plumbing:  
没看懂

## 2. Monitoring演变以及分类
### 2.1 演变
1. 通过定期执行一段checker脚本，确认被监控的服务器是否还存活。如果，未存活，则发出alert。对标公司的l7 or l4 helth checker
2. 通过收集metric，存储metric，以及对存储的metric进行查询，以起到监控作用。对标公司的Grafana
3. 日志类型监控。存储应用的日志，并定期对日志产生报告，以起到监控作用。对标公司的Imon + Kibana
4. 现代化监控基本为结合以上多种方式进行。

### 2.2 Monitoring的分类
Monitoring所关注的对象无外乎就是程序中产生的event。  
例如：Http的request，response，这些都可以称之为event。  
监控这些event就可以帮助我们解决系统问题，或者监控系统的运行及时Alert，或者优化系统的性能。  

这些event同时还包含大量的Context。例如：
* HTTP的request包含收到和发送的IP，cookie，URL等等。
* Response和包含status code，response body。

如果监控的过程中记录所有这些Context信息，将会占用大量空间。  
因此，实际监控中会用以下方法进行结合进行监控。

#### 2.2.1 Profiling
选取某一时刻或者时间段的所有event和其Context进行存储和分析。  
常用技术：  
Profiling的主要功能是用于策略性的debug分析。例如，分析某一时刻的某一代码块，哪里消耗CPU，哪里消耗内存等。  
可以看出Profiling是通过牺牲时间来换取空间的手段。

#### 2.2.2 Tracing
取样所有event中的一部分event进行监控。  
例如：对某一类request取样，监控request中的所有操作，可以知道是哪个操作消耗时间，或造成延迟。  
常用技术：Zipkin, Jaeger  
在分布式系统中可以通过request的unique id将不同系统中的处理串联起来，从而知道每个系统中所消耗的时间。  
可以看出Tracing通过取样（Sampling）减少event所占用的空间。

#### 2.2.3 Logging
通过记录某一部分event以及与该event相关的Context中的一部分filed进行监控。  
常用技术：ELK Stack（Elasticsearch，Logstash，Kibana）

#### 2.2.4 Metrics
metrics会在很大程度上忽略Context的信息，而通过聚合查询随着时间记录的metric进行监控和分析。  
比如，metric可以帮助你追踪应用中的各子系统的延迟以及数据量。使你可以之后延迟发生在哪个子系统。确定子系统后可以再通过logging或者Profilling的方式定位是哪个用户的请求发生了问题。  
可以注意到metric的方式更多关注的是整个系统的运行状况以及性能，而不是具体的request调用event的状况及性能。  
例如：系统最近1min处理了多少请求，花了多少时间处理，其中多少个链接了数据库，多少是链接了缓存。  
常用技术：Prometheus + Grafana

## 3. Reference
Prometheus: Up & Running的第一章第一节What is prometheus