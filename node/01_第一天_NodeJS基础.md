## Node基础

### 为什么学习Node?

* IO优势
  * 对于文件读写,Node采用的是非阻塞IO
  * __传统IO在读写文件的时候CPU来处理,而代码执行也处于等待中,浪费性能__
  * __非阻塞IO将读写操作交给CPU,而代码正常执行,减少等待浪费的性能__
* 应用场景
  * 实际应用: webpack/gulp/npm/http-server/json-server
  * 服务器中负责IO读写的中间层服务器(天猫中间层IO服务器)



### NodeJS特点

* 其移植了chrome V8 引擎,解析和执行代码机制和浏览器js相同
* 其沿用了JavaScript语法、另外扩展了自己需要的功能
* 总结: nodejs  是一个后端语言 ,  其具备操作文件的能力,  可以具备服务器的创建和操作能力
  * 其语法是JavaScript语法,代码运行在chrome V8 引擎之上




#### 基本使用

* 官网上下载 node-v-xx.msi 傻瓜式的安装包  一路下一步,就ok
* 检测是否安装成功 node -v
* 运行程序   node ./xxx.js


### 内置对象介绍

---

#### 分类

* 全局对象:  何时何处都能访问
* 核心对象:  向系统索要,引入即可使用
* 自定义对象:  按路径引入即可

#### process（全局对象）

* process.env 是一个对象，我们可以通过其.属性名来获取具体的环境变量值
    - 设置一个特定的环境变量,以达到简单区分不同的机器,从而针对生产/开发环境运行不同的效果
* process.argv 获取命令行参数

#### filename/dirname（全局对象）
* __filename 获取当前运行文件的目录,绝对路径
* __dirname 当前运行文件的绝对路径

#### nodejs实现规范

* CommonJS   :  规范JavaScript语言作为后端语言运行的标准
  * 具备什么能力,该怎么做 ,比如: 具备服务器的功能/  可以操作文件 .....
  * 模块应该怎么写: Module :     
    * 1:依赖一个模块   require('模块名(id)')
    * 2: 需要被模块依赖   module.exports = 给外部的数据
    * 3:一个文件是一个模块

#### 核心对象path
* 1:`const path = require('path');`
* 路径 -> 在处理路径的时候很擅长,但是,其不负责判断路径是否存在文件
* 拼接并修正路径 `path.join(__dirname,'a','b');` 以当前目录/a/b
* 接收一个合法的路径字符串，转换成一个对象
    - `let pathObj = path.parse(mypath);`
* 接收一个路径对象，转换成一个字符串路径
    - `let str = path.format(pathObj);`

```javascript
{ root: 'C:\\',
  dir: 'C:\\Users\\孙悟空',
  base: '金箍棒.txt',   // 该属性可以用于修改文件名和后缀
  ext: '.txt',
  name: '金箍棒' }

```
* __注意:path对象是方便我们操作路径的,对于获取来讲: parse解析成对象,format转换成字符串.join拼接并修正.... 对于修改路径对象来讲,可以用base属性更改,不能用name,ext更改__



#### 模块

* 弊端
    - 在js中要涉及到逻辑,还要在html中，为逻辑对象考虑引用顺序
    - 所有对象默认都是全局对象，命名冲突
    - commonjs规范
    - 一个文件就是一个模块
        + 导入用require('./xxx.js');
        + 导出用module.exports = xxx;
        + 在每一个模块内声明的变量属于模块内的作用域



#### 操作文件对象

* IO

  * I :input输入
  * O:output 输出
  * 文件的操作就是IO

* 复制文件的过程,  I: 通过计算机,存储文件到剪切板

  * 粘贴到指定目录:   O: 通过计算机,将剪切板上的数据,写出到 指定目录

