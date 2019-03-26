# Rxjava-Notes
Rxjava的一些操作符整理
==============================

* [just() 直接发送事件，最多只能发送10个参数](#just)
* [fromArray() 直接传入数组发送](#fromArray)


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
