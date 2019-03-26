# Rxjava-Notes
Rxjava的一些操作符整理
==============================

## 快速创建事件<br/>
* [just() 直接发送事件，最多只能发送10个参数](#just)
* [fromArray() 直接传入数组发送事件](#fromArray)
* [fromIterable() 直接传入List集合发送事件](#fromIterable)

## 延时发送事件
* [defer() 只有在订阅的时候才会创建被观察者](#defer)

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
2019-03-26 14:26:42.390 26835-26835/com.wy.video I/wangyin: 赋值
2019-03-26 14:26:42.390 26835-26835/com.wy.video I/wangyin: 创建
2019-03-26 14:26:42.391 26835-26835/com.wy.video I/wangyin: 值是：2

```

