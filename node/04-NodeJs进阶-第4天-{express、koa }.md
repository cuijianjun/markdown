### 复习

#### cookie和session

1. cookie和session存放的地方
   1. cookie存放在客户端,session存放在服务器
2. cookie和session的安全性
   1. 相对来讲session数据更为安全,原因就是因为客户端小白多
3. cookie和session的关系
   1. cookie就是用来标识客户端是谁的一个暗号
   2. session可以理解为一个对象   {  cookie暗号: 该用户的信息   }

#### 跨域

1. jsonp  实战开发中: 前后端都要改代码 ,并且没有兼容性问题
   1. 动态创建script标签
   2. 生成全局回调函数名称,响应回来后,调用该函数 ```window.callbackxxx= fn;```
   3. 将回调函数名告诉服务器 通过script标签的src属性  ?callback=callbackxxx
   4. 将标签插入到body中
   5. 在回调函数执行后,移除window.callbackxxx;
2. cors  至少IE10.  通过服务器的响应,来让浏览器不多管闲事
   1. 机制A:   请求出去,响应回来,浏览器根据服务器的允许情况,决定是否组件js代码执行
   2. 机制B:  OPTIONS预检请求,  在ajax发起之前,浏览器模拟一个请求,先问一问,如果不允许,则阻止ajax请求的发起
      1. OPTIONS = 跨域 + (非常规请求方式 || 自定义的头 || content-type 不是键值对 )
   3. 实战开发中: 后端改代码,前端代码不改,但是有兼容性问题
3. 代理
   1. 通过代理服务器来获取数据,并返回给浏览器
   2. request第三方包可以轻松解决...考虑稳定性,我们一般不自己写



#### 配置https

* 公钥  公开的加密方式
* 私钥  存在服务器的唯一解公钥加密的方式
* 证书  确保当前发送数据单位可信
* 签名  确保数据的一致性



#### 启程helloworld
* 安装`npm i express -S`
* 1:引入express第三方对象
* 2:构建一个服务器对象
* 3:开启服务器监听端口
* 4:处理响应
* 在express中，保留了原生http的相关属性和函数

#### app.use(虚拟目录,fn)
* 小练习
    - 选择性荤菜素菜
* 用户/abc/def的请求
    - 选择性调用app.use('/abc',fn)的中间件
    - 但是内部req.url则去除了/abc这个暗号
* app.use(fn)是任何请求都会触发执行的

#### 中间件类别(了解)
* 应用级中间件 `app.use(事fn)`
* 路由级中间件 
    - 1:获取路径级中间件
    - 2:配置路由
    - 3:加入到应用程序控制中`app.use(router);`
* 内置中间件
    - 处理一些静态资源文件的返回(设置将某个目录下的资源文件向外暴露)  
       + 当url匹配上我设置的目录下的子文件后，自动返回该文件
       + 加入到应用程序控制中`app.use(内置中间件);`
* 第三方中间件
    - 更方便的处理cookie/session,简易的解析post请求体数据
    - 在npm上下载并使用
    - 加入到应用程序控制中`app.use(第三方中间件);`
* 错误处理中间件
    - 在express中统一处理错误

#### 路由中间件
* 一个请求进来(总网线),分发到各自不同的处理(分多根网线给其他人)
    - __分流__
* 后端路由
    - (请求方式 + URL = 判断依据)(分流的判断依据) -> 做不同的处理(分流后的行为)
* 使用步骤
    - 1:获取路由中间件对象 `let router = express.Router();`
    - 2:配置路由规则 `router.请求方式(URL,fn事)`
        + fn中参数有req,res,next
    - 3:将router加入到应用`app.use(router)`


#### res扩展函数
```javascript
res.download('./xxx.txt') // 下载文件
res.json({})  // 响应json对象
res.jsonp(数据) // 配合jsonp   要求客户端请求的时候也是jsonp的方式,  并且callback=xxx
res.redirect()  //  重定向 301是永久重定向, 302临时重定向
res.send()    // 发送字符串数据自动加content-type
res.sendFile() // 显示一个文件
res.sendStatus() // 响应状态码
```
* 总结
    - res.json() 响应数据,最常用 , 返回ajax数据
    - redirect() 重定向
    - download() 下载
    - jsonp() 跨域处理

### 模板渲染
---
#### 使用art-template模板引擎
* 下载express-art-template art-template
* app.js中配置
    - 注册一个模板引擎
        + `app.engine(后缀名,express-art-template);`
    - res.render(文件名,数据对象);
    - express这套使用，默认在当前app.js同级的views目录查找

#### 内置中间件(处理静态资源)
* 1: 创建对象 ```let static = express.static('./public');```
* 2: 配置到中间件中 ```app.use(static);```

#### 第三方中间件(post请求体的获取)
* 原生的:`req.on('data',data=>{ data.toString();})`

```javascript
const bodyParser = require('body-parser');
// 解析键值对application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false })); 
// 不用扩展的库来解析键值对，而使用node内置核心对象querystring来解析键值对
// 解析application/json
app.use(bodyParser.json());

```

#### 服务端处理错误和404页面找不到

* 404页面响应```router.all('*',()=>{} )```
* 触发错误
  * next(err);
  * 处理错误  app.use( 4参数函数 )

#### nodemon
* 修改代码自动重启
* 安装全局命令行工具 `npm i -g nodemon`
* 进入到指定目录命令行 `nodemon ./xxx.js`
* 手动触发重启，在命令行输入 rs回车


### koa

* 代码编写上避免了多层的嵌套异步函数调用 async await来解决异步
* 更轻...  减少了内置的中间件


* 启动步骤
  1. 引入Koa构造函数对象 ```const Koa = require('koa')```
  2. 创建服务器示例对象 ```const app = new Koa();```
  3. 配置中间件 ```app.use(做什么?)```
  4. 监听端口启动服务器 ```app.listen(8888);```
* 做什么? (函数参数说明)
  * context上下文对象: 该对象类似原生http中的 req + res
    * 简单说:  context 对象就是从请求到响应 过程中的一个描述对象
  * next函数:调用下一个中间件
* request(请求对象):  其中包含客户端请求的相关信息
* response(响应对象): 包含响应数据的具体操作



#### request常用属性

* ctx.request.url(ctx.url)
* ctx.request.method(ctx.method)
* ctx.request.headers(ctx.headers)



#### response常用属性

* ctx.response.set(ctx.set)  __函数:参数key,val__
* ctx.response.status(ctx.status)


* ctx.response.body(ctx.body)  




#### 小结

* 以上所有使用的属性,都可以简写 ctx.xxx
* 使用async await的应用场景,如果你出现了异步操作,使用其,  后一个中间件使用了async,前后都使用
* 三主角: __函数前面 async, 内部才能await,要想await能有用,就用promise包裹他__




#### 第三方中间件

* 处理请求体 __koa-bodyparser__


* 处理路由 __koa-router__
  * 获取查询字符串 ctx.query
  * 获取/xxx/:id      ctx.params.id
* 处理静态资源 __koa-static__
* 渲染页面 __koa-art-template__
  * koa与视图通信的对象 __ctx.state__
* session中间件 __koa_session__



#### cookie和session的补充

* cookie就是一个区分不同浏览器的标识
* session是服务器中的存储方式 
* cookie 常用属性
  * `maxAge`   一个数字表示从 Date.now() 得到的毫秒数
  * `expires`一个数字表示从 Date.now() 得到的毫秒数
    * 保证兼容性可以同时设置,如果浏览器都识别,则优先以__maxAge为准__