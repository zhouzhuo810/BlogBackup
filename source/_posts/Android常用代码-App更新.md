---
title: Android常用代码-App更新
date: 2017-11-10 16:52:50
tags:
	- Android
categories: Android常用代码
---

## APP更新逻辑


```java
    private void checkUpdate() {
        String key = SpUtil.getKey();

        Api.getApi0()
                .CheckUpdate(key, SystemUtil.getPackageInfo(this).versionCode)
                .compose(RxHelper.<CheckUpdateResult>io_main())
                .subscribe(new Subscriber<CheckUpdateResult>() {
                    @Override
                    public void onCompleted() {
  
                    }
  
                    @Override
                    public void onError(Throwable e) {

                    }

                    @Override
                    public void onNext(CheckUpdateResult checkUpdateResult) {
                        if (checkUpdateResult.getCode() == 1) {
                            CheckUpdateResult.DataEntity data = checkUpdateResult.getData();
                            showUpdate(data);
                        }
                    }
                });
    }
  
    private void showUpdate(final CheckUpdateResult.DataEntity data) {
        if (data != null) {
            showTwoBtnDialog("更新", "v" + data.getVersionName() + "\n" + data.getUpdateInfo(), false, new IBaseActivity.OnTwoBtnClick() {
                @Override
                public void onOk() {
                    final TextView[] tv = new TextView[1];
                    final ProgressBar[] pb = new ProgressBar[1];
                    showUpdateDialog("更新", "即将开始下载...", false, new IBaseActivity.OnOneBtnClickListener() {
                        @Override
                        public void onProgress(TextView textView, ProgressBar progressBar) {
                            tv[0] = textView;
                            pb[0] = progressBar;
                        }
  
                        @Override
                        public void onOK() {
                        }
                    });
                    String url = Constants.API + File.separator + data.getUrl();
                    final String name = "HighNetFix_" + data.getVersionName() + "_" + data.getVersionCode() + ".apk";
                    OkGo.<File>get(url)
                            .tag(this)
                            .execute(new FileCallback(Constants.APK_DOWNLOAD_PATH, name) {
                                @Override
                                public void onSuccess(Response<File> response) {
                                    hideUpdateDialog();
                                    ApkUtils.installApk(HomePageActivity.this, BuildConfig.APPLICATION_ID, Constants.APK_DOWNLOAD_PATH, name);
                                }
  
                                @Override
                                public void onError(Response<File> response) {
                                    hideUpdateDialog();
                                    ToastUtils.showCustomBgToast("下载失败，请检查您的网络设置。");
                                }
  
                                @Override
                                public void downloadProgress(Progress progress) {
                                    long currentSize = progress.currentSize;
                                    long totalSize = progress.totalSize;
                                    int pro = (int) (currentSize * 100.0 / totalSize + 0.5f);
                                    tv[0].setText("已下载：" + pro + "%");
                                    pb[0].setProgress(pro);
                                }
                            });
                }
  
                @Override
                public void onCancel() {
                }
            });
        }
    }
```
