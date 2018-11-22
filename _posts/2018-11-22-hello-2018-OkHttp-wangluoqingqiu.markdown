---
layout:     post
title:      "Android OkHttp"
subtitle:   " \"Android-OkHttp的入门使用\""
date:       2018-11-22 09:27:00
author:     "Xiao"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Android
    - OkHttp
---
##代码

```
    public void getOkhttp() {
        OkHttpClient client = new OkHttpClient();
        Request request = new Request.Builder()
                .url(str_log)//str_log：服务器地址
                .build();
        try {
            Response response = client.newCall(request).execute();//同步,不可在主线程调用
            str = response.body().string();//str：空字符串
            Toast.makeText(this, str + "", Toast.LENGTH_SHORT).show();//吐司
            Log.e("TAG" , str + "") ;//日志
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```

```
 public void postOkhttp(){
		public static final MediaType JSON = MediaType.parse("application/json; charset=utf-8") ;
        OkHttpClient client = new OkHttpClient() ;
        //str_json：参数 ex：{"username":"xxx","password":"xxx"}。。。。
        RequestBody body = RequestBody.create(JSON , str_json) ;
        Request request = new Request.Builder()
                .url(str_url + "方法名")//str_url:服务器地址 
                .post(body)
                .build();
        try {
            Response response = client.newCall(request).execute() ;//同步，不可主线程调用
            Log.e("TAG" , response.body().string() + "") ;
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
```


