- [阅读官方文档笔记](#阅读官方文档笔记)
  - [首先对micrometer的理解：](#首先对micrometer的理解)
  - [micrometer自动加工metric name](#micrometer自动加工metric-name)
  - [Spring Boot中使用micrometer](#spring-boot中使用micrometer)
# 阅读官方文档笔记
## 首先对micrometer的理解：  
micrometer提供了可以跨monitor system平台的，向code中插入instrumentation的接口。  
上面的话过于抽象，需要一步一步解释：
1. 首先，monitor system是指，prometheus这类用于从应用接收metric，保存metric，然后根据metric对应用进行监控的监控系统。  
2. 其次，是instrumentation。monitor system为了可以从应用拿到metric，需要我们开发人员在代码中插入instrumentation，这些instrumentation会生成metric，然后，再将metric发送给monitor system。  
** instrumentation中文为仪表盘，其实，我觉得还挺形象的。因为，只有有了仪表盘，我们才能根据仪表盘的读数知道机器运行的状态。应用的监控亦是如此。
3. 知道了以上两个概念后，我们要知道，不同的monitor system对传送来的metric的格式有各自的规定，这就造成了不同平台间的metric不能通用，不可跨平台。  
而micrometer就是这样一个框架，向开发人员提供了统一的插入instrumentation的接口，然后，框架内部再根据要传送的monitor system，将instrumentation生成的metric转化成对应平台的格式，以此达到跨平台的效果。

## micrometer自动加工metric name
1. micrometer会自动对Timer添加，_count和_sum，自动生成两个新的metric  
   * `${name}_count` - Total number of all calls.  
   * `${name}_sum` - Total time of all calls.

   Ref: https://micrometer.io/docs/registry/prometheus#_timers
2. micrometer会自动将metric名转化为对应monitor system的命名规则。  
   例如：
   * 定义metric名为`service.feature.name`。则在prometheus的情况下，会被自动转为 `service_feature_name`。
   * counter的metric会自动加`_total`后缀。
   * timer会自动加`_second`后缀。
   * 具体可以参看官方[repository](https://github.com/micrometer-metrics/micrometer/blob/master/implementations/micrometer-registry-prometheus/src/main/java/io/micrometer/prometheus/PrometheusNamingConvention.java#L54)。

## Spring Boot中使用micrometer
1. Spring boot会用micrometer默认生成一些常用的metrics。  
   比较常用的两个：
   * Spring MVC Metrics (http.server.request):   
   用于监控Server所处理的请求，接受的request，返回的response等。
   * Http Client Metrics (http.client.requests):  
   用于监控通过WebClient向其他服务器发送请求的情况。例如：发送请求数量，返回response的时间，返回response的状态等。  
   ```
   Note: 
   必须要通过Spring的WebClient发送的请求，才可以被自动监控。
   公司的例子就很好，因为，没有使用Spring的WebClient，所以，必须要手动添加监控。
   ```