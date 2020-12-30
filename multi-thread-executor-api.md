# Java Executor Service
Ref: https://zhuanlan.zhihu.com/p/33266682

# Future和CompletableFuture的区别
* Future的取值方法过少，只可用isDone()方法进行轮询取值，或者通过get()阻断线程进行取值。均为同步方法，与多线程的异步理念相违。

* 而CompletableFuture提供了各种方法，可以进行响应式编程，对结果进行异步的操作。例如：
  * thenApply() 对上一步的结果进行转换操作，并返回新的转换结果。类似于集合的map()方法。
  * thenRun() 对上一个结果不进行操作，只是上一步操作完成后，顺序执行thenRun()。
  * handle(BiFunction<? super T, Throwable, ? extends U> fn) 上一个操作的执行结果分为值和exception传给handle()。
    
    如果前一步操作发生异常exception将不为空
    
    Ref： https://blog.csdn.net/Bruce_Bee/article/details/87815137
