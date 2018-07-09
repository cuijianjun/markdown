## Node基础

#### 知识点介绍

1. 跨域细节知识
2. cookie-session的故事
3. 制造秘密 : 加密

#### 跨域问题

* 传统开发方式:前端代码及请求数据接口都在同一个服务器上,前端代码测试依赖服务器
* ![52673330919](assets/1526733309199.png)
* 前后端分离:
  * 静态服务器: 运行前端代码
  * 后台服务器: 运行数据接口服务器
  * ![52673347952](assets/1526733479527.png)
  * __互不影响,浏览器向其他服务器发送ajax请求,会产生跨域__
  * 跨域: __浏览器认为ajax请求数据,对于跨域的情况来说,是不安全的,同源策略__
  * 同源策略: ajax/iframe
    * ajax: 协议 + 主机 + 端口   其中有一个与之前页面所在的不一样,这个ajax请求就跨域了
    * iframe: iframe A   到iframe B  过程中操作共享数据.比如window.name,就报错


#### jsonp

* 知识点补充

  * 前端ajax不行了...  绕过,去使用script标签,加载的是js代码 ->  var x = 1;  -> 全局中有有了x这个变量
  * __script标签请求的js代码会被执行__
  * 需要在js中声明一个函数,用来作为js代码回来后的调用,参数是后台的数据
  * ![52673475575](assets/1526734755753.png)

* url核心对象

  * ```js
    const url = require('url');
    url.parse('http://xxx.com?id=1',true); // 第二个参数是将id=1转换成对象
    // output:  { protocal:'http',..省略..query:{id:1}   }
    ```

* 总结:

  * __编写代码:前端人员编写 全局函数 + script标签 + 传递函数名__
  * __后端人员:  响应 函数名('数据') 的字符串,让script标签执行__
  * jsonp请求方式是GET
  * 没有兼容性问题




#### CORS(跨域资源共享)

* 至少IE10支持
* ![52673569034](assets/1526735690341.png)


* ```
  Access-Control-Allow-Origin: 'http://xxx.com'  //允许哪个域在跨域的时候访问,*代表所有
  // 告诉浏览器,跨域时允许有cookie,同时客户端也要设置withCredentials:true + Origin不能是*
  Access-Control-Allow-Credentials: true  
  Access-Control-Allow-Methods: 'GET,POST,PUT,DELETE';   // 默认允许get/post
  Access-Control-Request-Headers:'xxx';   // 允许你自己加的头来通信
  ```

* 浏览器在非简单请求(get/post以外)||包含自定义头||content-type非键值对的时候,会先请示服务器,来一个OPTIONS预检请求,如果不满足,拒绝发送ajax请求

* __服务器Access-Control-Allow-Credentials: true  客户端: withCredentials:true__

  * 在跨域的时候,cookie跨域写回给浏览器,但是,默认是不会自动携带,需要客户端设置携带,并且服务器允许携带cookie才跨域

* 总结: 服务器在编写代码,客户端正常ajax就行

  * 服务器可以设置允许哪些域访问/哪些请求方式/哪些头字段存在
  * 跨域的时候也可以设置允许携带cookie
  * options预检请求:  常规请求是先发起请求,到服务器,服务器响应数据,被浏览器拦截
    * 预检请求,  浏览器先发请求到服务器,服务器响应,如果不允许,  本身的js发送不出去

#### 代理

* 服务器本身不受所谓浏览器同源策略的影响``` http://api.douban.com/v2/movie/in_theaters```


* 下载依赖包便于请求操作 ```npm i request -S```
* ![52673921121](assets/1526739211217.png)

#### nginx代理

* __操作最好在管理员权限下进行__

* nginx -s [opt]  opt:stop, quit, reopen, reload

* __代理配置:__

  * ```python
    location /apis {
        	    rewrite  ^/apis/(.*)$ /$1 break; #重写URL
                include  uwsgi_params; # 携带请求参数
                proxy_pass   http://localhost:8888;
                # 服务器回写的cookie的domain是 代理服务器,
                # 如此操作可以修改cookie的domain为 浏览器 
                # 浏览器cookie -> 代理服务器cookie -> 自动到目标服务器
                proxy_cookie_domain domino_server nginx_server;
            }
    ```

  * ![52674077862](assets/1526740778628.png)


* 启动nginx: 命令行进入到解压目录 ```start nginx ```
* 查看nginx启动进程 ```tasklist /fi "imagename eq nginx.exe"```
* 关闭进程 ```nginx -s stop```

