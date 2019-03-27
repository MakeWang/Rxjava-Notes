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

## 变换操作符
* [map() 将一个事件类型转换成你想要的数据类型并返回](#map)
* [flatMap() 将一个事件拆分后组成一个新的事件发送，是无序排列](#flatMap)
* [concatMap() 将一个事件拆分后组成一个新的事件发送，是有序排列](#concatMap)
* [buffer() 每次获取一定数量的事件放到缓存区中并发送](#buffer)

## 组合/合并操作符
* [concat() 组合多个被观察者发送数据，合并按顺序执行，数量 <= 4](#concat)
* [concatArray() 组合多个被观察者发送数据，合并按顺序执行，数量 > 4](#concatArray)
* [merge() 组合多个被观察者发送数据，合并按照时间顺序，数量 <= 4](#merge)
* [mergeArray() 组合多个被观察者发送数据，合并按照时间顺序，数量 > 4](#mergeArray)
* [concatDelayError() 异常捕获，发生异常也会继续执行，按顺序执行](#concatDelayError)
* [mergeDelayError() 异常捕获，发生异常也会继续执行，按时间执行](#mergeDelayError)
* [zip() 针对多个被观察者事件进行合并，按照个数合并](#zip)
* [combineLatest() 和zip事件合并一样，按照时间来合并](#combineLatest)
* [combineLatestDelayError() combineLatest事件合并异常处理](#combineLatestDelayError)
* [reduce() 将一个被观察者前2个数据聚合后，再进行聚合合并后一起输出](#reduce)


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


map
-----------------------------------------------------------
作用：对被观察者发送的每1个事件都通过指定的函数处理，从而变换成另外一种事件即，将被观察者发送的事件转换为任意的类型事件</br>
```java
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
        emitter.onNext(1);
        emitter.onNext(2);
        emitter.onNext(3);
        emitter.onComplete();
    }
}).map(new Function<Integer, String>() {
    @Override
    public String apply(Integer integer) throws Exception {
        return integer + "只小鸭子";
    }
}).subscribe(new Consumer<String>() {
    @Override
    public void accept(String s) throws Exception {
        log(s);
    }
});
```




flatMap
----------------------------------------------------------------
作用：将被观察者发送的事件序列进行拆分或单独转换，再合并成一个新的事件序列，最后再进行发送</br>
```java
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
        emitter.onNext(1);
        emitter.onNext(2);
        emitter.onNext(3);
        emitter.onComplete();
    }
}).flatMap(new Function<Integer, ObservableSource<String>>() {
    @Override
    public ObservableSource<String> apply(Integer integer) throws Exception {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 3; i++) {
            list.add(integer + "栋"+i+"号");
        }
        return Observable.fromIterable(list);
    }
}).subscribe(new Consumer<String>() {
    @Override
    public void accept(String s) throws Exception {
        log(s);
    }
});

----------------结果----------------
I/wangyin: 2栋0号
I/wangyin: 2栋1号
I/wangyin: 2栋2号
I/wangyin: 1栋0号
I/wangyin: 1栋1号
I/wangyin: 1栋2号
I/wangyin: 3栋0号
I/wangyin: 3栋1号
I/wangyin: 3栋2号

```



concatMap
-------------------------------------------------------
作用：拆分或重新合并生成的事件序列的顺序被观察者旧序列生产的顺序</br>
```java
Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
        emitter.onNext(1);
        emitter.onNext(2);
        emitter.onNext(3);
        emitter.onComplete();
    }
}).concatMap(new Function<Integer, ObservableSource<String>>() {
    @Override
    public ObservableSource<String> apply(Integer integer) throws Exception {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 3; i++) {
            list.add(integer + "栋"+i+"号");
        }
        return Observable.fromIterable(list);
    }
}).subscribe(new Consumer<String>() {
    @Override
    public void accept(String s) throws Exception {
        log(s);
    }
});


----------------结果----------------
I/wangyin: 1栋0号
I/wangyin: 1栋1号
I/wangyin: 1栋2号
I/wangyin: 2栋0号
I/wangyin: 2栋1号
I/wangyin: 2栋2号
I/wangyin: 3栋0号
I/wangyin: 3栋1号
I/wangyin: 3栋2号
```


buffer
------------------------------------------------------
作用：定期从被观察者（Obervable）需要发送的事件中获取一定数量的事件或放到缓存区中，最终发送</br>
```java
// 参数1   =    缓存区大小
// 参数2   =    步长
Observable.just(1,2,3,4,5).buffer(3,1).subscribe(new Consumer<List<Integer>>() {
    @Override
    public void accept(List<Integer> integers) throws Exception {
        log("缓存区数量："+integers.size());
        for (Integer item : integers) {
            log("缓存数据："+item);
        }
    }
});


I/wangyin: 缓存区数量：3
I/wangyin: 缓存数据：1
I/wangyin: 缓存数据：2
I/wangyin: 缓存数据：3
I/wangyin: 缓存区数量：3
I/wangyin: 缓存数据：2
I/wangyin: 缓存数据：3
I/wangyin: 缓存数据：4
I/wangyin: 缓存区数量：3
I/wangyin: 缓存数据：3
I/wangyin: 缓存数据：4
I/wangyin: 缓存数据：5
I/wangyin: 缓存区数量：2
I/wangyin: 缓存数据：4
I/wangyin: 缓存数据：5
I/wangyin: 缓存区数量：1
I/wangyin: 缓存数据：5

```


concat
-------------------------------------------
作用：合多个被观察者一起发送数据，合并后按发送顺序串行执行,观察者数量≤4</br>
```java
Observable.concat(Observable.just(1, 2, 3),
        Observable.just(1, 2, 3),
        Observable.just(4, 5, 6),
        Observable.just(7, 8, 9)).subscribe(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) throws Exception {
        log(String.valueOf(integer));
    }
});

```


concatArray
-------------------------------------------
作用：合多个被观察者一起发送数据，合并后按发送顺序串行执行,观察者数量>4</br>
```java
Observable.concatArray(Observable.just(1, 2, 3),
        Observable.just(1, 2, 3),
        Observable.just(4, 5, 6),
        Observable.just(7, 8, 9),
        Observable.just(10,11,12)).subscribe(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) throws Exception {
        log(String.valueOf(integer));
    }
});

```


merge
--------------------------------------------
作用：组合多个被观察者一起发送数据，合并后按时间线并行执行,观察者数量≤4</br>
```java
Observable.merge(Observable.intervalRange(0, 3, 1, 1, TimeUnit.SECONDS),
        Observable.intervalRange(2, 3, 1, 1, TimeUnit.SECONDS)).subscribe(new Consumer<Long>() {
    @Override
    public void accept(Long aLong) throws Exception {
        log(String.valueOf(aLong));
    }
});
```

mergeArray
--------------------------------------------
作用：组合多个被观察者一起发送数据，合并后按时间线并行执行,观察者数量>4</br>
```java
Observable.mergeArray(Observable.intervalRange(0, 3, 1, 1, TimeUnit.SECONDS),
        Observable.intervalRange(2, 3, 1, 1, TimeUnit.SECONDS)).subscribe(new Consumer<Long>() {
    @Override
    public void accept(Long aLong) throws Exception {
        log(String.valueOf(aLong));
    }
});
```



concatDelayError
------------------------------------------------------------
作用：异常捕获，发生异常也会继续执行，按顺序执行</br>
```java
Observable<Integer> o1 = Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
        emitter.onNext(1);
        emitter.onNext(2);
        emitter.onError(new NullPointerException());
        emitter.onNext(3);
    }
});
Observable<Integer> o2 = Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
        emitter.onNext(4);
        emitter.onNext(5);
    }
});

Observable.concatArrayDelayError(o1,o2).subscribe(new Observer<Integer>() {
    @Override
    public void onSubscribe(Disposable d) {

    }

    @Override
    public void onNext(Integer integer) {
        log("结果："+integer);
    }

    @Override
    public void onError(Throwable e) {
        log("发送异常");
    }

    @Override
    public void onComplete() {

    }
});

----------结果------------------------
I/wangyin: 结果：1
I/wangyin: 结果：2
I/wangyin: 结果：4
I/wangyin: 结果：5

```

mergeArrayDelayError
------------------------------------------------------------
作用：异常捕获，发生异常也会继续执行，按时间顺序执行</br>
```java
Observable<Integer> o1 = Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
        emitter.onNext(1);
        emitter.onNext(2);
        emitter.onError(new NullPointerException());
        emitter.onNext(3);
    }
});
Observable<Integer> o2 = Observable.create(new ObservableOnSubscribe<Integer>() {
    @Override
    public void subscribe(ObservableEmitter<Integer> emitter) throws Exception {
        emitter.onNext(4);
        emitter.onNext(5);
    }
});

Observable.mergeArrayDelayError(o1,o2).subscribe(new Observer<Integer>() {
    @Override
    public void onSubscribe(Disposable d) {

    }

    @Override
    public void onNext(Integer integer) {
        log("结果："+integer);
    }

    @Override
    public void onError(Throwable e) {
        log("发送异常");
    }

    @Override
    public void onComplete() {

    }
});

----------结果------------------------
I/wangyin: 结果：1
I/wangyin: 结果：2
I/wangyin: 结果：4
I/wangyin: 结果：5

```



zip
-------------------------------------------
作用：合并多个被观察者（Observable）发送的事件，生成一个新的事件序列（即组合过后的事件序列），并最终发送</br>
```java
Observable<String> o2 = Observable.create(new ObservableOnSubscribe<String>() {
    @Override
    public void subscribe(ObservableEmitter<String> emitter) throws Exception {
        emitter.onNext("A");
        Thread.sleep(2000);
        emitter.onNext("B");
        Thread.sleep(2000);
        emitter.onNext("C");
        Thread.sleep(2000);
        emitter.onNext("D");
        Thread.sleep(2000);
        emitter.onComplete();
    }
});

Observable<String> o3 = Observable.create(new ObservableOnSubscribe<String>() {
    @Override
    public void subscribe(ObservableEmitter<String> emitter) throws Exception {
        emitter.onNext("A");
        Thread.sleep(2000);
        emitter.onNext("B");
        Thread.sleep(2000);
        emitter.onNext("C");
        Thread.sleep(2000);
        emitter.onNext("D");
        Thread.sleep(2000);
        emitter.onComplete();
    }
});
Observable.zip(o1, o2, new BiFunction<Integer, String, String>() {
    @Override
    public String apply(Integer integer, String s) throws Exception {
        return integer + s;
    }
}).subscribe(new Consumer<String>() {
    @Override
    public void accept(String s) throws Exception {
        log("数据拿到：" + s);
    }
});

-----------结果-----------------
I/wangyin: 数据拿到：1A
I/wangyin: 数据拿到：2B
I/wangyin: 数据拿到：3C
I/wangyin: 数据拿到：onComplete

```


combineLatest
----------------------------------------
作用：当两个Observables中的任何一个发送了数据后，将先发送了数据的Observables 的最新（最后）一个数据与另外一个Observable发送的每个数据结合，最终基于该函数的结果发送数据</br>
```java
Observable.combineLatest(Observable.just(1L, 2L, 3L), Observable.intervalRange(0, 3, 1, 1, TimeUnit.SECONDS), new BiFunction<Long, Long, Long>() {
    @Override
    public Long apply(Long integer, Long aLong) throws Exception {
        Log.i("wangyin", "合并结果：" + integer + "+" + aLong);
        return integer + aLong;
    }
}).subscribe(new Consumer<Long>() {
    @Override
    public void accept(Long s) throws Exception {
        Log.i("wangyin", "结果：" + s);
    }
});

-------------结果-----------------
I/wangyin: 合并结果：3+0
I/wangyin: 结果：3
I/wangyin: 合并结果：3+1
I/wangyin: 结果：4
I/wangyin: 合并结果：3+2
I/wangyin: 结果：5

```


combineLatestDelayError
-------------------------------------------
作用：作用类似于concatDelayError（） / mergeDelayError（） ，即错误处理</br>



reduce
------------------------------------------------------------
作用：把被观察者需要发送的事件聚合成1个事件&发送</br>
```java
Observable.just(1,2,3,4).reduce(new BiFunction<Integer, Integer, Integer>() {
    @Override
    public Integer apply(Integer integer, Integer integer2) throws Exception {
        log(integer + " * " + integer2);
        return integer * integer2;
    }
}).subscribe(new Consumer<Integer>() {
    @Override
    public void accept(Integer integer) throws Exception {
        log("结果："+integer);
    }
});


----------结果----------------------
I/wangyin: 1 * 2
I/wangyin: 2 * 3
I/wangyin: 6 * 4
I/wangyin: 结果：24



```


























