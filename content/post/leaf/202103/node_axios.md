---
title: "Node-Koa2之axios请求云之讯短信发送接口和soap请求WebService"
date: 2021-03-10T11:24:18+08:00
draft: false
toc: true
url: "/leaf/202103/axios-soap.html"
categories: 
- 技术
tags: 
- api
- axios
- soap
- node.js
- koa2
---
# axios请求云之讯短信接口实现短信发送  
## 安装axios  
```shell
    cnpm install axios --save
```
## 方法实现  
```javascript
static async sendMessage(ctx){
    let param = Math.random().toString().substr(2, 4);  //生成四位随机数
    let phone = '157********';  
    const url = 'https://open.ucpaas.com/ol/sms/sendsms';
    const axios = require('axios');
    let sendRe = await axios({
        method: 'post',
        url: url,
        data: {
                sid: "4rtw******", 
                token: "5fjg******", 
                appid: "8aj5******", 
                templateid: '2780*', 
                param: param, 
                mobile: phone, 
                //uid: config.uid 
        },
        headers: {
            "Content-Type": "application/json;charset=utf-8",
            "Accept": "application/json"
        }
    });
    if(sendRe.data.code == '000000'){
        ctx.json({
            code:11,
            message:"发送成功"
        });
    }else{
        ctx.json({
            code:22,
            message:"短信验证码发送失败，"+sendRe.data.code
        })
    }
}
```
# soap请求WebService服务器数据
## 安装soap  
```shell
    cnpm install soap --save
```
## 发起soap请求  
```javascript
async function GetProjectInfo () {
    var url = 'http://digitcode.yesno.com.cn/CCNOutService/OutDigitCodeService.asmx?wsdl';
    var args = { userID :'b6cf5d3*******1b3e',
      userPwd:'50e3*******066',
      ip:'101.*****',
      acCode:"20210310****",
      language:'1',
      channel:'X'
      };
    return new Promise(function (results) {
        const soap = require('soap');
        soap.createClient(url, function(err, client) {
          client.Get_AcCodeInfoInterface(args, function(err, result) {
            if (err) {
              results(err)
            }else {
              results(result)
            }  
          });
        });
      });
}
module.exports = {
 GetProjectInfo
};
```
## 获取到WebService返回的Xml数据  
![](/images/leaf/202103/node_soap.png)  
```javascript
let resultProductInfo = await GetProjectInfo();
let xmlReply = resultProductInfo.reply;
```
## 解析Xml数据   
### 安装xmlreader  
```
    cnpm install xmlreader --save
```
### xml数据转json对象实现  
```javascript
async function XmlToJson (xmlString) {
    return new Promise(function (result){
        var xmlreader = require("xmlreader");
        xmlreader.read(xmlString, function(errors, response){
            if(null !== errors ){
                result(errors);
            }
        let datas={};
        datas.reply = response.messageinfo.item.reply.text();
        datas.p_code = response.messageinfo.item.zcode.text();
        datas.cuanhuocode = response.messageinfo.item.cuanhuocode.text();
        datas.productcode = response.messageinfo.item.productcode.text();
        result(datas);
      });
    });
}
 
module.exports = {
    XmlToJson
};
```
___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

