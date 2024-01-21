---
title: Python + Selenium Web自动化
date: 2024-01-14 00:48:41
tags: 编程
---

## 1. 原理

Selenium 是一套 Web网站 的程序自动化操作 解决方案。

通过它，我们可以写出自动化程序，像人一样在浏览器里操作web界面。 比如点击界面按钮，在文本框中输入文字 等操作。而且还能从web界面获取信息。 比如获取 火车、汽车票务信息，招聘网站职位信息，财经网站股票价格信息 等等，然后用程序进行分析处理。

Selenium 的自动化原理如下图所示。

![image](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/tut_20240113094425_19.png)

从上图可以看出：我们写的自动化程序 需要使用 **客户端库**。我们程序的自动化请求都是通过这个库里面的编程接口发送给浏览器。比如，我们要模拟用户点击界面按钮， 自动化程序里面就应该 调用客户端库相应的函数， 就会发送 **点击元素** 的请求给 下方的 **浏览器驱动**。 然后，浏览器驱动再转发这个请求给浏览器。这个自动化程序发送给浏览器驱动的请求 是HTTP请求。客户端库从哪里来的？ 是Selenium组织提供的。Selenium组织提供了多种 编程语言的Selenium客户端库， 包括 java，python，js， ruby等，方便不同编程语言的开发者使用。我们只需要安装好客户端库，调用这些库，就可以发出自动化请求给浏览器咯。**浏览器驱动** 也是一个独立的程序，是由浏览器厂商提供的， 不同的浏览器需要不同的浏览器驱动。 比如 Chrome浏览器和 火狐浏览器有 各自不同的驱动程序。

浏览器驱动接收到我们的自动化程序发送的界面操作请求后，会转发请求给浏览器， 让浏览器去执行对应的自动化操作。浏览器执行完操作后，会将自动化的**结果**返回给浏览器驱动， 浏览器驱动再通过HTTP响应的消息返回给我们的自动化程序的客户端库。自动化程序的客户端库 接收到响应后，将结果转化为 `数据对象` 返回给 我们的代码。我们的程序就可以知道这次自动化操作的结果如何了。

## 2. 安装

Selenium环境的安装主要就是安装两样东西： `客户端库` 和 `浏览器 驱动` 。

### 2.1 安装客户端库

不同的编程语言选择不同的Selenium客户端库。对应我们Python语言来说，Selenium客户端库的安装非常简单，用 pip 命令即可。

```
pip install selenium
```

如果安装不了，可能是网络问题，可以指定使用国内的豆瓣源

```
pip install selenium -i https://pypi.douban.com/simple/
```

### 2.2 安装Chrome浏览器驱动

Chrome浏览器安装好以后，接下来就是安装浏览器驱动 `Chrome Driver`。链接：[Chrome driver 新版本下载](https://googlechromelabs.github.io/chrome-for-testing/)。Stable表示浏览器对应的最新版本。

![image-20240114005829361](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240114005829361.png)

这是个zip包，下载下来之后，解压里面的程序文件 chromedriver.exe 到某个目录下面，注意这个目录的路径最好是没有中文名和空格的。比如，解压到 `d:\tools` 目录下面。然后把该目录添加到环境变量Path 里。

![image-20240114010725908](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240114010725908.png)

## 3. 简单示例

```python
import time
from selenium import webdriver
from selenium.webdriver.common.by import By

# 创建 WebDriver 对象，指明使用chrome浏览器驱动
driver = webdriver.Chrome()		
# 获取xx网站
driver.get("https://mail.163.com/")		
# 延时3秒等待网站加载完毕
time.sleep(3)							
# 将当前浏览器的控制权切换到第一个iframe 
driver.switch_to.frame(0)   			
driver.find_element(By.XPATH,'//input[@name="email"]').clear() 
# 输入框输入“xx”
driver.find_element(By.XPATH,'//input[@name="email"]').send_keys('xx')	
driver.find_element(By.XPATH,'//input[@name="password"]').clear()
# 输入框输入“yy”
driver.find_element(By.XPATH,'//input[@name="password"]').send_keys('yy')
# 点击动作
driver.find_element(By.XPATH,'//a[@id="dologin"]').click()	
# 等待停到此处，防止浏览器执行完自动关闭
input()
```

