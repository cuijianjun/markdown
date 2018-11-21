#### Vue 部分原理揭秘

> 笔记 20180826---note

1. 前端的3大框架  ---- vue/react/angular 

2. 主要的核心是数据，数据变化，视图随之变化 

3. 数据变化 ---- 谁变化了，怎么变了，等等一系列特征 

4. Observe方法实现的 ----代码（observe.html） 

   > https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/observe 

5. Proxy代替了  ——（proxy.html）

   > https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy

6. Proxy常用的属性

   > proxy-delete .... 

   - get 获取
   - set 设置
   - deleteProperty  删除属性
   - has    in 操作

7. 对象的数据属性

   - configurable --删除
   - enumerable -- for in 
   - writable  - 修改
   - value - 包含这个属性的数据值

   > 访问器属性

   - configurable --删除
   - enumerable -- for in 
   - get
   - set

8. 观察者——原生支持(性能最高)、高级支持      Vue
   脏检查——性能不行、全浏览器支持            Angular v1.x

9. 兼容ie6  --- 脏检查(dirty.html)

10. vue的基本使用（vue.html）

11. 实现一个自己的vue

    1. el的实质（vue-query）
    2. vue的读数据（vue-proxy-get）