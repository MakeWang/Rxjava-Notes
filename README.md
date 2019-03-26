# Rxjava-Notes
Rxjava的一些操作符整理
==============================

## 快速创建事件<br/>
* [just() 直接发送事件，最多只能发送10个参数](#just)
* [fromArray() 直接传入数组发送事件](#fromArray)
* [fromIterable() 直接传入List集合发送事件](#fromIterable)

## 延时发送事件
* [defer() 只有在订阅的时候才会创建被观察者](#defer)
* [timer() 延时指定时间发送事件](#timer)
* [interval() 每隔指定时间，无限发送事件](#interval)
* [intervalRange() 每隔指定时间，可以指定发送事件的数量发送事件](#intervalRange)
* [range() 指定一个范围内发送事件](#range)


just
----------------------------------
作用：快速创建一个被观察者对象，直接发送事件，最多只能发送10个参数</br>
```java
Observable.just(1,2,3,4).subscribe(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) throws Exception {
        log("值："+integer);
    }
});
```

fromArray
-------------------------------------------
作用：快速创建一个被观察者对象，直接传入数组数据后遍历发送事件</br>
```java
Integer []arr = {1,2,3,4};
Observable.fromArray(arr).subscribe(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) throws Exception {
        log("值："+integer);
    }
});

```

fromIterable
--------------------------------------------
作用：快速创建一个被观察者对象，直接传入List集合后遍历发送事件</br>
```java
List<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);
list.add(4);
Observable.fromIterable(list).subscribe(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) throws Exception {
        log("值："+integer);
    }
});
```


defer
--------------------------------------------
作用：直到有观察者（Observer）订阅时，才会动态创建被观察者（Observable）对象进行发送事件</br>
```java
Observable<Integer> observable = Observable.defer(new Callable<ObservableSource<? extends Integer>>() {
    @Override
    public ObservableSource<? extends Integer> call() throws Exception {
        log("创建");
        return Observable.just(index);
    }
});
log("赋值");
index = 2;
observable.subscribe(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) throws Exception {
        log("值是：" + integer);
    }
});

------结果-------
I/wangyin: 赋值
I/wangyin: 创建
I/wangyin: 值是：2
```

timer
----------------------------------------------------
作用：快速创建一个被观察者，延时指定时间发送事件</br>
```java
//延长3秒发送事件
Observable.timer(3, TimeUnit.SECONDS).subscribe(new Consumer<Long>() {
    @Override
    public void accept(Long aLong) throws Exception {
        log("返回：" + aLong);
    }
});
```

interval
---------------------------------------
作用：快速创建一个被观察者，延时指定时间，无限递增发送事件</br>
```java
// 参数1   =    第一次延迟时间
// 参数2   =    延迟间隔时间
// 参数3   =    时间单位
Observable.interval(0, 3, TimeUnit.SECONDS).subscribe(new Consumer<Long>() {
    @Override
    public void accept(Long aLong) throws Exception {
        log("执行：" + aLong);
    }
});
```

intervalRange
----------------------------------------
作用：快速创建一个被观察者，延时指定时间，可指定发送数量事件</br>
```java
// 参数1   =     事件序列起点
// 参数2   =     事件数量
// 参数3   =     第一次事件延迟发送时间
// 参数4   =     间隔时间
// 参数5   =     时间单位
Observable.intervalRange(2, 10, 1, 2, TimeUnit.SECONDS).subscribe(new Consumer<Long>() {
    @Override
    public void accept(Long aLong) throws Exception {
        log("开始：" + aLong);
    }
});

----------结果------------
I/wangyin: 开始：2
I/wangyin: 开始：3
I/wangyin: 开始：4
I/wangyin: 开始：5
I/wangyin: 开始：6
I/wangyin: 开始：7
I/wangyin: 开始：8
I/wangyin: 开始：9
I/wangyin: 开始：10
I/wangyin: 开始：11
```


range
-------------------------------------
作用：快速创建一个被观察者，指定一个范围内发送事件，参数不能不为负数，不能延时发送事件</br>
```java
// 参数1    =    事件序列起点
// 参数2    =    事件数量
Observable.range(1,10).subscribe(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) throws Exception {
        log("值："+integer);
    }
});

```






















