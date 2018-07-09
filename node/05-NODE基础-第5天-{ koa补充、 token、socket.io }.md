### koa补充、socket.io、cookie补充


#### cookie和session的补充

* cookie就是一个区分不同浏览器的标识

* session是服务器中的存储方式 

* cookie 常用属性 ```ctx.cookie.set('key',value,{options})

  * 服务器session机制回写cookie,或者直接回写cookie,不设置时间,都是浏览器关闭cookie就失效
  * `maxAge`   一个数字表示从 Date.now() 得到的毫秒数(如果maxAge和expires都识别,以maxAge为准)
  * `expires`一个数字表示从 Date.now() 得到的毫秒数
  * ```httpOnly``` 设置是否不允许客户端操作cookie
    * 保证兼容性可以同时设置,如果浏览器都识别,则优先以__maxAge为准__

* __注意:httpOnly属性如果为true,客户端无法获取__

  ​


#### jwt在koa中的使用

* jsonwebtoken  不基于框架的jsonwebtoken
  * 通过加密算法生成一个token
  * 解密token并验证token的有效性



```javascript
const jwt = require('koa-jwt');

const jsonwebtoken = require('jsonwebtoken');

let token = jsonwebtoken.sign({},'shhhhh'); // 登录后返回token

// 请求时验证token			// 不论如何都显示页面		// 排除 以/public开头的
app.use(jwt({ secret: 'shhhhh',passthrough:true }).unless({path:[/^\/public/]}));
```



* 小结: jwt的使用中有一些注意事项
  * 1. unless函数,代表排除哪些不进行token验证
    2. passthrough 代表是否最终都显示页面(通常设置为true,token的目的是记住用户状态)
    3. 当客户端获取到token之后,必须在请求头上加 authorization:bearer空格token
    4. 登录后 通过jsonwebtoken  去sign生成一个token,通过响应头给客户端



* 前端小结:

  * 1. ajaxSetup的作用 -> 简化代码,一次设置,此次生效

    2. 发送之前干嘛 -> 将token添加到请求头

    3. 响应之后干嘛 -> 获取token并保存

       ​





#### cookie存在的安全性问题

* CSRF (Cross-site request forgery 跨域站点伪造) __伪造请求,黑客骗你带cookie发请求__
* 试想: 用户不小心进入黑客准备好的页面,页面内隐藏了一个表单提交
  * 提交时携带用户的数据.. 
  * 能通过什么???   用户的cookie数据
* __大前提:用户先登录过某网站,被黑的时候,cookie还在有效期__

#### 什么是token???

	1:token就是令牌,将令牌传递给服务器,从而服务器根据不同的令牌识别不同的用户.
	2:token机制下伴有加密算法,从而让该标识更安全
	3:token客户端的存和取需要自理,cookie自动化
	4:token机制类似cookie,区别在于非自动而全手动的响应/请求头

* 总结: token就是客户端全手动的用户标识,cookie就是客户端自动的用户标识

为什么使用token???

那么如果机制一样.. 我们只有在cookie无法实现需求的时候才使用token

应用场景:
	1: 移动端原生应用(根本没有浏览器更不谈cookie了)

讨论:
	跨域的场景下是否必须使用token???
	1: 由于跨域默认不允许携带cookie,所以导致cookie看起来好像无用了,
	因此我们会想到使用token
	2: 注意: 
	    跨域下CORS也是可以携带cookie的,只是需要在服务器和客户端做一些设置

		1. 服务器允许携带cookie
		2. 服务器不能origin是*号,只能是单个域名
		3. 客户端ajax也要进行设置,默认不允许

​	    jsonp会自动携带cookie
	    代理也能代理请求的cookie等头信息![52717082668](assets/1527170826683.png)
	3:总结:基于第1/2点描述,只有第1条满足的情况,我们必须使用token....

![52717283077](assets/1527172830774.png)



#### 今日总结:

	1. cookie -> httpOnly -> 为true就不让客户端document.cookie来操作
	2. cookie和token的区别
    	1. 共同点:  带一个标识让服务器认识,通过请求头告知服务器,服务器通过响应头给浏览器
    	2. 不同点: cookie客户端从存到用都是全自动,token是全手动
    	3. 应用场景: token就是在移动端使用
	3. token来模拟cookie的实现思路
    	1. 为了代码简洁,可以配置全局默认行为: 请求前,检查是否有token,加入请求头,登录后,获取响应头token保存
    	2. 服务器: 登录后生成token(jsonwebtoken.sign),响应头给客户端,其他需要身份的页面直接ctx.state.user获取加密的数据
	4. 防御CSRF的策略
    	1. 让标识更复杂,token复杂一点
    	2. 辨识来的页面,rerefer请求头判断(js可以改)
    	3. 响应一个转账前的必然页面,生成一个标识,放到页面隐藏域,并存储到session中
        	1. 如果当前提交的隐藏域结果不与session中的一致,说明,要么页面不是这个页面,要么已经有了另一个该页面
    	4. 尽可能减少cookie过期时间,不要太久了,增加被黑的时长范围



#### 第三方中间件

* 处理请求体 __koa-bodyparser__


* 处理路由 __koa-router__
  * 获取查询字符串 ctx.query
  * 获取/xxx/:id      ctx.params.id
* 处理静态资源 __koa-static__
* 渲染页面 __koa-art-template__
  * koa与视图通信的对象 __ctx.state__
* session中间件 __koa_session__


#### 聊天室练习

### koa-socket

#### 核心思想

* 交互方式可能通过websocket/轮询ajax/服务器响应流
  * 1. 服务器可主动发数据到客户端
    2. 客户端向客户端发数据通过服务器
* 服务器广播   ```io.broadcast('事件名',{ 数据} );```
* 服务器向客户端||客户端向服务器  ``` socket.emit('事件名',{数据});```
* 客户端接收  ```socket.on('事件名',data=>{} )```
* 服务器接收  ```io.on('事件名',data=>{});```



#### 进阶学习

* 私聊
  * 1. 客户端告诉服务器to谁,及内容
    2. 服务器通过app._io.to(socket.id)找到目标客户端,再通过emit通信
* 群组聊天
  * 加入群组 ```ctx.socket.socket.join(newGroup._id);```
  * 向群里通信```ctx.socket.socket.to(groupId).emit('message', messageData);```