---
title: "PHP&kuaidi.com实现快递查询"
date: 2021-01-26T16:14:21+08:00
draft: false
toc: true
url: "/leaf/202101/kuaidiapi.html"
categories: 
- 技术
tags: 
- 快递API
- PHP
- 快递接口
---
# 申请快递查询API授权key  
## [kuaidi.com](http://kuaidi.com)  
# SDK文件KDApi.php  
```
/**
 * 物流信息查询接口SDK
 */
class KDAPI{
    private $_APPKEY = 'twde5d8ef***********';   //改为您自己的key
    private $_APIURL = "https://highapi.kuaidi.com/openapi-querycountordernumber.html?";    
    private $_show = 0;
    private $_muti = 0;
    private $_order = 'desc';

    /**
     * 获得的快递网接口查询KEY。
     * @param string $key
     */

    public function KuaidiAPi($key){

        $this->_APPKEY = $key;
    }

    /**
     * 设置数据返回类型。0: 返回 json 字符串; 1:返回 xml 对象
     * @param number $show
     */

    public function setShow($show = 0){

        $this->_show = $show;
    }


    /**
     * 设置返回物流信息条目数, 0:返回多行完整的信息; 1:只返回一行信息
     * @param number $muti
     */

    public function setMuti($muti = 0){

        $this->_muti = $muti;
    }

    /**
     * 设置返回物流信息排序。desc:按时间由新到旧排列; asc:按时间由旧到新排列
     * @param string $order
     */

    public function setOrder($order = 'desc'){

        $this->_order = $order;
    }

    /**
     * 查询物流信息，传入单号，
     * @param 物流单号 $nu
     * @param 公司简码 $com 要查询的快递公司代码,不支持中文,具体请参考快递公司代码文档。 不填默认根据单号自动匹配公司。注:单号匹配成功率高于 95%。
     * @throws Exception
     * @return array
     */

    public function query($nu, $com=''){

        if (function_exists('curl_init') == 1) {

            $url = $this->_APIURL;
            $dataArr = array(
                'id' => $this->_APPKEY,
                'com' => $com,
                'nu' => $nu,
                'show' => $this->_show,
                'muti' => $this->_muti,
                'order' => $this->_order
            );


            foreach ($dataArr as $key => $value) {

                $url .= $key . '=' . $value . "&";
            }

            // echo $url;
            $curl = curl_init();
            curl_setopt($curl, CURLOPT_URL, $url);
            curl_setopt($curl, CURLOPT_HEADER, 0);
            curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
            curl_setopt($curl, CURLOPT_TIMEOUT, 10);
            $kuaidresult = curl_exec($curl);
            curl_close($curl);


            if($this->_show == 0){

                $result = json_decode($kuaidresult, true);
            }else{

                $result = $kuaidresult;
            }

            return $result;

        }else{

            throw new Exception("Please install curl plugin", 1); 
        }
    }

}

```
# 使用示例example.php
```
include 'KDApi.php';
$kuaidichaxun = new KDAPI();
//设置返回格式。 0: 返回 json 字符串; 1:返回 xml 对象
//$kuaidichaxun->setShow(1); //可选，默认为 0 返回json格式
//返回物流信息条目数。 0:返回多行完整的信息; 1:只返回一行信息
//$kuaidichaxun->setMuti(1); //可选，默认为0
//设置返回物流信息排序。desc:按时间由新到旧排列; asc:按时间由旧到新排列
//$kuaidichaxun->setOrder('asc');
//查询
$result = $kuaidichaxun->query('YT21212*****', 'yuantong');

var_dump($result);
```
# 运行结果  
![](/images/leaf/202101/express01291003.png)  




___
> 喜欢这篇文章的话 打赏一下吧！ 

| ![Wechat](/images/pay/eb05acdaec967.png)  | ![Alipay](/images/pay/0831de845.png) |
| --------   | -----:  |

