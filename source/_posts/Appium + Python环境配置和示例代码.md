title: Appium + Python环境配置和示例代码
date: 2015-11-05 21:36:56
tags: [Appium]

---
Appium + Python环境配置和示例代码
====================

[Appium](https://appium.io)是[Sauce LABS](https://saucelabs.com)主要开发和维护的一个开源移动测试框架，它具有以下特点：
* 夸平台和应用类型
 + 支持iOS，Android，FirefoxOS
 + 支持Native App， Hybird App和Web App
* 支持多语言
 + 支持Python， Java， Object-C， Javasript， PHP， C#
* 非侵入式
 + 无需修改产品代码，无需重新编译
* 易于分布式部署
 + 基于WebDriver的HTTP Restful API的C/S架构
 
<!--more-->
 
下面简单介绍一下如何在Windows平台配置Appium + Python环境，以及介绍一段可运行的简单代码。本文主要关注Android平台，其他平台类似。

### 配置安卓开发环境
1. 下载安装[Android SDK](http://developer.android.com/sdk/index.html)，例如`D:\android\sdk`。

2. 编辑`PATH`，将`sdk\tools`，`sdk\platform-tools`添加至最后，例如：
 > `;D:\android\sdk\tools;D:\android\sdk\platform-tools;`  
  
3. 新建`ANDROID_HOME`，值为Android SDK所在路径，例如：
 > `D:\android\sdk`  

4. 运行sdk\SDK Manager.exe，下载安卓平台和工具，安卓平台选择你需要关注的平台版本，例如4.4.4。其他如下图即可：
![SDK Manager](http://7xo4cs.com1.z0.glb.clouddn.com/SDK.png)



### 配置安装Appium环境

1. Appium是基于Node.Js开发的HTTP Controller，所以需要先安装Node.Js，选择相应的平台版本[下载](https://nodejs.org/en/download/)

2. [下载](http://appium.io/downloads.html)安装Appium Desktop Apps

3. 利用`pip`安装selenium和appium For Python
```shell
pip install selenium
pip install Appium-Python-Client
```

4. 配置Appium。Appium配置比较少，主要关注一下端口号，另外建议勾选`Override Existing Session`，刚开始运行的时候经常会遇到`A new session could not be created`错误，勾选上这个就不会遇到这个错误了。如下图：
![Appium Settings](http://7xo4cs.com1.z0.glb.clouddn.com/Appium.png)

5. 点击![start](http://7xo4cs.com1.z0.glb.clouddn.com/start.png)启动Appium。
### 编写一个可执行的测试代码
如果你顺利的执行到这里的话，那么恭喜你，你离运行自动化测试只差编写代码了，先来一段代码执行吧。执行一下代码前，请确保你有唯一一台手机连接到电脑，并且开启USB调试，通过`adb devices`能够找到你的手机。这段代码以登录小米商城为例，执行前请先到小米商城[M站](http://m.mi.com)下载小米商城到`E:/python/`。
```python
# coding: UTF-8
from appium import webdriver
import time

desired_caps = {
    'platformName': 'Android',
    'deviceName': 'Android Emulator',
    'app': 'E:/python/mishop_4.0.8.apk',
    'resetKeyboard': True,
    'unicodeKeyboard': True,
    'autoLaunch': True
}
driver = webdriver.Remote('http://localhost:4759/wd/hub', desired_caps)
time.sleep(10)
service_tab = driver.find_element_by_android_uiautomator(
    'new UiSelector().className("android.widget.TextView").text("服务")'
)
service_tab.click()
time.sleep(5)
start_login_button = driver.find_element_by_id(
    'com.xiaomi.shop:id/usercentral_listheader_imageview'
)
start_login_button.click()

time.sleep(5)
login_button = driver.find_element_by_id(
    'com.xiaomi.shop:id/btn_start_login'
)
login_button.click()

time.sleep(5)
user_name = driver.find_element_by_id(
    'com.xiaomi.shop:id/et_account_name'
)
user_name.send_keys('yourname')

time.sleep(5)
password = driver.find_element_by_id(
    'com.xiaomi.shop:id/et_account_password'
)
password.send_keys('yourpassword')

time.sleep(5)
login_now = driver.find_element_by_id(
    'com.xiaomi.shop:id/btn_login'
)
login_now.click()
time.sleep(5)


driver.quit()
```