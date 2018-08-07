#### webpack

---

#### 历史介绍
* 2009年初，commonjs规范还未出来，此时前端开发人员编写的代码都是非模块化的，
    - 那个时候开发人员经常需要十分留意文件加载顺序所带来的依赖问题
* 与此同时 nodejs开启了js全栈大门，而requirejs在国外也带动着前端逐步实现模块化
    - 同时国内seajs也进行了大力推广
    - AMD 规范 ，具体实现是requirejs define('模块id',[模块依赖1,模块依赖2],function(){  return ;}) , ajax请求文件并加载
    - Commonjs || CMD 规范seajs 淘宝玉伯
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


#### webpack.config.js文件配置
* entry 是一个对象，程序的入口
    - key:随意写
    - value: 入口文件
* output 是一个对象,产出的资源
    - key: filename
    - value : 生成的build.js
* module 模块（对象）
    - loaders:[]
        + 存在一些loader   `{ test:正则,loader:'style-loader!css-loader'    }`

#### 处理css
* import './xxx.css';
* ```import './1.jpg';```
* ![1528982951270](assets/1528982951270.png)

#### 处理less

- `loader:'style-loader!css-loader!less-loader'`

#### 处理ES6
- babel-loader + babel-preset-env(es2015/2016/2017)

#### 处理文件 + base64
* url-loader 可以将文件生成为base64编码，到build.js中
* 文件在base64加密后会比原来大三分之一
* 应用场景是比较小的图片 4kb以内的图片

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

* --open 自动打开浏览器
* --hot 热替换，  不在刷新的情况下替换，css样式
* --inline 自动刷新
* --port 9999 指定端口
* --process 显示编译进度



#### 包的分类管理和分类恢复

* 1: 安装包的时候，做一个分类的管理   
  * 开发依赖（打包相关webpack） npm i 包名 -D  ->      devDependencies
  * 生产依赖（不包含webpack打包依赖） npm i  包名 -S  dependencies
* 2: 恢复依赖
  * 如果包文件不小心删了，少了
  * 开发恢复 ```npm i ```
  * 生产恢复 ```npm i --production```

