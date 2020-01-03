# Selenium

## 1.Selenium简介

Selenium是ThroughtWorks公司推出的强大的开源Web功能测试工具系列；支持多平台、多浏览器、多语言去实现自动化测试。支持多种开发语言：ruby,python,java,perl,c#等，同时Selenium测试直接自动运行在浏览器中，支持的浏览器包括IE,Chrome和FireFox等

## 2.简单脚本介绍

```python
from selenium import webdriver #导入模块
import time
brower = webdriver.Chrome() #将谷歌浏览器给brower变量
time.sleep(3)
brower.get("http://www.baidu.com")
time.sleep(3)
brower.find_element_by_id("kw").send_keys("selenium") #定位元素并赋值
time.sleep(3)
brower.find_element_by_id("su").click() #定位元素，点击
time.sleep(3)
brower.quit() #彻底退出浏览器
```

注：一个控件有若干属性id、name、xpath等，百度输入框的id为kw,搜索按钮的id为su

运行该脚本后，会自动使用Chrome浏览器打开百度首页搜索selenium后关闭浏览器

## 3.控件元素定位

注意：不管使用哪种方式进行元素定位，必须保证页面上该属性的唯一性

### 3.1webdriver提供给的对象定位方法

- id
- name
- class name
- link text
- partial link text
- tag name
- xpath
- css selector

### 3.2栗子-百度输入框的多种定位法

![1577948724803](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1577948724803.png)

```python
#coding=utf-8
from selenium import webdriver
import time
brower = webdriver.Chrome()
brower.get("http://www.baidu.com")
###百度输入框的多种定位方式###
#通过id定位
brower.find_element_by_id("kw").send_keys("selenium")
#通过name定位
brower.find_element_by_name("wd").send_keys("selenium")
#通过tag name定位
brower.find_element_by_tag_name("input").send_keys("selenium")
#通过class name定位
brower.find_element_by_class_name("s_ipt").send_keys("selenium")
#通过css方式定位
brower.find_element_by_css_selector("#kw").send_keys("selenium")
#通过xpath定位
brower.find_element_by_xpath("//*[@id='kw']").send_keys("selenium")
######
brower.find_element_by_id("su").click()
time.sleep(3)
brower.quit()
```

### 3.3多种定位方法

#### 3.3.1id和name定位

id和name是非常常见的定位方式，大多数控件都有这两个属性，而且对控件的id和name命名时一般会取不同的名字

还是以百度输入框为例：

```
#通过id定位
brower.find_element_by_id("kw").send_keys("selenium")
#通过name定位
brower.find_element_by_name("wd").send_keys("selenium")
```

#### 3.3.2tag name和class name定位

对于大多数控件，不单单只有id和name两个属性，比如还有class name 和 tag name

tag name是指标签名

```
#通过tag name定位
brower.find_element_by_tag_name("input").send_keys("selenium")
#通过class name定位
brower.find_element_by_class_name("s_ipt").send_keys("selenium")
```

#### 3.3.3CSS定位

对于一个控件的CSS获取可以使用Chrome的F12打开检查模式，抓取到输入框的标签（可参考下列图片）

![1577950439676](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1577950439676.png)

```
#通过css方式定位
brower.find_element_by_css_selector("#kw").send_keys("selenium")
```

3.3.4XPath定位

XPath是一种在XML文档中定位元素的语言。因为HTML可以看作是XML的一种实现，所以selenium用户可以使用这种方式在web应用中定位元素（具体获取可参考下列图片）

![1577950672947](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1577950672947.png)

```
#通过xpath定位
brower.find_element_by_xpath("//*[@id='kw']").send_keys("selenium")
```

#### 3.3.5link text定位

在不是输入框也不是按钮的情况下，可能是文字连接，则可以通过link

定位百度首页的新闻：

```python
#coding=utf-8
from selenium import webdriver
import time
brower = webdriver.Chrome()
brower.get("http://www.baidu.com")
brower.find_element_by_link_text("新闻").click()
time.sleep(3)
brower.quit()
```

3.3.6Partial link text定位

部分链接定位

定位百度首页的hao123：

```python
#coding=utf-8
from selenium import webdriver
import time
brower = webdriver.Chrome()
brower.get("http://www.baidu.com")
brower.find_element_by_link_text("hao").click()
time.sleep(3)
brower.quit()
```

## 4.操作测试对象

控件定位结束后，这就结束了？

其实不是的，定位只是第一步。定位之后需要对这个元素进行操作，鼠标点击还是键盘输入取决于我们定位的控件是按钮还是输入框

### 4.1鼠标点击与键盘输入

栗子：先在百度输入框里输入test，然后清空输入selenium后，点击搜索

