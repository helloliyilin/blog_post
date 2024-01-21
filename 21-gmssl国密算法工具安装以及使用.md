---
title: gmssl国密算法工具安装以及使用
date: 2024-01-10 11:55:52
tags: 密码
categories: 密评
---

## 1. gmssl介绍

> GmSSL是一个开源的密码工具箱，支持SM2/SM3/SM4/SM9/ZUC等国密(国家商用密码)算法、SM2国密数字证书及基于SM2证书的SSL/TLS安全通信协议，支持国密硬件密码设备，提供符合国密规范的编程接口与命令行工具，可以用于构建PKI/CA、安全通信、数据加密等符合国密标准的安全应用。GmSSL项目是OpenSSL项目的分支，并与OpenSSL保持接口兼容。

参考链接：[gmssl-github链接](https://github.com/guanzhi/GmSSL)

## 2. gmssl安装

GmSSL 3 采用了cmake构建系统。下载源代码后将其解压缩，进入源码目录，执行：

```
mkdir build
cd build
cmake ..
make
make test
sudo make install
```

在`make install`完成后，GmSSL会在默认安装目录中安装`gmssl`命令行工具，在头文件目录中创建`gmssl`目录，并且在库目录中安装`libgmssl.a`、`libgmssl.so`等库文件。

测试版本gmssl是否安装成功

```
gmssl version
```

## 3. SM2

### 3.1 生成SM2公私钥对

```
gmssl sm2keygen -pass 1234 -out sm2.pem -pubout sm2pub.pem
```

参数：

- pass 生成的SM2私钥的加密口令；
- out 生成的SM2私钥；
- pubout 生成的SM2公钥。

### 3.2 使用SM2私钥对文件进行签名

```
gmssl sm2sign -key sm2.pem -pass 1234 -in data.txt -out sm2.sig
```

参数：

- key SM2私钥
- pass SM2私钥的加密口令
- id 指定签名使用的的ID（可选项，默认为1234567812345678）
- in 待签名文件
- out SM2签名结果

### 3.3 使用公钥或者数字证书，对数据的SM2签名值验签

```
gmssl sm2verify -pubkey sm2pub.pem -in data.txt -sig sm2.sig
```

参数：

- pubkey 公钥
- cert 数字证书
- id 签名值使用的ID
- in 待验证的原始数据
- sig 待验证的签名值

### 3.4 使用SM2公钥或者数字证书对数据进行加密

```
gmssl sm2encrypt -pubkey sm2pub.pem -in data.txt -out sm2.der
```

参数：

- pubkey 公钥
- cert 数字证书（如果使用数字证书进行数据加密时使用此参数）
- in 待加密数据
- out SM2加密结果

### 3.5 使用SM2私钥对加密数据进行解密

```
gmssl sm2decrypt -key sm2.pem -pass 1234 -in sm2.der
```

参数：

- key SM2私钥
- pass SM2私钥口令
- in 待解密数据
- out 解密结果

## 4. SM3

### 4.1 计算数据的SM3哈希值

```
echo -n abc | gmssl sm3		//计算‘abc’字符串的sm3哈希
```

若要对文件进行sm3哈希，可以用下面的命令。

```
gmssl sm3 -in 输入文件路径 -out 输出文件路径
```

### 4.2 使用SM3算法和私钥计算数据的HMAC值

```
echo -n abc | gmssl sm3hmac -key 11223344556677881122334455667788
```

若要对文件进行sm3hmac哈希，可以用下面的命令。

```
gmssl sm3hmac -key 11223344556677881122334455667788 -in 输入文件路径 -out 输出文件路径
```



可以学习了一下什么是HMAC值，参考链接如下：[hmac_百度百科 (baidu.com)](https://baike.baidu.com/item/hmac/7307543)

> HMAC是密钥相关的哈希运算消息认证码（Hash-based Message Authentication Code）的缩写，由H.Krawezyk，M.Bellare，R.Canetti于1996年提出的一种基于Hash函数和密钥进行消息认证的方法，并于1997年作为RFC2104被公布，并在IPSec和其他网络协议（如SSL）中得以广泛应用，现在已经成为事实上的Internet安全标准。它可以与任何迭代散列函数捆绑使用。

## 5. SM4

参数：

- key SM4加密解密使用的长度为128bit的key，使用16进制表示
- iv SM4加密使用的IV
- encrypt 进行加密
- decrypt 进行解密
- cbc 使用CBC模式
- ctr 使用CTR模式
- in 待加密/解密数据
- out 加密/解密结果

首先定义key与iv

```
KEY=11223344556677881122334455667788
IV=11223344556677881122334455667788
```

### 5.1 使用SM4算法进行加密和解密——使用CBC模式

```
echo 20201307lcy | gmssl sm4 -cbc -encrypt -key $KEY -iv $IV -out sm4.cbc
gmssl sm4 -cbc -decrypt -key $KEY -iv $IV -in sm4.cbc
```

### 5.2 使用SM4算法进行加密和解密——使用CTR模式

```
echo 20201307lcy | gmssl sm4 -ctr -encrypt -key $KEY -iv $IV -out sm4.ctr
gmssl sm4 -ctr -decrypt -key $KEY -iv $IV -in sm4.ctr
```

## 6. SM9

### 6.1 生成SM9算法的主公钥和主私钥

```
gmssl sm9setup -alg sm9sign -pass 1234 -out sign_msk.pem -pubout sign_mpk.pem
```

### 6.2 通过主私钥生成用户的SM9密钥

```
gmssl sm9keygen -alg sm9sign -in sign_msk.pem -inpass 1234 -id alice -out alice.pem -outpass 1234
```

### 6.3 通过用户SM9私钥对数据进行签名

```
gmssl sm9sign -key alice.pem -pass 1234 -in data.txt  -out hello.sig
```

### 6.4 通过SM9主公钥及用户id对签名值进行验签

```
gmssl sm9verify -pubmaster sign_mpk.pem -id alice -in data.txt -sig hello.sig
```

