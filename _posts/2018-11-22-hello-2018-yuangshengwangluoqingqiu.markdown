---
layout:     post
title:      "Android原生的网络请求方法"
subtitle:   " \"Android自带的URL\""
date:       2018-11-22 09:25:00
author:     "Xiao"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Android
---

```

//get无参数
 public void login() {
        try {
            URL url = new URL("xxxxxxxxxxxxxxxxxxx");
            URLConnection connection = url.openConnection();
            connection.connect();
            InputStreamReader isr = new InputStreamReader(connection.getInputStream());
            BufferedReader br = new BufferedReader(isr);
            String str = "";
            StringBuilder sb = new StringBuilder();
            while ((str = br.readLine()) != null) {
                sb.append(str);
            }
            str_log = sb.toString() + "";
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    String str_url = "xxxxxxxxxxxxxxxxxxx";

    //post带参数

    public void postParameter() {
        try {
            //str_url：后台地址 Login：方法名称 （此处方法名称可根据实际情况改变）
            URL url = new URL(str_url + "Login");
            URLConnection connection = url.openConnection();
            connection.setDoInput(true);
            connection.setDoOutput(true);
            //输出流-提交数据
            DataOutputStream dos = new DataOutputStream(connection.getOutputStream());
            StringBuilder sb = new StringBuilder();
            Map<String, String> map = new HashMap<>();
            map.put("参数1", "xxx");
            map.put("参数2", "xxx");
            map.put("参数3", "xxx");
            。 。 。 。 。
            。 。 。 。 。
            。 。 。 。 。
            for (String key : map.keySet()) {
                sb.append(key + "=" + URLEncoder.encode(map.get(key), "UTF-8") + "&");
            }
            dos.writeBytes(sb.toString());
            dos.flush();
            dos.close();
            //输入流-获取数据
            BufferedReader br = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            StringBuilder sbt = new StringBuilder();
            String line;
            while ((line = br.readLine()) != null) {
                sbt.append(line);
            }
            Log.e("TAG", sbt.toString() + "");
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```
注：使用以上两个方法需要开辟新线程，不可在主线程直接调用。


