#### 2019-1-1更新日志
安卓使用的SDK_15.5.5 alipaySdk-20180601.jar
IOS使用的SDK_15.5.9 

#### 2018-11-28更新日志
最近总有人问在iOS上没有回调的问题，真机实测不存在该问题。

###### 第一步：在xcode中检查project的Bundle Identifier是否与其他项目重名
###### 第二步：在xcode中检查project的URL Types上alipay的URL Schemes格式是否正确
xcode的URL Types上alipay的URL Schemes正确格式应为ali2xxxxxxxxxxxxxxx。2开头的这串数字是你的APP_ID，英文字母与数字之间没有任何符号！！！
###### 第三步：检查调用插件写法是否正确


### 安装

cordova plugin add cordova-plugin-jb-alipay --variable APP_ID=[your AppId]

### 使用 API

    // ionic3上使用时需早import结束后添加 declare let cordova;
    
    // 第一步：订单在服务端签名生成订单信息，具体请参考官网进行签名处理 https://docs.open.alipay.com/204/105465/
    
    var payInfo = "app_id=2015052600090779&biz_content=%7B%22timeout_express%22%3A%2230m%22%2C%22product_code%22%3A%22QUICK_MSECURITY_PAY%22%2C%22total_amount%22%3A%220.01%22%2C%22subject%22%3A%221%22%2C%22body%22%3A%22%E6%88%91%E6%98%AF%E6%B5%8B%E8%AF%95%E6%95%B0%E6%8D%AE%22%2C%22out_trade_no%22%3A%22IQJZSRC1YMQB5HU%22%7D&charset=utf-8&format=json&method=alipay.trade.app.pay&notify_url=http%3A%2F%2Fdomain.merchant.com%2Fpayment_notify&sign_type=RSA2&timestamp=2016-08-25%2020%3A26%3A31&version=1.0&sign=cYmuUnKi5QdBsoZEAbMXVMmRWjsuUj%2By48A2DvWAVVBuYkiBj13CFDHu2vZQvmOfkjE0YqCUQE04kqm9Xg3tIX8tPeIGIFtsIyp%2FM45w1ZsDOiduBbduGfRo1XRsvAyVAv2hCrBLLrDI5Vi7uZZ77Lo5J0PpUUWwyQGt0M4cj8g%3D";

    // 第二步：调用支付插件  
    // ionic1上的写法
    cordova.plugins.alipay.payment(payInfo,function success(e){
      // 支付成功

    },function error(error){
      // 支付失败
      console.log("支付失败" + error.resultStatus);
    });

    // ionic3上的写法
    cordova.plugins.alipay.payment(payInfo, (res) => {
      // 支付成功
      
    }, (error) => {
      // 支付失败
      console.log("支付失败" + error.resultStatus);
    });

     //e.resultStatus  状态代码  e.result  本次操作返回的结果数据 e.memo 提示信息
     //e.resultStatus  9000  订单支付成功 ;8000 正在处理中  调用function success
     //e.resultStatus  4000  订单支付失败 ;6001  用户中途取消 ;6002 网络连接出错  调用function error
     //当e.resultStatus为9000时，请去服务端验证支付结果
                /**
                 * 同步返回的结果必须放置到服务端进行验证（验证的规则请看https://doc.open.alipay.com/doc2/
                 * detail.htm?spm=0.0.0.0.xdvAU6&treeId=59&articleId=103665&
                 * docType=1) 建议商户依赖异步通知
                 */
                
                
#### TIPS
##### 1. iOS上支付成功之后无法回调
xcode的URL Types上alipay的URL Schemes正确格式应为ali2xxxxxxxxxxxxxxx。2开头的这串数字是你的APP_ID，英文字母与数字之间没有任何符号！！！

##### 2. 沙箱环境
在我个人的开发过程中确实是没有使用到沙箱环境，都是直接真实支付1分钱来做测试。
如要使用沙箱环境，请自行参考官方文档https://docs.open.alipay.com/200/105311/