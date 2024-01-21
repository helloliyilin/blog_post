---
title: har和pcapng文件签名值验签
date: 2024-01-12 09:55:42
tags: 密码
categories: 密评
---

## 1. har文件验签

HAR文件，全称HTTP Archive，是一种专门用于储存HTTP请求和响应信息的通用文件格式，这种格式基于JSON。HAR文件可以捕获并记录所有HTTP请求和响应的详细信息，包括请求URL、请求方法、请求头、请求体、响应状态码、响应头和响应体等。

### 1.1. har文件放浏览器解析

在浏览器地址栏输入：about:blank，然后右键`检查`，点击`网络`按钮，把har文件加载到浏览器进行解析。



![image-20240112101129361](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112101129361.png)

找到其中一个文件。“负载”中的**strValue**的值表示随机数，“预览”中的**value**的值表示公钥和签名值的解析值。

![image-20240112101619668](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112101619668.png)

或者有可能遇到的随机数**random_id**和解析值**encrypted_data**，如下图所示。

![image-20240112123849781](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112123849781.png)

### 1.2解析值提取公钥和签名值

value的解释值需要在网站[ASN.1 JavaScript decoder](https://lapo.it/asn1js/)进行解析。打开网站默认界面如下图，把解析值粘贴进输入框，点击“decode”按钮。

![image-20240112103456341](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112103456341.png)

公钥值

![image-20240112104154046](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112104154046.png)

签名值

![image-20240112104409394](https://gitee.com/helloliyilin/picgoimg/raw/master/img/image-20240112104409394.png)

### 1.3 签名验证

根据ans.1的编码规则需要把前2个字节删除，第3个字节如果是00也要删除。第1个字节表示数据类型，第2个字节表示后面的字节数，第3个字节如果值为00表示填充值。把处理后的随机数、公钥值和签名值放到网站进行解析。

![image-20240112110209087](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112110209087.png)

## 2. pcapng文件验签

### 2.1 wireshare解析文件

过滤http，找到POST。

![image-20240112110835166](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112110835166.png)

右键追踪流-HTTP流

![image-20240112110928734](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112110928734.png)

### 2.2 在HTTP流提取随机数、证书和签名值

找到随机数==random==。

![image-20240112111300527](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112111300527.png)

找到登录的证书`loginCert`和登录的签名值`loginSignedData`。

![image-20240112111535308](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112111535308.png)

### 2.3 证书提取公钥

新建txt文件，把证书值复制到文件，重命名xx.cer，打开文件把公钥值提取出来，记得把空格删除。

![image-20240112112000870](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112112000870.png)

### 2.4 签名验签

签名值是以base64编码的，需要转换为hex形式。

![image-20240112112537265](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240112112537265.png)

随机数、公钥和签名值解析验签。

![image-20240112112723962](https://gitee.com/helloliyilin/picgoimg/raw/master/img/image-20240112112723962.png)