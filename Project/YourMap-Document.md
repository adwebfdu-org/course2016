##高级Web技术  2016春  期末项目文档
##YourMap
---
出品组：高

秦海峰 13302010079 / 
陈晨光 13302010033 / 
汤定一 13302010077 / 
雷丽暇 13302010104 / 
仝嘉文 13302010085 

>###目录
>####1 项目设计
>####1.1 设计框架
>####1.2 关键功能点
>####2	团队分工
>####2.1 分工安排
>####2.2 过程管理
>####3	代码链接


####1. 项目设计
####1.1 设计框架
前端使用AngularJS的MVC框架部署，使得View和Controller分离，并在测试前端时使用Model管理测试样例数据。Controller管理前端页面跳转、前后台交互等业务逻辑，View负责各个页面渲染。

前后端业务分离，后台使用Spring-Struts-Hibernate框架部署，其中Spring负责集成管理，在applicationContext.xml中使用java bean封装数据库driver、数据库Dao、各个action handler以及service层的对象，提供了控制反转的条件；Struts管理action，负责接收和处理前台请求url，在struts.xml中把url和对应的处理类挂钩起来；Hibernate提供了EntityDAO对数据库的通用访问接口，通过添加Service层，根据业务需求调用EntityDAO，设计数据库访问存储的的代码。

####1.2 关键功能点

#### AngularJS使用百度地图API
创建地图实例之后，
```JavaScript
var map = new BMap.Map("container");
```
在前端AngularJS代码中调用百度地图的JavaScript开发API，使用定位控件、平移缩放控件、比例尺控件和自定义控件等地图控件，与地图UI交互相关的业务需求。

定位控件：
```JavaScript
var opts3 = {offset: new BMap.Size(5, 1150), enableAutoLocation: true} //定位控件位置
var geolocationControl = new BMap.GeolocationControl(opts3); // 设置定位控件 start
```
平移缩放控件：
```JavaScript
var opts1 = {offset: new BMap.Size(10, 1120)}
map.addControl(new BMap.NavigationControl(opts1));
```
比例尺控件：
```JavaScript:
var opts2 = {offset: new BMap.Size(10, 1120)}
var scaleControl = new BMap.ScaleControl(opts2);
```
自定义控件：
```JavaScript:
// 定义一个控件类,即function
function lookSightDetail() {
  this.defaultAnchor = BMAP_ANCHOR_TOP_LEFT;
  this.defaultOffset = new BMap.Size(50, 20);
}
// 创建控件
var myDetailCtrl = new lookSightDetail();
// 添加到地图当中
map.addControl(myDetailCtrl);
```
使用覆盖物Overlay接口标识指定区域，并通过标注Marker使用百度地图中已有的景点，
````JavaScript
var point = new BMap.Point(121.48, 31.22);
var marker = new BMap.Marker(point);
var infoWindow = new BMap.InfoWindow(sContent);
map.addOverlay(marker);
```

#### 用户数据的上传和发布
相比传统的Web1.0中，网络数据由官方的机构作为服务端发布，普通用户只能作为客户端访问数据，Web2.0是一个连接了众多普通用户创建的内容而构建的网络。Web2.0支持用户上传数据，本应用支持用户评价，对景点打分，上传图片、视频和模型等不同类型的文件，前端界面收集用户发布的这些数据，使用AngularJS的$http对象将数据发送给后台，后台根据请求url，查找Struts.xml定位到action handler类，接收数据，并存储文件到云端服务器。当用户在前端请求数据时，后端以JSON的形式将数据返回给前端，前台Controller接收数据，Html View将数据渲染成页面效果。

#### 使用Three.js 的3D景观展示
本应用中对于复旦大学和世博园中国馆添加了景观3D模型，3D模型的搭建包括材质、贴图、光线方面的处理。首先在maya中建模，然后用github上three.js项目中的exporter把maya中建好的中国馆模型导出成json文件，因为maya中的材质与three.js中的材质不通用，要在three.js中通过代码设置材质。在three.js中用THREE.JSONLoader导入JSON文件。中国馆的主体部分采用Phong材质可以体现金属的光泽，玻璃部分采用Lambert材质并设置为透明。光线采用平行光颜色白色。相机移动方式为Orbit，可以用左键控制镜头角度，右键控制相机位置，中键控制镜头远近。maya与three.js的三维空间不同，需要把从maya导入的模型沿y轴旋转-90度，沿x轴旋转90度。

#### 基于内容的推荐算法
为了给用户推荐他喜欢的景点，本应用使用了基于内容的推荐算法。n个景观，m个用户，建立一个景观-用户的n*m评分矩阵，其中每一个入口就是用户对该景点的“喜欢程度”，包括足迹、心愿单和收藏。评分矩阵为sightMatrix：
```java
//所有景点的特征集合矩阵，所有入口初始化为0
sightMatrix = new int[sSize][uSize];
```
如果用户对该景点添加了足迹，则在该入口加1，添加了心愿单和收藏的景点，则在该入口加2，不同类型的“喜欢程度”对应的具体分数值是我们组自己定的，可以修改。完成了每个入口的计算之后，该矩阵作为item-based的推荐基础，每个景观的特征向量为该评分矩阵中该景观的向量。对用户返回的推荐结果为，该用户没有足迹的景观中，与该用户已有足迹的景观最类似的前几个景观，返回结果最多不超过3个。景观之间的相似程度由景观特征向量的余弦夹角值表示。具体代码可以详见后端util包中的Recommend.java。
```java
public double computeSimilarity(int a, int b) {
	double similarity = -1;
	int[] v1 = sightMatrix[a];
	int[] v2 = sightMatrix[b];
	int x = 0;
	int v1LenSqr = 0;
	int v2LenSqr = 0;
	for (int i=0; i<uSize; i++) {
		x += v1[i]*v2[i];
		v1LenSqr += v1[i]*v1[i];
		v2LenSqr += v2[i]*v2[i];
	}
	similarity = x / (Math.sqrt(v1LenSqr) *  Math.sqrt(v2LenSqr));
	return similarity;
}
```

#### 基于GitHub的第三方登录
具体步骤为（1）创建访问第三方应用（GitHub）登陆页面的入口
（2）用户在第三方应用上登陆完成后，第三方应用返回code，
（3）本应用再给第三方发送带有code的登陆请求
（4）code验证成功后，第三方应用返回access token
（5）本应用将包含access token的登陆请求发送给第三方
（6）access token验证成功后，第三方应用返回登陆用户的相关数据，并跳回本应用指定的页面


####2. 团队分工

####2.1 分工安排
百度地图API使用、景观相关页面前台逻辑、搜索历史实现 ------ 秦海峰

用户管理相关页面前台逻辑、交互设计、界面优化调整 ------ 雷丽暇

Service层访问数据库接口设计与实现、Action Handler设计与实现、第三方登录实现 ------ 陈晨光

数据收集、数据库管理、高级推荐算法调研和实现、文档编写、测试 ------ 仝嘉文

第三方登录调研、3D模型开发部署 ------ 汤定一

####2.2 过程管理
PJ布置-5月中旬 ------ 需求理解、前后端接口定义、前端页面设计

5月中旬-5月下旬 ------ 数据库搭建、百度地图API调用

5月中旬-6月中旬 ------ 具体的各个前后端交互功能实现

6月中旬- 6月下旬 ------ 推荐算法实现、3D模型搭建、景观素材库搭建

6月29日凌晨 ------ 界面优化调整、测试与调整、文档编写

####3 代码链接

[前端代码]

[后台代码]

