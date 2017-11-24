
## 我的音乐列表

### 项目介绍
* 一个多用户的音乐管理网站
### 项目基本业务介绍
* 注册页
  * 用于用户账号注册
* 登录页
  * 用户登陆 后面页面登陆拦截
* 音乐列表页
  * 展示当前用户的音乐列表页
* 音乐添加和修改页
  * 当前用户添加和修改音乐

### 项目构成
* 服务层 node.js
* 数据层 mysql
* 应用层 前台音乐管理界面

### 项目阶段

#### 项目技术选型
* 基于node的服务层
* 应用框架选择 express
* 应用到的模块
  * express-art_template  模板引擎
  * mysql  数据库模块
  * fs-extra  文件操作拓展模块
  * captchapng2  彩色验证模块
  * express-session 处理session的模块,记住用户
  * body-parser  用于处理post表单提交
  * formidable   用于处理文件上传

#### 数据库建模

* 用户表 users
  * 唯一标识 => id
  * 用户名 => username
  * 邮箱 => email
  * 密码 => password
* 音乐列表 music
  * 唯一标识 id
  * 歌曲标题  title
  * 时长  time
  * 歌手  singer
  * 所属用户 uid 用于关联users中的id
  * 歌曲路径  file
  * 歌词路径 filelrc

#### 目录结构

* app.js 程序入口
* views 模版展示页面
* controllers 保存各个api实现的封装函数,链接数据层和前台展示
* models  数据管理
* ...待续...

#### 后端接口实现阶段
##### 路由接口  
* 服务器搭建 app.js
* 中间件配置
  * 在路由使用session之前，先生产session
  ```` javascript
    app.use(session({
      secret: 'itcast', //唯一标识，必填
      resave: false, 
      //true强制保存,不管有没有改动session中的数据，依然重新覆盖一次
      saveUninitialized: true,//一访问服务器就分配session
      //如果为false,当你用代码显式操作session的时候才分配
      // cookie: { secure: true // 仅仅在https下使用 }
  }));
  ````

* 处理请求
  * app.use()
* 注意事项
  * 引入body-parser第三方对象后别忘了在处理路由中间件之前进行对post请求的解析
  ```` javascript
  处理post请求数据
  // parse application/x-www-form-urlencoded
  app.use(bodyParser.urlencoded({
    extended: false
  }));
  // parse application/json
  app.use(bodyParser.json());
  ````
* formidable 中的参数files和fields
* 接口返回json格式的code编码和对应的描述
  ```` javascript
  res.json({
    code:'001',msg:'成功'
  })
  ````


##### 相关页面的后端渲染  接口 res.render()

* 登陆
* 注册
* 音乐列表
* 添加音乐
* 更新页面

##### 其他
* 抽离用户验证拦截功能 结合正则表达式 在路由中间件执行之前先设置用户拦截
# 前端页面展示

* 登录页
* 音乐列表
  * 歌词滚动的实现
    * 思路 以事件为属性 歌词为值 创建歌词对象
    * 对歌词处理
      * 先切割
      * 在使用正则获取时间  转化为对象key
      * 获取 歌词部分 对象的值
    * 获取歌词时间
      * 给audio添加播放事件updatetime
      * 获取e.targert/currentTime() TODO:
    * 获取歌词 及 滚动时的top值
    * 其他 for循环中的continue 
* formData文件上传
  * new FormData()
  * formData.append()
  * $.ajax 中添加两个属性
    * contentType:false,
    * prosessType:false,
* 密码强度
* 登陆后右上角显示当前用户信息  
  * art-complate中的 if语法         * if
      
      ````javascript
        {{if boolean}}
        {{else}}
        {{/if}}
      ````
  * session
* html公共部分抽离 
  * 胡子语法 include 类似php
* 在中间件中使用 app.locals.xxx.给模板引擎传值
#### 项目代码结构
* 中间件
  * 自定义功能
  * 第三方
    * 权限控制
  * 路由
    * api数据路由
      * 配置请求方式 + url = 控制器调用 => C
    * 音乐页面路由 + 用户页面路由
      * 数据+渲染  => V

* 数据库  => M
  * 