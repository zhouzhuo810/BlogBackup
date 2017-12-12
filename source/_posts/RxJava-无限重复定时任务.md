---
title: RxJava-无限重复定时任务
date: 2017-12-12 10:57:15
tags:
	- RxJava 
categories: RxJava 
---

### 开启任务

```java
    private void getAllDatas(final long delay) {
  
    	//解除上次订阅
        if (subscribe != null) {
            subscribe.unsubscribe();
            subscribe = null;
        }
  
        subscribe = Observable.interval(delay, TimeUnit.MILLISECONDS)
                .flatMap(new Func1<Long, Observable<GetSomeThingResult>>() {
                    @Override
                    public Observable<GetSomeThingResult> call(Long aLong) {
                    	//接口调用
                        return Api.getDefaultApi().getSomeThing();
                    }
                })
                .observeOn(AndroidSchedulers.mainThread())
                .subscribeOn(Schedulers.io())
                .subscribe(new Subscriber<GetSomeThingResult>() {
                    @Override
                    public void onCompleted() {
  
                    }
  
                    @Override
                    public void onError(Throwable e) {
                        ToastUtils.showCustomBgToast(getString(R.string.no_net_text) + e.toString());
                        getAllDatas(Constants.AUTO_REFRESH_DURATION);
                    }
  
                    @Override
                    public void onNext(GetSomeThingResult result) {
                    	//... do some thing with data
  
                        getAllDatas(Constants.AUTO_REFRESH_DURATION);
                    }
                });
  
    }
```

<!-- more -->

### 取消任务

```java
    @Override
    protected void onDestroy() {
        super.onDestroy();
        try {
            if (subscribe != null && !subscribe.isUnsubscribed()) {
                subscribe.unsubscribe();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```