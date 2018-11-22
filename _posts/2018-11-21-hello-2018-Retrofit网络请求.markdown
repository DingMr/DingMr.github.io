---
layout:     post
title:      "Android Retrofit"
subtitle:   " \"Android Retrofit入门使用\""
date:       2018-11-22 09:29:00
author:     "Xiao"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Android
	- Retrofit
---
## 代码

```
public interface API {

    @GET("Login")
    Call<ResponseBody> getNews(@Query("username") String userName, @Query("password") String password);

    @FormUrlEncoded
    @POST("Login")
    Call<ResponseBody> postCheck(@Field("username") String userName, @Field("password") String password);
}
```

```
 public void getRetrofit() {
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(url)//url:服务器地址
                .build() ;
        API api = retrofit.create(API.class) ;
        Call<ResponseBody> newsCall = api.getNews("xxx" , "xxx" );//参数
        newsCall.enqueue(new Callback<ResponseBody>() {
            @Override
            public void onResponse(Call<ResponseBody> call, retrofit2.Response<ResponseBody> response) {
                try {
                    Log.e("TAG" , response.body().string() + "") ;
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            @Override
            public void onFailure(Call<ResponseBody> call, Throwable t) {
                Log.e("TAG" , t.toString() + "") ;
            }
        });
    }
```

```
 public void postRetrofit(){
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(url)
                .build();
        API api = retrofit.create(API.class) ;
        Call<ResponseBody> call = api.postCheck("xxx" , "xxx" ) ;//参数
        call.enqueue(new Callback<ResponseBody>() {
            @Override
            public void onResponse(Call<ResponseBody> call, retrofit2.Response<ResponseBody> response) {
                try {
                    Log.e("TAG" , response.body().string()+"");
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            @Override
            public void onFailure(Call<ResponseBody> call, Throwable t) {
                Toast.makeText(MainActivity.this, ""+t.toString(), Toast.LENGTH_SHORT).show();
            }
        });
    }
```
注：post请求时遇到的问题
## ①  ：
```
 @POST("Login")
 Call<ResponseBody> postCheck(@Query("username") String userName, @Query("password") String password);
```
当我用与GET相同的写法写POST请求时，出现以下错误：
![这里写图片描述](https://img-blog.csdn.net/20180803121219872?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![这里写图片描述](https://img-blog.csdn.net/20180803121236535?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
>先是在打印log日志的地方出现空指针，随后断点调试发现请求格式无效。原因未找到，如知晓，麻烦留言告诉我，谢谢！
## ②：
随后谷歌了一波，我更改了接口申明：

```
@FormUrlEncoded
@POST("Login")
Call<ResponseBody> postCheck(@Query("username") String userName, @Query("password") String password);
```
我忘记在那篇博客看到的，说这个 ***@FormUrlEncoded*** 注解必不可少（POST中）。结果如下：
![这里写图片描述](https://img-blog.csdn.net/20180803122243570?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![这里写图片描述](https://img-blog.csdn.net/20180803122251438?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
>调用API接口中的方法时直接崩溃，断点调试结果：找不到本地变量。原因未明，如知晓，麻烦留言告诉我，谢谢！
## ③：
根据log日志提示，最终更改为：

```
@FormUrlEncoded
@POST("Login")
Call<ResponseBody> postCheck(@Field("username") String userName, @Field("password") String password);
```
>编译通过。

![这里写图片描述](https://img-blog.csdn.net/20180803122915470?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTc2NzM4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
>这是从https://www.jianshu.com/p/0fda3132cf98博主那儿抄下来的，希望对你们有用！