```python
#coding=utf-8
from selenium import webdriver
import time
brower = webdriver.Chrome()
brower.get("http://www.baidu.com")
time.sleep(3)
brower.find_element_by_id("kw").send_keys("test")
time.sleep(2)
brower.find_element_by_id("kw").clear()
time.sleep(2)
brower.find_element_by_id("kw").send_keys("selenium")
brower.find_element_by_id("su").click()
time.sleep(3)
brower.quit()
```

### 4.2submit提交表单

与click()具有相同的效果

```python
brower.find_element_by_id("su").submit()
```

### 4.3text获取元素文本

栗子：获取百度主页“有事搜一搜，没事看一看”的控件元素文本

```python
#coding=utf-8
from selenium import webdriver
import time
brower = webdriver.Chrome()
brower.get("http://www.baidu.com")
time.sleep(3)
#class name:sub-title
data = brower.find_element_by_class_name("sub-title").text
print data #打印信息
time.sleep(3)
brower.quit()
```

控制台输出：

![1577965358228](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1577965358228.png)

### 4.4添加等待

引入time包，就可以在脚本中自由添加休眠时间

```
import time
time.sleep(3)
```

智能等待

添加implicitly_wait()方法就可以实现智能等待;implicity_wait(30)比time.sleep()更智能，后者固定时间等待，前者是在一个时间范围内智能的等待。

```python
#coding=utf-8
from selenium import webdriver
brower = webdriver.Chrome()
brower.get("http://www.baidu.com")
brower.implicitly_wait(30) #智能等待30秒
brower.find_element_by_id("kw").send_keys("selenium")
brower.find_element_by_id("su").click()
brower.quit()
```

### 4.5打印信息

栗子：打印出title和url

```python
#coding=utf-8
from selenium import webdriver
brower = webdriver.Chrome()
brower.get("http://www.baidu.com")
print brower.title #打印页面title
print brower.current_url #打印url
brower.quit()
```

控制台输出：

![1577969374778](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1577969374778.png)

### 4.6浏览器操作

#### 4.6.1浏览器最大化

由于调用启动的浏览器不是全屏的，可以最大化提高脚本执行观看效果

```python
#coding=utf-8
from selenium import webdriver
import time
brower = webdriver.Chrome()
brower.get("http://www.baidu.com")
print "浏览器最大化"
brower.maximize_window() #浏览器最大化
time.sleep(2)
brower.find_element_by_id("kw").send_keys("selenium")
brower.find_element_by_id("su").click()
time.sleep(3)
brower.quit()
```

控制台输出：

![1577970198825](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1577970198825.png)

#### 4.6.2设置浏览器宽、高

可随意设置浏览器的长宽高

栗子：设置浏览器的宽为400，高为800

```python
#coding=utf-8
from selenium import webdriver
import time
brower = webdriver.Chrome()
brower.get("http://www.baidu.com")
time.sleep(3)
print "设置浏览器宽400，高800显示"
brower.set_window_size(400,800)
time.sleep(3)
brower.quit()
```

#### 4.6.3操作浏览器的前进、后退

在浏览网页时，我们可以随时前进、后退；但是在进行web自动化测试模拟该场景是一个比较困难的问题

栗子：先访问到百度首页，然后进入新闻页面，再后退到首页，最后前进到新闻页面

```python
#coding=utf-8
from selenium import webdriver
import time
brower = webdriver.Chrome()
#访问百度首页
first_url = "http://www.baidu.com"
print "now access %s" %(first_url)
brower.get(first_url)
time.sleep(3)
#访问新闻页面
second_url = "http://news.baidu.com"
print "now access %s" %(second_url)
brower.get(second_url)
time.sleep(3)
#返回（后退）到百度首页
print "back to %s" %(first_url)
brower.back()
time.sleep(3)
#前进到新闻页
print "forward to %s" %(second_url)
brower.forward()
time.sleep(2)
brower.quit()
```

控制台输出：

![1577973488685](C:\Users\岳心怡\AppData\Roaming\Typora\typora-user-images\1577973488685.png)

4.6.4控制浏览器滚动条

栗子：访问到百度首页，搜索selenium,然后把页面滚动条拖到底部，将滚动条移动到页面的顶部

```python
#coding=utf-8
from selenium import webdriver
import time
#访问百度
brower = webdriver.Chrome()
brower.get("http://www.baidu.com")
#搜索
brower.find_element_by_id("kw").send_keys("selenium")
brower.find_element_by_id("su").click()
time.sleep(3)
#将页面滚动条拖到底部
js = "var q = document.documentElement.scrollTop=5000"
brower.execute_script(js)
time.sleep(3)
#将滚动条移至页面顶部
js = "var q = document.documentElement.scrollTop=0"
brower.execute_script(js)
time.sleep(3)
brower.quit()
```

