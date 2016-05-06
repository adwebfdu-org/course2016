# Lab1

Document And Code(All Project)

However, All pdf docx is broken...

https://github.com/TruthABC/adweblab1/




# 高级web lab1文档 13300200017-刘石坚

#1、选题说明：

选题1（主要选题）：采用Phonegap（codorva）软件和HTML5的hybrid移动app开发（应用到Geolocation API，local storage等HTML5特征 ）。

选题2（次要选题）：JS前端框架的学习和实践（AngularJS, ReactJS等），以及Node环境下的部署和运行。

#2、项目概要：

项目体现为计算器混合应用。

项目运用Ionic框架（基于AngularJS,Cordova），通过编写html、css、js开发web、原生移动平台应用，即混合移动应用（hybrid app）。

项目开发分为视图部分（View）与控制部分（Controller），其中视图部分基于Ionic，引用Ionic的css，控制部分采用AngularJS实现页面逻辑（操作逻辑、地址路由）。

#3、环境约束：

1、项目要求配置Java环境。（JDK1.8.0_73）

2、项目要求配置Android环境。（Andriod SDK API-19）

3、项目要求安装Ionic、Cordova。（需要先安装Node.js，并执行“$ npm install -g cordova ionic”）

4、项目开发的IDE可以为WebStorm，可通过WebStorm直接在浏览器中调试项目（在windows下直接打开html文件因没有部署将无法正常运行）。

5、项目可以输出apk，供安卓系统使用，apk从debug路径中取出为：“adweblab1.apk”，经测试可在安卓模拟器GenyMotion中运行（Phone - 4.4.4 - API 19 - 768x1280）。

#4、开发说明：

1、项目通过Ionic命令“$ ionic start adweb1”建立，ionic生成项目的目录，以此为开发起点。（见图1）

2、本次项目代码工作在项目根目录的“www”文件夹下进行，编写html，css，js文件。（见图2）

3、为得到混合移动应用（hybrid app），项目需要使用Cordova进行编译，也可以使用Ionic指令，集成Cordova编译。（“$ ionic platform add android”，“$ ionic build android”）


















图1-项目开发根目录
















图2-代码工作目录

#5、应用介绍：

























图3 “计算器”功能

1、应用——“计算器”功能（图3）：
1)支持四则运算。
2)支持连续运算。
3)支持最近历史运算结果本地存储。
4)提供自动校正机制，容错能力强，输入序列任意。

涉及的视图、控制实现主要为三个文件：
1)“templates/tab-calculator.html”
2)“css/calculator.css”
3)“js/controller.js”。

2、应用——“获取坐标”功能：
在tab标签第二栏中调用了html5规范下的Geolocation API，获取所在地的经纬度。

3、“计算器”功能

4、“计算器”功能的计算逻辑使用AngularJS实现，逻辑细节实现为一个“状态机”（controller.js的代码注释中有详细说明），通过判定计算器所处状态进行数据处理，保证用户的所有输入序列都将为状态机的可接受序列（不会出现逻辑层面的BUG）。

细节：状态将在5个状态下跳转：[等待“0号运算数”输入状态（初）]、[“0号运算数”就绪状态]、[等待“1号运算数”输入状态]、[“1号运算数”就绪状态]、[刚完成一次“纯等于”运算状态]。

5、“计算器”功能的“last”按钮的功能为提取上次运算的结果并进入[刚完成一次“纯等于”运算状态]，采取html5的localStorage技术获取历史结果，使数据在关闭浏览器经过较长时间的情况下也可留存。

6、“计算器”功能的文本框通过AngularJS作用域变量，将视图与控制部分双向绑定。

