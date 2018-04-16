# RXjs
**keywords: RXjs、前端、练习、响应式编程**

需要注意区分observable和observable的值

## do
无论对上一个值进行如何的修改，返回的都是上一个observable。

## map
基于给定的方法产生新的observable
```
var clicks = Observable.fromEvent(document, 'click');
var higherOrder = clicks.map((ev) => Observable.interval(1000));
var firstOrder = higherOrder;
firstOrder.subscribe(x => console.log(x));

输出为：
IntervalObservable {_isScalar: false, period: 1000, scheduler: AsyncScheduler}
```
由于map并不解析observable中的observable，所以输出为observable对象的字符串。

## merge
订阅每个给定的输入 Observable (作为参数)，然后只是将所有输入 Observables 的 **所有值** 发 送(不进行任何转换)到输出 Observable 。**注意只是发送值，即内部有observable也只是将其以值的形式输出**

## switch
与mergeAll相似，打平内部observable，但是switch会取消前一个observable的订阅，只保留最新的observable，而mergeAll会保留之前的订阅。

## mergeAll
订阅发出 **Observables 的 Observalbe** ，也称为高阶 Observable 。 每当观察到发出的内部 Observable 时，它会订阅并发出输出 Observable 上的这个 内部 Observable 的所有值。

## mergeMap
将每个源值投射成 Observable ，该 Observable 会合并到输出 Observable 中。  
**将每个值映射成 Observable ，然后使用mergeAll打平所有的内部Observables。**

## switchMap
与mergeMap类似，但是，会取消前一个Observable。

　
