---
title: "PHP-微信公众号实现发送模板消息"
date: 2020-12-15T11:19:55+08:00
draft: false
toc: true
url: "/leaf/2020/12/15/wechat01"
categories: 
- 杂记
tags: 
- 微信开发
- 模板消息
- PHP
---
## PHP微信公众号实现发送模板消息  
```
    $appid = "wxb7729*******";
    $appsecret = "8b99**************";
    $url = "https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=$appid&secret=$appsecret";
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
    curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, FALSE);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    $output = curl_exec($ch);
    curl_close($ch);
    $jsoninfo = json_decode($output, true);	
    $ACCESS_TOKEN = $jsoninfo["access_token"];
    $data=array( 
    'touser'=>"oaIgKt*********", //要发送给用户的openid
    'template_id'=>"gSxmffQ*********",//改成自己的模板id，在微信后台模板消息里查看
    'url'=>"https://listenlive.cn", //用户点击要跳转的url
    'data'=>array(
      'first'=>array(
        'value'=>"亲爱的同学，您有直播提醒，请查阅。",
        'color'=>"#000"
        ),
      'keyword1'=>array(
        'value'=>"教授直播",
        'color'=>"#f00"
        ), 
      'keyword2'=>array(
        'value'=>"教授",
        'color'=>"#173177"
        ), 
      'keyword3'=>array(
        'value'=>"2020-03-25 19:00",
        'color'=>"#3d3d3d"
        ),
     
      'remark'=>array(
        'value'=>"戳进来可以查看详情>>>",
        'color'=>"#3d3d3d"
        ),
    )
    );
    
    $json_data=json_encode($data);//转化成json数组让微信可以接收
    $url="https://api.weixin.qq.com/cgi-bin/message/template/send?access_token=".$ACCESS_TOKEN;//模板消息请求URL
    
    $res=https_request($url,urldecode($json_data));//请求开始
    $res=json_decode($res,true);
    
    if($res['errcode']==0 && $res['errcode']=="ok"){
    echo "发送成功！";
    }
 
    //curl请求函数
    function https_request($url,$data = null){
    $curl = curl_init();
    curl_setopt($curl, CURLOPT_URL, $url);
    curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, FALSE);
    curl_setopt($curl, CURLOPT_SSL_VERIFYHOST, FALSE);
    if (!empty($data)){
    curl_setopt($curl, CURLOPT_POST, 1);
    curl_setopt($curl, CURLOPT_POSTFIELDS, $data);
    }
    curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
    $output = curl_exec($curl);
    curl_close($curl);
    return $output;
    }
```
## 发送成功  
![](/images/leaf/202012/202012151141.jpg)
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |