# async profiler
## 简单概括
简单来说：x 轴是抽样数，x 轴越长说明被这个方法被抽样的次数越多，消耗 cpu 时间也越长 y 轴是栈的深度，通常越往上越细，如果发现往上有一个平顶，且平顶很宽，则说明它可能有问题，消耗 cpu 时间较多。


## Reference
https://www.ruanyifeng.com/blog/2017/09/flame-graph.html