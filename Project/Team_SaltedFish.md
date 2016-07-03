# PlayAround项目文档  
  
目录：  
[项目框架](#项目框架)  
[项目提交](#项目提交)  
[关键功能设计](#关键功能设计)  
[团队分工](#团队分工)  
[代码链接](#代码链接)  
  
## 项目框架  
前端使用基于Ionic和AngularJS的MVC框架部署，View负责视图的编写，Controller处理交互过程，Model定义规则。  
后端使用基于Node.js的Express框架，并使用oracle数据库管理数据。  
项目主要使用了百度地图的API，前端基于百度地图提供的服务实现与地图相关的业务需求。  
  
## 项目提交  
1. 项目adweb-pj  
项目adweb-pj存放前端相关的内容，包括ionic实现的页面设计和angularjs的controller交互内容，具体如下：  
`/www/css`：项目的样式设计；  
`/www/img`：项目使用到的图片文件；  
`/www/js`：项目的JavaScript文件，包括交互控制、业务逻辑等；  
`/www/lib`：项目开发时用到的angular、ionic和jquery支持；  
`/www/index.html`：登录注册页面，是项目的入口；  
`/www/search.html`：项目主页的html，显示景观搜索；  
`/www/scenery.html`：显示地图界面和景观列表；  
`/www/route.html`：路线查询的html，用户查询路线时返回地图上路线图和详细的路线；  
`/www/routePlanning`：路线规划的html，将路线返回在地图上并显示路线周围的景点标示；  
`/www/avatar.html`：用户上传、更换头像的页面；  
* 项目adweb-pj-express  
项目adweb-pj-express存放后端相关的代码内容，  
`app.js`:  路由  
`index.js`:  文件上传  
`users.js`:  用户操作接口  
`sights.js`: 景点操作接口  
`tags.js`: 标签操作接口  
* 数据库数据  
云服务器端架设 Docker Orcale 数据库，进行远程访问  
  
## 关键功能设计  
1. 登录/注册   
使用了sha-1的加密方式来保证了密码的安全性  

  ``` javascript
  var password = $scope.password; 
  var sha = hex_sha1(password);
  ```
* 列表的分类显示和排序  
     采用了ionic和angularjs框架下的`ng-repeat`带有的order和filter实现了这两个功能。  
* 基于百度JavaScript API的地图功能  
所有百度地图的功能模块都被封装在`map.directive`里面进行。      
  1. 地理坐标定位  
  根据浏览器进行坐标定位。  
  本来想采用html5自带的定位方式，但是chrome浏览器无法使用该定位方式，出于兼容性的考虑，最后使用了百度地图API来实现这一个功能。  
  `var geolocation = new BMap.Geolocation();`  
  * 不同类别景观的显示和点击出现信息窗口、景观跳转：  
  根据景观类型设置不同图标显示在地图上的点，并对每个点添加包括景观简要信息文字、跳转按钮的信息窗口。

    ``` javascript
  var point1 = new BMap.Point(markers[i].position.lng,markers[i].position.lat);//根据景观经纬度设置位置  
  var firstIcon = new BMap.Icon("img/school.png",new BMap.Size(28,28));  
  var marker1 =new BMap.Marker(point1,{icon:firstIcon});//添加景观标注  
  var infoWindow = new BMap.InfoWindow(sContent,opts);//新建信息窗口  
  addClickE(marker1,point1,infoWindow);  
  map.addOverlay(marker1);//标注加入到地图  
    ```
  * 路线查询  
  利用百度API的步行规划接口，为用户显示两地之间的路线规划和到某个景观的路线  

    ``` javascript    
  var walking = new BMap.WalkingRoute(map, {renderOptions:{map: map, autoViewport: true}});          
  walking.search(startName, endName);  
    ```
  * 路线规划  
  获取起始点和终点的坐标，计算出中心点，以它为圆心，两点间距离为直径，获取改范围内的景观信息。  
  * 地块范围的划分  
  在数据库中存入该景物的地块的几个坐标，调用百度地图画多边形的函数方法，进行范围的划分。  

    ``` javascript
  var str = {"point":[  
  {"lng":"121.469178","lat":"31.240359"},  
  {"lng":"121.468504","lat":"31.241355"},  
  {"lng":"121.469492","lat":"31.242004"},  
  {"lng":"121.469834","lat":"31.241108"},  
  {"lng":"121.469681","lat":"31.240668"}  
  ]};  
  var p = new Array(5);  
  alert("111");  
  for(var i = 0;i<str.point.length;i++){  
      p[i] = new BMap.Point(str.point[i].lng,str.point[i].lat);  
      //alert(p[i]);  
  }  
  var polygon = new BMap.Polygon([ p[0],p[1],p[2],p[3],p[4]], {strokeColor:"yellow", strokeWeight:2, strokeOpacity:0.5});  //创建多边形  
  map.addOverlay(polygon);   //增加多边形  
    ```
* 上传文件（头像文件以及素材库文件）  
采用了express的fsExtra以及multer模块进行主要的文件上传。  
主要实现方法是先利用multer将文件拷贝到本地的`tmp`文件夹下，然后再利用fsExtra将文件写到服务器端。  

  ``` javascript
fsExtra.copySync(tmpFilepath, desFilepath);  
response = {  
    message: 'File uploaded successfully',  
    filename: desFilename  
};  
res.redirect('http://localhost:8100/scenery.html');  
res.send(JSON.stringify(response));  
  ```
  
* 拖动调整页面大小  
利用`jquery.ui.touch-punch.min.js`实现。  
  
* 后台与数据库的交互操作  
利用express的oracledb模块实现。  
  
## 团队分工  
* 夏晏 12300120012
前端交互设计及界面设计、主要界面实现、文件上传实现

* 康明宇 13307110293
后端结构搭建、数据库操作、百度地图API相关

* 陈文卓 13302010095
前端网页设计、百度地图API、第三方API

* 居琪 13302010083
前后端数据传输、百度地图API
  
## 代码链接  
前端代码   https://github.com/akixy26/adweb-pj.git  
后端代码   https://github.com/akixy26/adweb-pj-express.git  
Express服务器--数据库接口与文档说明   https://github.com/kenkangxgwe/express-server-for-adweb-pj.git  
