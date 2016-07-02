# 高级Web技术Project

#### by 小组adweb1314
#### 成员：刘石坚 许海龙 徐昆
----
####前端：https://github.com/adweb1314/adweb
####后端：https://github.com/adweb1314/adweb_back
####数据：https://github.com/adweb1314/adweb_db
####WebService_Server：https://github.com/adweb1314/adweb_service
####WebServiceClient_Server：https://github.com/adweb1314/adweb_serviceClient
----
# 高级Web技术Project文档

Team adweb1314
刘石坚 许海龙 徐昆

###一、系统架构

######1、全部project采取前后端分离的架构。(Ionic+AngularJS+Cordova+SSM+Mysql)

(1)前端采用混合移动开发技术，开发基于Ionic、AngularJS、Cordova的混合移动应用：Ionic-css及Ionic-AngularJS实现视图展示，AngularJS实现模型与控制逻辑，Cordova实现移动应用的混合开发。
在技术上前端同时实现了C/S，B/S两种模式，在视图上，项目css的设计同时照顾到浏览器使用者的感受。发布时，前端项目既可以输出apk包用于移动平台，也可以输出war包用于部署到服务器。

(2)后端采用SSM框架，具体技术分别使用Spring Boot实现服务器的控制逻辑、部署，使用MyBatis实现服务器与数据持久层的链接，数据库采用Mysql。

######	2、全部project采取面向服务的架构。(WebAPI+RESTful+WebService[Axis2])

	(1)本项目同时使用百度地图和高德地图两家公司的接口：所有前端地图的现实、渲染、逻辑均基于调用基于高德地图提供的服务实现，故在地图的逻辑、设计上直接享有与高德地图相等价的健壮、易用等特性。对于特定逻辑，project后台中将部分百度地图API封装为服务供前台调用。

	(2)前后台所有数据的传输均基于后台REST化服务调用实现，实现了基于HTTP协议AJAX的数据传输，对于前台而言，后台所有信息、功能均描述在基于URI资源标识的含义内。后台由于其RESTful特性，不只是为本应用提供相应的数据、功能，在部署后也将为整个万维网扩展无数的URI资源。

	(3)对于一些特定逻辑，本project中使用了W3C中SOAP、WSDL规范的WebService技术，开发了基于SOAP调用服务、基于WSDL描述服务调用接口的WebService。对于WebService的实现，项目采用Axis2框架部署服务，服务逻辑使用纯净的POJO进行开发，服务的部署、发布、调用则借助Axis2框架实现。借助WebService技术，这些服务可以无障碍地直接在不同平台、不同语言中进行远程调用。这使得不只是在Web开发方面，进一步在不同要求的开发背景下也均能实现不同要求的服务远程调用。

 
图1 系统架构

###二、项目提交说明

	提交分为五个仓库（见https://github.com/adweb1314）
1、	adweb：Ionic项目，前端；
2、	adweb_back：Maven项目，主要后端；
3、	adweb_db：sql文件，开发、测试时数据库数据与表结构的导出；
4、	adweb_serviceClient：JavaEE Dynamic Web项目，次要后端；
5、	adweb_service：JavaEE Dynamic Web项目，WebService部署端。

######1、项目：adweb

项目adweb为project前端，Ionic项目，开发工作集中在/www目录下：
/www/css目录下为项目的样式设计，
/www/js目录下为项目所有控制、模型逻辑的实现，使用AngularJS，
/www/img为前台渲染时需要的个别图片内容
/www/lib存放Ionic对web开发相关的css与AngularJS支持，
/www/templates中为所有页面的html文件，设计遵循Ionic提供的支持完成。
/www/index.html中为对所有css、js、webAPI-js的引入及项目调试、执行的入口

开发及调试可利用静态部署（作为StaticWebProject部署）的方式直接运行index.html，部署需要在项目根目录执行cmd指令“Ionic build android”以输出apk文件。

 
图2 adweb，前端项目目录

######	2、项目adweb_back

	项目adweb_back为project的主要后端，Maven项目，前端绝大多数逻辑由此提供支持。
	项目分为三个子目录分别进行开发：
	/src/main/java/pj目录，主后端要逻辑实现：
/bean目录下为个别前台回传数据的数据类
		/ctrl目录下为前端请求的映射、处理、返回的实现类
		/support目录下为一些辅助完成/ctrl中具体功能的类
/Application.java为运行后台的入口，SpringBoot内置Tomcat容器，无需部署即可作为服务器运行
	/src/main/mybatis目录，全部数据库操作的实现（配置）
		/entity目录下为前台回传数据、数据库表结构映射的数据类
		/mapping目录下为实现数据库具体逻辑的xml配置文件，所有SQL语句所在
		/mybatis-config.xml为数据库基本信息（URL、用户、密码）的配置文件
	/src/main/resources目录，静态资源部署等
		/static目录下为后台所需所有静态资源（图片），在部署时将部署到部署路径的根路径，也可以直接在此部署静态Web项目
		/application.yml——SpringBoot启动时的配置文件

 
图3 adweb_back，后端项目目录

######	3、adweb_db：数据库数据、表结构导出
 
图4 adweb_db，数据库表预览

######	4、adweb_service与adweb_serviceClient：
	Dynamic Web Project项目，Web Service的部署端与调用端，次要后台，仅在运行“新建景观”逻辑时需要开启此两项服务。
 
图5 adweb_service部署
 
图6 adweb_serviceClient目录

###三、项目分工

本小组adweb1314因一位成员退课，为三人小组，每个人负责的工作量较之预定均大大偏多，以下为组内成员达成一致后的内部工作量百分比。

刘石坚：50%——架构，前、后端开发。
前、后端框架搭建，前端地图相关功能实现，前端景观相关功能实现(80%)，前、后端上传功能实现，后端WebService实现，项目测试、整合，文档。

徐昆：30%——后端开发，接口设计维护。
后端数据库管理，后端数据库所有逻辑实现，后端接口设计，接口文档。

许海龙：20%——前端开发。
前端个人相关功能实现，前端景观相关功能实现，项目测试、整合。

