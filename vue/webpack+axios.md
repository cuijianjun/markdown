### axios 及webpack

------

### axios

#### 基本使用

```js
Axios.method('url',[,..data],options)
.then(function(res){  })
.catch(function(err) { } )
```

#### 合并请求

- ```js
  this.$axios.all([请求1,请求2])
  .then(  this.$axios.spread(function(res1,res2){  
           
           })
  )
  ```



#### 取消请求

* API 3步骤

* ```js
  const CancelToken = axios.CancelToken;
  const source = CancelToken.source();  // 创建标识请求的源对象
                  
   this.source = source;  // 将对象存储到组建
   cancelToken: source.token, // 请求的options属性
   this.source.cancel();  // 取消到之前的那个请求
  
  // 前端的断点续传，  及时当前获取上传的文件大小，   存储起来
  var file = <input type="file" /> .files[0].slice(文件开始部分，文件结尾部分) 
  new FormData().append('file',file);
  
  // 后台就是接收多次文件，都往hbx.mp3这个文件上追加
  ```



#### 拦截器

* 请求拦截器： 在发起请求之前，做的事
* 响应拦截器： 在响应回来以后，做的事

- 单请求配置options: `axios.post(url,data,options);`
- 全局配置defaults: `this.$axios.defaults`
- config : `请求拦截器中的参数`
- response.config `响应拦截器中的参数`

- options
  - baseURL 基础URL路径
  - params 查询字符串（对象）
  - transformRequest：function(post请求传递的数据) {  }  转换请求体数据
  - transformResponse：function( res) { 自己转换相应回来的数据 } 转换响应体数据
  - headers 请求头信息
  - data 请求体数据
  - timeout 请求超时，请求多久以后没有响应算是超时（毫秒）



### 模块化

- webpack命令
  npm init -y
  npm install webpack@3.6.0 --save-dev --registry https://registry.npm.taobao.org
- package.json文件
  `"scripts": {
    "test": "webpack ./main.js ./build.js"
  },`
- 命令行运行 npm run test

#### ES6模块

- 导入和导出只能存在顶级作用域
- require引入是代码执行的时候才加载
- import 和export 都是提前加载 ，加载在代码执行之前

#### 箭头函数和function

- 一方面箭头函数是种简写形式
- 应用场景: 由于箭头函数本身没有this和arguments，通常用在事件类的回调函数上，让其向上级function绑定this，而非事件对象
- 箭头函数不可以作为构造函数

#### ES6函数简写

- 用在对象的属性中

  ```js
  fn3() { //干掉了:function,用在对象的属性中
  
  				console.log(this);
  
  		},
  
  ```

  ​

#### webpack

---



#### 历史介绍
* 2009年初，commonjs规范还未出来，此时前端开发人员编写的代码都是非模块化的，
    - 那个时候开发人员经常需要十分留意文件加载顺序所带来的依赖问题
* 与此同时 nodejs开启了js全栈大门，而requirejs在国外也带动着前端逐步实现模块化
    - 同时国内seajs也进行了大力推广
    - AMD 规范 ，具体实现是requirejs define('模块id',[模块依赖1,模块依赖2],function(){  return ;}) , ajax请求文件并加载
    - Commonjs || CMD 规范 seajs 淘宝玉伯
        + commonjs和cmd非常相似的
            * cmd  require/module.exports
        + commonjs是js在后端语言的规范: 模块、文件操作、操作系统底层
        + CMD 仅仅是模块定义
    - UMD 通用模块定义，一种既能兼容amd也能兼容commonjs 也能兼容浏览器环境运行的万能代码
* npm/bower集中包管理的方式备受青睐，12年browserify/webpack诞生
    - npm 是可以下载前后端的js代码475000个包
    - bower 只能下载前端的js代码,bower 在下载bootstrap的时候会自动的下载jquery
    - browserify 解决让require可以运行在浏览器，分析require的关系，组装代码
    - webpack 打包工具，占市场主流

```javascript
(function (root, factory) {  
    if (typeof exports === 'object') { 
        module.exports = factory();  //commonjs环境下能拿到返回值
    } else if (typeof define === 'function' ) {  
        define(factory);   //define(function(){return 'a'})  AMD
    } else {  
        window.eventUtil = factory();  
    }  
})(this, function() {  
    // module  
    return {  
        //具体模块代码
        addEvent: function(el, type, handle) {  
            //...  
        },  
        removeEvent: function(el, type, handle) {  
              
        },  
    };  
});  
```
#### webpack打包模块的源码
* 1:把所有模块的代码放入到函数中，用一个数组保存起来
* 2:根据require时传入的数组索引，能知道需要哪一段代码
* 3:从数组中，根据索引取出包含我们代码的函数
* 4:执行该函数，传入一个对象 module.exports
* 5:我们的代码，按照约定，正好是用module.exports = 'xxxx' 进行赋值
* 6:调用函数结束后，module.exports从原来的空对象，就有值了
* 7:最终return module.exports; 作为require函数的返回值

#### webpack配置文件
* 在当前命令行的目录中，创建一个webpack.config.js文件
* 由于webpack是基于node,配置文件中的写法参照commonjs的模块导出


#### webpack.config.js文件配置（认识）
* entry 是一个对象，程序的入口
    - key:随意写
    - value: 入口文件
* output 是一个对象,产出的资源
    - key: filename
    - value : 生成的build.js
* module 模块（对象）
    - loaders:[]
        + 存在一些loader   `{ test:正则,loader:'style-loader!css-loader'    }`


#### npm2.5以前
* 各个包内包含node_modules,会导致目录层级很深，有些包下载的时候就会报错
* npm2.5以后，安装的依赖包，就全部都是在平级根目录

#### 处理css
* require('./xxx.css');

#### 处理less
* `loader:'style-loader!css-loader!less-loader'`

#### 处理ES6
* babel是一个语法转换器,可以单独使用，也可以给打包工具结合起来使用
* webpack  -> babel-loader
* browserify -> babelify
* 可以转换es6/es7/react -> es5
* 设置当前语法: preset(预设)  es2015
* 下载预设
    - babel-loader + babel-preset-env(es2015/2016/2017)
* babel默认只转换语法部分 关键字 
* 函数部分需要插件支持

#### 处理文件 + base64
* url-loader 可以将文件生成为base64编码，到build.js中
* 文件在base64加密后会比原来大三分之一
* 应用场景是比较小的图片 4kb以内的图片

#### 特殊符号
* ! 分隔多个loader
* 字符串后跟上? 参数   key=value&key=value
* 对象: options:{ key:value,key:value  }

#### 字符串内使用的内置变量
* name: `[name].[ext]`
* name是获取原文件名,ext是获取原文件名的后缀
* output:{   path:'绝对路径', //设置产出的资源目录 ,filename:'build.js'}

#### 处理html
* html-webpack-plugin
* 1:下载
* 2:在webpack.config.js文件中引入
* 3:plugins属性中，配置该对象
* 4:给其options设置template（参照物）

#### 单文件方式
* 依赖 vue-loader
* vue-template-compiler


#### webpack-dev-server