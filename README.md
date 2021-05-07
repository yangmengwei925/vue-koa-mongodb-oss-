## 线上

由于使用Element框架实现前端，并没有做移动端适配，所以建议PC端观看。

**线上地址：**[itc](http://itc.newarray.vip/index)

**项目地址：**[GITHUB](https://github.com/weirui88888/itc)

![star](https://user-gold-cdn.xitu.io/2019/11/30/16eb7fbf98cdd740?w=728&h=86&f=png&s=1558)

由于作者阿里云域名**正在备案**，当前小伙伴只能通过ip地址进行访问，后续备案通过后，会替换成域名访问形式。

**建议参考本文最后[教程](https://github.com/weirui88888/itc#%E6%9C%AC%E5%9C%B0%E5%90%AF%E5%8A%A8)，将项目克隆到本地启动（不需要任何阿里云系列产品即可本地启动），配合该篇文章进行阅读**


## 效果图
### 注册
![注册](https://user-gold-cdn.xitu.io/2019/11/30/16eb7ede52f3489f?w=1904&h=956&f=jpeg&s=149213)

---
### 登陆
![登陆](https://user-gold-cdn.xitu.io/2019/11/28/16eb1054ab42f4cb?w=1910&h=962&f=jpeg&s=149056)

---
### 首页
![首页](https://user-gold-cdn.xitu.io/2019/11/30/16eb7f1d690cb336?w=1430&h=778&f=jpeg&s=102219)

---
### 编辑
![编辑](https://user-gold-cdn.xitu.io/2019/11/30/16eb7f487f0ddd84?w=1428&h=780&f=jpeg&s=72518)

## 背景
最近想学习下koa框架，于是便从网上找了一些资料，在大致了解了koa框架的基本用法后，想要自己做点什么东西出来。

在掘金平台找到一篇[文章](https://juejin.im/post/5ba0a27b5188255c6a043058)，是一个前端用vue，后端用koa，数据库用mongodb实现的简单留言功能。看似简单，在阅读完代码后，发现有很多东西，之前还是没有接触过的。比如说后台生成验证码、token、密码加密等

于是我也仿着实现了一个简单的注册登陆模块，麻雀虽小，五脏俱全。**基本上整个前后端交互的细节都有涉猎，同时也将自己的作品上传至阿里云服务器，是非常适合新手学习和了解整个前后端交互的一套技术体系。**

这里假如你把整个项目弄明白了，那么你将会有以下收获：

- 可以部署自己的服务，让别人通过网址访问你的网站
- 知道整个前后端交互的一套流程
- 前端经常拿到的token是如何生成的
- 会考虑如何去逐步从各方面优化你的应用
- 会知道原来前端真的只是冰山一角，对前端有了重新的认知
- 拥有了想要成为全栈工程师的梦想

#### 主要包含以下模块

- 注册
- 登陆
- 首页
- 编辑页

#### 细节包含

- CURD增删改查
- 前端校验
- 上传用户头像
- 阿里云oss上传图片
- token鉴权
- 模糊搜索
- 排序
- 分页
- xss内容过滤
- 请求／响应拦截
- ...

#### 主要技术栈

##### 前端
`vue` `vue-cli3` `webpack4` `axios` `vue-router` `vuex`

##### 后台
`koa-generator` `koa2` `ali-oss` `gd-bmp` `jsonwebtoken` `koa/multer`

##### 数据库
`mongodb`

##### 服务器相关
`nginx` `pm2` `阿里云服务器` `域名` `oss对象存储`

##### 简单画了个流程图

![](https://user-gold-cdn.xitu.io/2019/11/28/16eb10d1ef3d5ef5?w=677&h=644&f=jpeg&s=33969)
## 前言
本篇文章，主要围绕整个开发和部署过程中的一些疑难点展开，具体的代码逻辑，个人建议，还是把代码拉下来跑起来，深度阅读几遍来的实在。


我认为这里面最值得学习的应是**思路**，不要在意一些细节，比如：

- 我用的数据库是mongodb，因为之前了解过，觉得合适自己，容易上手，如果你喜欢mysql，你大可以自己替换
- 我用的前端框架是element，如果你感到身体不适，习惯于用Ant Design又或是手写样式，你大可以自己折腾
- ...

**变通**，乃开发者应具备的不二神器。

当然，想要通读完代码或者是自己动手实现一个轮子，你需要具备以下基本知识

- Vue（vue-cli、vuex、vue-router、element、Axios...）
- Koa（路由、中间件...）
- Mongodb（数据库增删改查...）
- ES6语法（promise、async、await...）
- ...

具备了以上知识点，你能做到的，仅是在本地实现自己的轮子。如果想要最终部署在网上，让大家可以看到你的作品，参与进来，你可能还需要准备以下干粮

- 域名
- 服务器
- OSS对象存储
- nginx基本操作要领
- pm2基本操作要领
- ...

下面主要从前端、后端、数据库、线上部署四个方面做一些阐述

## 前端
使用vue-cli3脚手架生成前端骨架，后端骨架使用koa-generator生成

这里我为了节省开发成本，用了element框架，考虑到性能问题，我使用了element提供的**按需引入**方式，引入需要的组件。

为了能够在用户登陆后，以及整个流程中方便获取用户的相关信息，使用vuex全局注册单例对象store。

方便实现请求后台功能，以及请求拦截、响应拦截，使用Axios提供的实例方法

...
#### 交互细节
- 路由跳转根据页面是否需要权限以及是否已登陆做判断
- 路由带有redirect重定向字段时，用户登陆后，自动跳转到之前的页面
- 登陆、注册页面对用户输入内容做校验，比如用户名、密码的格式、长度等（考虑到后期数据库能力）
- 上传图片大小、个数、格式做限制，将图片上传至阿里云对象存储oss（**如果本地启动的话，不需要阿里云对象存储即可**）
- 登陆用户仅可以操作自己之前发过的帖子，其他用户的内容，仅具有查看权限
- 根据访问每条记录的详情次数，生成每条记录的热门程度，方便用户筛选
- 用户登陆后，将用户的token存在前端(store、localStorage)，每次调用后台接口，都会经过token验证这一层，如果token已过期，需要引导用户重定向到登陆页，这里token存储一份在localStorage下，目的是为了用户关闭页面后，再次访问时，如果token没有过期，直接可以进入对应的页面
- 页面涉及到操作后台数据的，比如点击按钮执行对应请求，阻止按钮重复点击动作
- 对于一些不会变动的库文件，比如vue、vue-router、vuex、axios等，通过webpack的externals配置项，可以在生产环境配合使用稳定的cdn提供服务，便于利用缓存机制，提升性能和交互体验
- ...

**备注**：这里关于图片上传，我最早的实现方式是将图片存储在nginx服务器上，然后将地址返回给前端，后来考虑到服务器性能问题，改用ali-oss上传图片，可以更好的利用缓存机制，提高用户体验

## 后端
后台框架使用既成的koa框架实现。这里我想说下，去年用express框架写过一个[express+mongodb+vue实现增删改查-全栈之路](https://juejin.im/post/5aabc2caf265da239376d5ff)，这次使用koa后，个人感觉在使用上并没有特别大的差异，大致就是一些api变化了，精简了整个框架，同时基于async/await语法糖可以更好地开发中间件。对于这些后台框架，如果你不是有志于成为领域佼佼者，我觉得，作为前端，知道它是干嘛的，以及怎么用的，就可以了。

#### 交互细节

- 通过mongoose中间件对象模型工具实现数据库连接，以及Schema创建
- 验证码通过gd-bmp实现，再用jsonwebtoken(jwt)根据一定规则，生成token后，一并存储在数据库中，用户登陆或者注册时，先判断验证码是否存在数据库中，再进行其他动作
- 封装check_token中间件，每次收到前端请求时，判断请求头里面的token是否过期，进而执行后续动作
- 使用@koa/multer实现前端上传图片，存储到server文件夹下，再用ali-oss上传图片到阿里云
- ...

**备注**：后台知识于前端而言，比较生涩隐晦，建议自己遇到不了解的，自行前往了解
## 数据库

数据库的话，这里我用的是Mongodb，推荐使用homebrew一键安装，
同时为了方便后续操作和查看数据库，建议下载一个Robo 3T GUI图形工具。
剩下的我觉得需要了解的就是Mongodb的数据库增删改查[API](https://docs.mongodb.com/manual/reference/method/js-collection/)

## 重点

到这里，本地开发基本没有问题了。换句话说，你可以在本地访问你的前端服务，调用后台接口，进行本地数据库的增删改查了。

但是我想你既然读到了这里，肯定是不满足于仅在自己的机器上折腾，你更加感兴趣的是如何将自己的项目部署在服务器上，让其他小伙伴也可以看到你的作品。更多细节请参考[阿里云服务器部署web项目全过程 ](http://www.sohu.com/a/341033591_100286357)。

接下来为了防止大家头皮发麻，我会用最朴素的言语，为你揭开神秘的面纱。

### 准备工作

#### 服务器
你想让人家看到你的作品，那你是不是需要准备自己的一台**服务器**，所以你需要先去阿里云买个服务器，至于服务器的标准，决定于你钱包里的毛爷爷

买了服务器之后，最直观的感受是，你只是拿到了一串数字（ip地址）

```
// 阿里云服务器ip地址
123.56.119.218
```
#### 域名
别人访问你的服务器，也只能通过ip地址访问，你绝不甘心于此，数字谁能记住？于是乎，你还需要去阿里云上面买个**域名** ，再通过域名解析，将你的域名指向你的ip地址，这样，别人就可以更从容的记住你的站点

```
// 小伙伴们通过访问域名，进入你的网站，而不是使用一串数字访问
www.xxx.com -> 123.56.119.218
```
#### 备案
有了域名和服务器后，为了证明你未来网站的合法性，你还需要去**备案**域名网站，周期大概20天左右。只有备案通过后，别人才可以通过域名访问你的网站，否则，只能通过ip地址进行访问。

在等待域名备案成功的这段时间，你要做的事情还有**不少**，同时也注定着蛋疼（踩坑）

#### 上传文件至服务器

通过命令行ssh root@xxx 方式连接服务器，其中xxx为你的服务器ip地址，
进行相关依赖的安装。这里包括但不限于mongodb、nginx、pm2、node、git等，说白了，就是要在服务器环境上搭建类似于你的本地环境，在安装的时候，最好注意下相关的版本，为了避免出现兼容问题，你可以看下你本地的版本，对应下载即可。

这里我不想做过多描述，因为本人搭建的时候，也是踩了好多坑，建议新人都踩一遍，遇到不会的，网上找教程，你遇到的坑，前人都趟过了。只有自己多踩坑，才能记忆深刻。

当这些工作都准备完毕后，你需要一个工具，可以将你本地的代码，传输到服务器上，这里推荐大家使用FileZilla，方便、快速。比如说我本地开发完后，执行npm run build后，生成的dist目录内的文件和server文件夹，全部通过FileZilla上传到/home目录下的www文件夹下，www这个目录是你自己创建的，你可以任意取名字，但是这里的路径，跟后面需要配置的nginx静态资源目录路径root有一定的关系。

![centos](https://user-gold-cdn.xitu.io/2019/11/29/16eb7a5f5e120cd8?w=866&h=121&f=jpeg&s=52870)

#### 服务器安装PM2
到这里，基本上所有的准备工作已经就绪，你只需要通过命令行进入服务器上存放后台代码的server文件夹，用pm2启动后台程序即可。关于pm2，你可以阅读[PM2入门](https://wohugb.gitbooks.io/pm2/content/features/quick-start.html)。

```
假如你的server文件夹放在/home/www下，你只需要执行以下几步

1.cd /home/www/server
2.pm2 start bin/www --name xxx (这里的xxx是你为你启动的服务起个名字)

关于pm2的相关命令，可以使用pm2 examples查看示例
```

#### 服务器安装Nginx
首先，你要知道我们的静态资源和后台服务代码都存在nginx服务器的一个目录下

在通过域名解析后，当我们通过域名访问前端静态资源时，会指向你之前在nginx.conf文件里面的root目录，同时会访问你的默认首页index.html

这时，前端已经就绪了，但是因为后台程序是启动在服务器的本地3000端口上，如果直接访问，会存在跨域的问题，这个时候，nginx的主要作用之一**反向代理**就起作用了

```
// nginx.conf配置文件

server {
    listen 80; // 默认端口
    server_name 123.56.119.218; // 域名，当你访问这个域名时，会直接去找/home/www目录下的index.html文件
    root   /home/www; // 根目录
    index  index.html index.htm index.php; // 默认首页
    
    // 反向代理
    location /api/ { // 前端访问api下的接口时，会代理到proxy_pass指向的地址
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:3000; // 反向代理，用户访问api接口时，指向本地服务3000端口
    }
}

```

**提示：** 每次修改完nginx.conf配置文件，都需要执行以下命令，重新启动nginx服务器

```
重启nginx服务器步骤

1.  cd /usr/local/nginx/sbin
2.  ./nginx -s reload
```

## 本地启动

首先你本地要全局安装了mongodb、node、npm、robo3T等相关工具，本地启动mongodb

```
1.git clone https://github.com/weirui88888/itc.git 把项目克隆到本地

2.cnpm install // 项目根目录安装前端项目依赖

3.cnpm install // server目录下安装后端项目依赖

4.在根目录下启动前端程序

    npm run serve
    
5.在server目录下启动后端程序

    npm run start

6.访问localhost:8080即可看到成果
```

**如果开通了阿里云OSS对象存储，想要将图片上传至阿里云对象存储的话，请参考server/controller/record.js文件中的详细注释**

## 最后

在整个搭建的过程中，会遇到各种各样的问题。但是正是这些问题，会让我们成长，也认识到自己的微不足道，同时也鞭策着我们不断前行，愈战愈勇。这里也要感谢[licy97love](https://juejin.im/user/59eeff42f265da43070275e1)这位同学，在部署服务器过程中给予我的帮助和指导。

这是我第一次写一个完整的前后端应用并部署服务器，所以难免程序中，会存在一些瑕疵，望包含。同时如果有一些疑惑点或者是建议，可以提`issue`，也可以发邮件`call`我。

**线上地址：**[itc](http://itc.newarray.vip/index)

**项目地址：**[GITHUB](https://github.com/weirui88888/itc)

欢迎`star`和`fork`,谢谢！

写在文章的最后，如果这篇文章，给你启示或是成长，请为我点亮一颗小红心。**程序员，码字不易，谢谢**。

![star](https://user-gold-cdn.xitu.io/2019/11/29/16eb7bb01a8f4894?w=728&h=104&f=png&s=3307) 
## 参考

[VUE+KOA+MONGODB小练习](https://juejin.im/post/5ba0a27b5188255c6a043058)

[centos云服务器系统下vuecli+koa实现前后端分离项目的部署](https://blog.csdn.net/zxy15946565183/article/details/82317887)

[PM2入门](https://wohugb.gitbooks.io/pm2/content/deployment/getting_started_with_deployment.html)

[Mongo API](https://docs.mongodb.com/manual/reference/method/js-collection/)

[前端想要了解的Nginx](https://juejin.im/post/5cae9de95188251ae2324ec3#heading-6)

[Nginx与前端开发](https://juejin.im/post/5bacbd395188255c8d0fd4b2)