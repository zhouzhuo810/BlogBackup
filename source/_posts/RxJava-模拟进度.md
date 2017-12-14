---
title: RxJava-模拟进度
date: 2017-12-11 22:35:43
tags:
	- RxJava 
categories: RxJava 
---


```java
//0-100 每隔20ms +1
Observable.just(100)
        .flatMap(new Func1<Integer, Observable<Long>>() {
            @Override
            public Observable<Long> call(Integer integer) {
                return Observable.interval(20, TimeUnit.MILLISECONDS);
            }
        })
        .compose(RxHelper.<Long>io_main())
        .subscribe(new Subscriber<Long>() {
            @Override
            public void onCompleted() {
  
            }
  
            @Override
            public void onError(Throwable e) {
  
            }
  
            @Override
            public void onNext(Long aLong) {
                pb.setProgress(aLong.intValue());
            }
        });
```

### 应用场景

- 计时器
- 进度模拟
- 定时任务