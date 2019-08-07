---
layout:     post
title:      使用Travis-ci和Eclipse进行在线build
subtitle:   --
date:       2018-3-15
author:     Raymond
header-img: img/home-bg-o.jpg
catalog: true
tags:
    - Eclipse 
    - Travis-CI



---



这学期软件构造的实验需要放到github上，然后TA会从你github上clone你的代码然后进行验收，这个地方就会存在一个问题，如果你的有些库TA电脑上没有，就会出现问题，这个时候有什么办法呢，就是利用travis进行在线build，在线build通过后，TA的电脑就一定能通过。

#### Travis-CI
Travis CI是在软件开发领域中的一个在线的，分布式的持续集成服务，用来构建及测试在GitHub托管的代码。登录它时可以把Github账号和它关联起来，然后就可以对github中的代码进行在线build。
##### maven
我们想进行在线build，还需要在线build工具，目前常见的工具有 gradle,maven,ant
ant的话配置文件特别难写，果断放弃，这个地方就采用了maven。
### 步骤
###### 1
在eclipse把项目变成maven项目
右键项目名，在configure里面选择 conver to maven project
然后项目里就会多出一个pom.xml配置文件。我们需要给这个文件添加一些依赖
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190310154323512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
这是我所用到的依赖，关于这个应该怎么写，有个网站叫
https://mvnrepository.com/ 里面可以查询到利用到的库的依赖应该怎么写。这个地方有一个坑，那个网站上的依赖，比如junit,会自动给你加上
```
<scope>test</scope> 
```
这句话，这个的意思是只在test文件夹里使用junit，而如果其他地方使用，就会报错，如果其他地方也用到了，就必须把这句话删掉。
还有另一个坑就是travis默认要使用UTF-8编码，所以如果以后有报错Maven: Using platform encoding (GBK actually) to copy filtered resources, i.e. build is platform dep，可以加上
```
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
```
###### 2
写好这个后，我们还需要在项目里写一个.travis.yml文件，用来告诉travis一些信息，最后的目录如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190310155920640.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
我的文件就是一个简单的java文件，所以.travis.yml里只有
```
language: java

jdk:
  - oraclejdk8
```
 这两句话，代表使用java，版本是jdk8
 其他语言或项目该怎么写可以参考travis里的文档
 ###### 3
 完成上述工作后，就把更改后的项目push到github上，然后登陆travis，找到你需要的repositary，进行在线build就好了。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190310155807213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190310160050954.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mjk3MDQ1Ng==,size_16,color_FFFFFF,t_70)