* node中有两种IO的操作

  * 同步IO

    * 一行代码(读文件)不执行完毕...后续代码不能执行

  * 异步IO (建议)

    * 一行代码(读写文件) 不执行完毕(正在读写中) ... 后续代码也可以执行

  * 代码体验:

    * 读写文件  

    * ```js
      const fs = require('fs'); //必须这个名称
      //读 fs.readFile(路径,回调函数);
      //写 fs.writeFile(路径,数据,回调函数);
      ```

    * 总结: 异步的读/写文件  参数1:都是路径,可以相对可以绝对,最后一个参数都是回调函数,回调函数的参数中错误对象优先



* 同步和异步IO的区别: 同步IO会阻塞后续代码执行,异步IO不会阻塞后续代码执行



#### 包（文件夹）

* 多个文件，有效的被组织与管理的一个单位
* 留一个入口

#### npm
* 自己先有一个包描述文件
* 创建一个包描述文件 `npm init`
* 下载一个包 `npm install art-template jquery@1.5.1 --save`
    - 记录依赖`--save`
* 根据package.json文件中的`dependencies`属性恢复依赖
    - 恢复包 `npm install`
* 卸载一个包 `npm uninstall jquery@1.5.1 --save`
* 查看包的信息
    - `npm info jquery`
* 查看包的信息中的某个字段(版本)
    - `npm info jquery versions`
* 查看包的文档
    - `npm docs jquery`
* 安装全局命令行工具
    - `npm install -g http-server`
* 卸载全局命令行工具
    - `npm uninstall -g http-server`
* 查看全局包的下载路径
    - `npm root -g`

#### nrm是npm的镜像源管理工具
* 1:全局安装 `npm install -g nrm`
* 2:查看当前可选的镜像源 `nrm ls`
* 3:切换镜像源 `nrm use taobao`

#### 包的加载机制
* 我们未来可能需要辨识一个包中，入口是否是我们想要的启动程序
* 1:查找node_modules下的包名文件夹中的main属性(常用)
* 2:不常用:查找node_modules下的包名.js
* 3:查找node_modules下的包名文件夹中的index.js(常用)



### fs文件模块

* 文件读写
* 其他功能
* 扩展介绍

### http核心模块
---
#### http超文本传输协议
* 协议至少双方 -> http双方！！
* 客户端(浏览器)    -> 服务器
    - 原生应用(QQ)  -> 服务器

#### 请求与响应交互的过程
* 见图

#### 主体对象
* 服务器对象
* 客户端对象
* 请求报文对象(对于服务器来说，是可读)
* 响应报文对象(对于服务器来说，是可写)

#### 创建服务器步骤
* 1:引入http核心对象
* 2:利用http核心对象的.createServer(callback); 创建服务器对象
* 3:使用服务器对象.listen(端口,ip地址) 开启服务器
* 4:callback(req,res) 根据请求处理响应

#### 请求对象
* 请求首行中的url `req.url ` 
* 请求首行中的请求方式 `req.method`
* 请求头中的数据`req.headers`  是一个对象
* 头信息中，也可以作为与服务器交互的一种途径

#### 获取请求体数据

* 代码对比


* 浏览器:  $('#xx').on('submit',function(e){    })
* 服务器:  req.on('data',function(d){ d.toString(); })

#### querystring核心对象
* querystring.parse(formStr)
* username=jack&password=123转换成如下
* { username: 'jack', password: '123' }

#### 响应对象
* 响应首行 `res.writeHead(状态码)`
* 写响应头 
    - 一次性写回头信息
        + `res.writeHead(200,headers)`
    - 多次设置头信息
        + `res.setHeader(key,value);`
* 写响应体
    - 一次性写回响应体
        + `res.end();`
    - 多次写回响应体
        + `res.write();`

#### 请求与响应
* 头行体
* content-type是对请求或者响应体数据，做出的说明

#### 响应体数据
* res.write('字符串'||读出文件的二进制数据)
* res.end('字符串'||读出文件的二进制数)

#### 回写页面
* 英雄列表
* art-template http 
* 只能是访问 get请求   url: /hero-list  才返回该数据
* 其他请求返回ok
