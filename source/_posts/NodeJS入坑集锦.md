---
title: NodeJS入坑集锦
date: 2017-06-06 18:25:45
tags: 【NodeJS, 微信, 公众号, 支付】
description: 记录本人在进行微信APP支付&&微信公众号支付，NodeJS后台实现过程中遇到的问题
---
## 准备工作 
---------
需要参数：
token: test, // 微信配置中的token 

appId: wx7e46b******** // 微信开放平台配置里的appid 

appSecret: f47171232cfc69*******11e88a3 // 微信开放平台配置里的appsecret 

machId: 1449239*** // 微信商户平台的商户ID 

machSecret: 1ZAqhmo5LzbhdwAU4********* // 微信商户平台的api秘钥

notifyUrl: https://example.com // 支付成功回调接口路径

开发步骤
商户系统和微信支付系统主要交互说明：

步骤1：用户在商户APP中选择商品，提交订单，选择微信支付。

步骤2：商户后台收到用户支付单，调用微信支付统一下单接口。参见[统一下单API](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=9_1)。

步骤3：统一下单接口返回正常的prepay_id，再按签名规范重新生成签名后，将数据传输给APP。参与签名的字段名为appid，partnerid，prepayid，noncestr，timestamp，package。注意：package的值格式为Sign=WXPay

步骤4：商户APP调起微信支付。api参见本章节[app端开发步骤说明](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=8_5)

步骤5：商户后台接收支付通知。api参见[支付结果通知API](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=8_5)

步骤6：商户后台查询支付结果。，api参见[查询订单API](https://pay.weixin.qq.com/wiki/doc/api/app/app.php?chapter=9_2)

## 微信公众号支付
------
> 微信公众号支付业务流程图
![业务流程图](https://pay.weixin.qq.com/wiki/doc/api/img/chapter7_4_1.png)
### 1.调用微信统一下单
统一下单接口URL地址：https://api.mch.weixin.qq.com/pay/unifiedorder


### 2.二次签名
调用微信统一下单接口请求成功返回如下：
<xml>
   <return_code><![CDATA[SUCCESS]]></return_code>
   <return_msg><![CDATA[OK]]></return_msg>
   <appid><![CDATA[wx2421b1c4370ec43b]]></appid>
   <mch_id><![CDATA[10000100]]></mch_id>
   <nonce_str><![CDATA[IITRi8Iabbblz1Jc]]></nonce_str>
   <openid><![CDATA[oUpF8uMuAJO_M2pxb1Q9zNjWeS6o]]></openid>
   <sign><![CDATA[7921E432F65EB8ED0CE9755F0E86D72F]]></sign>
   <result_code><![CDATA[SUCCESS]]></result_code>
   <prepay_id><![CDATA[wx201411101639507cbf6ffd8b0779950874]]></prepay_id>
   <trade_type><![CDATA[JSAPI]]></trade_type>
</xml>