---
title: Python-Selenium-Web自动化自学第二天
date: 2024-01-16 09:29:13
tags: 编程
---

## 2. 选择元素基本方法

### 2.1 选择元素的方法

要想定位元素，就是先告诉浏览器，你要操作哪个界面元素， 让它找到你要操作的界面元素。方法就是：告诉浏览器，你要操作的这个 web 元素的 `特征` 。元素的特征怎么查看？请大家用chrome浏览器访问百度，按F12后，点击下图箭头处的 `Elements` 标签（中文名叫`元素`），即可查看页面对应的HTML 元素。

![image-20240116093434340](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240116093434340.png)

### 2.2 根据 id属性 选择元素

页面上有个输入股票名称的输入框，使用鼠标右键菜单 查看该 input元素，会发现它有一个属性叫id。

![image-20240116093633641](https://cdn.jsdelivr.net/gh/helloliyilin/picgoimg//img/image-20240116093633641.png)

我们可以把 id 想象成元素的编号， 是用来在html中标记该元素的。根据规范， 如果元素有id属性 ，这个id 必须是当前html中唯一的。

```python
import time
from selenium import webdriver
from selenium.webdriver.common.by import By

# 创建 WebDriver 对象，指明使用chrome浏览器驱动
driver = webdriver.Chrome()		
# 获取xx网站
driver.get("https://www.baidu.com/")		
# 延时3秒等待网站加载完毕
time.sleep(3)
# 根据id选择元素然后对页面元素进行操作了
driver.find_element(By.ID,'kw').send_keys('百度一下')
# 等待停到此处，防止浏览器执行完自动关闭
input()
```

其中`driver = webdriver.Chrome()`，driver赋值的是 WebDriver 类型的对象，我们可以通过这个对象来操控浏览器，比如打开网址、选择界面元素等。

使用了 WebDriver 对象的方法 `find_element` ，这行代码运行是，就会发起一个请求通过浏览器驱动转发给浏览器，告诉它，需要选择一个id为 kw 的元素。浏览器找到id为kw的元素后，将结果通过浏览器驱动返回给自动化程序，所以find_element方法会返回一个 **WebElement 类型的对象**。我们通过这个WebElement对象，就可以 `操控` 对应的界面元素。比如调用这个对象的 send_keys 方法就可以在对应的元素中 输入字符串，调用这个对象的 click 方法就可以 **点击** 该元素。如果根据 传入的ID，找不到这样的元素，find_element 方法就会抛出`selenium.common.exceptions.NoSuchElementException` 异常。

>  这里我直接用了`driver.find_element(By.ID,'kw')`代表了 **WebElement 类型的对象**，你也可以可以定义一个`element`的变量。

### 2.3 根据 class属性、tag名 选择元素

例如有个网址对应的html内容 有如下的部分

```html
    <body>
        
        <div class="plant"><span>土豆</span></div>
        <div class="plant"><span>洋葱</span></div>
        <div class="plant"><span>白菜</span></div>

        <div class="animal"><span>狮子</span></div>
        <div class="animal"><span>老虎</span></div>
        <div class="animal"><span>山羊</span></div>

    </body>
```

我们用class属性定位元素

```
driver.find_elements(By.CLASS_NAME, 'animal')
```

> 注意element后面多了个s，find_elements 返回的是找到的符合条件的 `所有` 元素 (这里有3个元素)， 放在一个 `列表` 中返回。而如果我们使用 `wd.find_element` (注意少了一个s) 方法， 就只会返回 `第一个` 元素。

### 2.4 根据 tag 名 选择元素

类似的，我们可以通过指定 参数为 `By.TAG_NAME` ，选择所有的tag名为 div的元素，如下所示

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()

driver.get('https://cdn2.byhy.net/files/selenium/sample1.html')

# 根据 tag name 选择元素，返回的是 一个列表
# 里面 都是 tag 名为 div 的元素对应的 WebElement对象
elements = wd.find_elements(By.TAG_NAME, 'div')

# 取出列表中的每个 WebElement对象，打印出其text属性的值
# text属性就是该 WebElement对象对应的元素在网页中的文本内容
for element in elements:
    print(element.text)
```

