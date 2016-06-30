# 高级Web课程项目文档
---

## 小组成员
### `13302010041/叶韬` `13302010040/谢贤成`  `13302010055/陈高星`  `13302010076/王思杰` 
 
## 1.项目框架设计
### 1.1前端设计框架
#### 前端采用成熟的Ionic+Cordova构建混合移动应用。UI绘制主要基于Ionic的css和UI控件，逻辑控制及数据控制主要基于AngularJs这个优秀的前端框架。其中：
 
> * controller负责前端数据控制和逻辑控制
> * service负责提供逻辑服务，数据持久化服务以及测试样例数据
> * route基于angular-ui-router构建前端路由，前端路由符合restful风格
> * template构建前端页面
> * css目录索引css样式文件
> * img目录索引静态图片资源


#### three.js加载3d模型
#### 项目中使用three.js加载obj文件并展示3d模型。通过three.js中OBJLoader加载文件得到模型对象然后加入场景中。

```	
    var objLoader = new THREE.OBJLoader();
    objLoader.setPath( path);
    objLoader.load( objName, function ( object ) {
        scene.add( object );
    }, onProgress, onError );
```
        
#### 在场景中添加DirectionalLight，并通过OrbitControls对camera进行操作，实现对3d模型的旋转观察。最后对场景进行渲染。

      var directionalLight = new THREE.DirectionalLight( 0xffeedd );
      controls = new THREE.OrbitControls( camera, renderer.domElement );
                  
      function animate() {
         requestAnimationFrame( animate );
         controls.update();
         directionalLight.position.set( camera.position.x, camera.position.y, 1 ).normalize();
         render();
      }
   
      


### 1.2 后端设计框架
#### 后端负责为前端提供业务数据,采用了Struts2-Spring-Hibernate框架，其中Struts2来管理MVC中的Controller,负责处理前端请求，Spring则负责统一管理Java Bean的创建与注入,提供IoC,AOP等功能,有效的降低各个模块之间的耦合,Hibernate则封装了底层的数据库操作，使得我们能够以面向对象的方式去操作数据。
#### 后端代码结构
> * action包含了所有的controller,controller使用service提供的服务处理业务逻辑,并返回json数据
> * bean包含了业务用到的模型
> * dao中的EntityDAO使用泛型封装了数据的常见操作
> * exception中定义了各种常见的异常以及异常拦截器，通过配置struts.xml统一拦截所有项目异常
> * filter包含了所有的过滤器，主要用于权限验证,拦截超出用户权限的访问
> * listener包含了各种监听器，监听session的变化
> * service定义了各个bean相关的service层的接口
> * service.impl应用dao层提供的dao实现了定义的接口
> * util包含了一些常用的方法，例如文件处理等等

### 1.3 项目分工
1. 后端架构设计与搭建(拦截器设计,权限设计),文档撰写,景点相关功能的后端实现 ----谢贤成
2. 前后端接口文档撰写,后端数据模型构建,用户相关功能后端实现 ----叶韬
3. 前端架构设计与搭建,前端用户相关交互与景点展示设计与实现,文档编写    ---陈高星
4. 第三方登录调研,3D模型载入研究,景点相关交互设计与实现     ---王思杰

## 2.项目链接
### [项目地址](http://git.oschina.net/TwoColors/adweb)