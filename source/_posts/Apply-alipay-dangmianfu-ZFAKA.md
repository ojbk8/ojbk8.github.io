---
title: "免费申请开通支付宝当面付基础版费率0.38%对接ZFAKA教程"
date: 2019-07-02T00:00:00+08:00
lastmod: 2019-07-02T00:00:00+08:00
draft: false
tags: ["alipay"]
---

# 申请开通支付宝当面付基础版费率0.38%

操作步骤

打开[蚂蚁金服开放](https://openhome.alipay.com/platform/home.htm) 登录 

打开：https://openhome.alipay.com/isv/isvMerchantManage.htm 点击【新增商户】

特别提醒：

- 输入类目自己看着，一般选零售，比如生活百货（不建议选择金融网络）
- 营业执照是非必填项，可以不上传
- 上传门头，可以利用搜索引擎或者大众点评
- 联系方式？（据说可以随便输入，建议输入真实的）
- 费率建议选择0.38

申请出错

有网友反馈无法申请，返回错误！可以试一试下面的地址申请！不过**费率是0.6**

https://b.alipay.com/signing/authorizedProductSet.htm


并按提示完成签约上线, 查看 [商家签约管理](https://openhome.alipay.com/isv/settling/queryMerchantSignProductList.htm)

![](/images/alipay-dangmianfu.png)



# 支付宝当面付对接ZFAKA

[RSA私钥及公钥生成](https://docs.open.alipay.com/58/103242)

生成方式一（推荐）：使用支付宝提供的一键生成工具（内附使用说明）

Windows：[下载](http://p.tb.cn/rmsportal_6680_secret_key_tools_RSA_win.zip)

MAC OSX：[下载](http://p.tb.cn/rmsportal_6680_secret_key_tools_RSA_macosx.zip)

解压打开文件夹，直接运行“RSA 签名验签工具.bat”（WINDOWS）或“RSA 签名验签工具.command”（MAC_OSX），点击“生成 RSA 密钥”，会自动生成公私钥，然后点击 `打开文件位置`，即可找到工具自动生成的密钥。
具体步骤请参考签名专区[生成RSA密钥指南](https://docs.open.alipay.com/291/105971)。

1. 把`RSA 签名验签工具`生成的`商户应用公钥`复制到网页&移动应用 [管理中心-蚂蚁金服开放平台](https://openhome.alipay.com/platform/appManage.htm#/apps)【应用信息】【加签方式】【应用公钥(SHA256withRsa)】【应用公钥】中
2. 把ZFAKA【支付宝当面付】上方的【本支付接口异步支付回调地址】

``` bash
https://xxx.xxx.xxx/product/notify/?paymethod=zfbf2f
```

复制到支付宝的【授权回调地址】中，其中`https://xxx.xxx.xxx`是自己的域名

3. 在支付宝【加签方式】【查看支付宝公钥】并把它复制到ZFAKA【后台管理】【设置中心】【支付设置】【支付宝当面付】【编辑】【ali_public_key】中

4. 把支付宝【APPID】复制到【ZFAKA】中

5. 把`RSA 签名验签工具`生成的`商户应用私钥`复制到【ZFAKA】应用密匙【rsa_private_key】中
