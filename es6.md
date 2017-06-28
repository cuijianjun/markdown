## es6 基础语法

#### let 特性

1. 不允许重复申明
2. 没有预解析
3. 块级作用域

#### const

1. 在定义之后，值是固定不变的
2. 常量的值不能修改，但是如果常量保存的是一个对象，那么对象的属性是可以被修改的
3. 对象冻结
```
 const person = Object.freeze({
    name:"zhangsan",
    age :30
})
var constantize= (obj)=>{//对象和key都冻结
    Object.freeze(obj);
    Object.keys(obj).forEach((key,value)=>{
        if(typeof obj[key] === 'object'){
            constantize(obj[key]);
        }
    })
}
```
#### 解构赋值

1. 按照对应的顺序进行解构【数组】
2. 根据对应的名称进行解构【对象】

#### 字符串

1. str.at()返回对应位置的字符
2. s.codePointAt(0)//返回码点
3. String.fromCodePoint(134071)//根据码点返回字符
4. s.repeat(10)//复制10次
5. str.includes()

 - 参数：
   - 要查找的字符串
   - 起始位置
   - 返回布尔值，表示是否找到参数字符串
6. str.startswith() 起始位置
7. str.endswith( ) 结束位置

#### Math对象的扩展
- Math.trunc()
 -  去除小数部分，返回整数部分
- Math.sign()
   - 判断一个数字是整数还是负数还是 （正0还是负 0 ）
- Math.hypot(...values)
   - 返回所有参数的平方和的平方跟（勾股定理）

##### 数组的扩展

1. 类数组转化为真数组（包括字符串）
   - [].slice.call();
   - Array.from();
2. 把参数变为数组
   - Array.of(1,2,3,4,5)
3. 找出第一个符合条件的数组元素
  - arr.find()
  - 遍历整个数组，遍历过程中调用函数，如果回调函数返回true，则返回当前元素，如果所有条件都不符合，返回undefind
  -  参数：
      - 回调函数
      - 回调函数内this的指向
4. 找出第一个符合条件的数组元素的位置
  - arr.findIndex()
  - 同上 如果所有元素都不符合条件，则返回-1;
5. arr.fill()
  - 用来填充数组
  - 参数
     -  填充的内容
     -  起始位置
     - 结束位置
6. for-of
 - 可以遍历数组，字符串，【有遍历接口】

 ```
 for(var key of arr.keys()){//key值遍历接口
  }
  for(var key of arr.values()){//value值遍历接口
  }
  for(var key of arr.entries()){//key值和value值遍历接口
  }
 ```

7. 数组推导[字符串也可以使用]
> 允许直接通过现有的数组形成新的数组

- 例一
```
var arr  = [1,2,3,4]
var arr2= [for (i of arr1) i*2]
```

- 例二

`var arr2 = [for (i of arr1 ) if(i>2) i ]`

#### 对象的扩展

1. 属性的简洁表示法--直接返回对象

```
function(){
x++;y++
return {x+y};
}
```
2. 方法的简洁表示法

```
var obj ={
   name:"aa";
    showname(){
       return this.name;
   }
}
```

3. 属性名表达式

```
let person = {
      name:"momo",
      [sex] :false,
      ["get"+"name"](){
         alert(this.name)
      }
   }
```

4. Object.is();

> 判断传入的参数是否相等

5. Object.assign(target,[source1,source2....]);

> 将source对象上的可枚举的属性赋值到target上，如果有同名属性，则会被后面的覆盖前面的

6. Object.getPrototypeOf();

> 获取一个对象的prototype

7. Object.setPrototypeOf();

> 设置一个对象的prototype

8. Proxy

> 用于修改一些默认行为

- 参数
  - 1. 要代理的目标对象
  - 2. 设置对象

```
var obj = {
    a:1,
    b:2
}

var p1= new Proxy(obj,{
    get(obj,attr){//当属性被访问时触发
        console.log(obj,attr);
        return obj[attr];
    },
    set(obj,attr,value){//当属性被设置时
        console.log(obj,attr,value);
        obj[attr] = value;
        return 1;
    }
})
```
9. Object.observe(obj,observe,[eventType]);

> 用于检测对象的变化，用一旦发生变化就会调用回调函数

- 参数：
   - 需要监控的对象
   - 回调函数（接收一个数组参数）
   - 指定事件
- eventType

   - add: 添加属性
   - update： 属性值的变化
   - delete：删除属性
   - setPrototype : 设置属性
   - reconfigure：属性的attributes对象的变化

```
var obj = {
    a:1,
    b:2
};
Object.observe(obj,function(a){
    onsole.log(a);
},"update");

obj.a = 3;
```

#### 函数的扩展

1. 函数参数的默认值

```
function fn(a,b=1){}
funjction fn({a,b=1}){}
```

> 定义的默认参数必须在尾参数，因为定义后该参数可以忽略

2. rest参数
> 用于获取参数的多余参数

`function fn(a,b=1,...变量名){}`

- reat后面不能有其他参数，否则报错

3. 扩展运算符（...）
> 将一个数组转为用逗号分隔得参数序列，主要用于函数调用

> 求最大值

```
var a= [1,2,3,4,5,6];
console.log(Math.max(...a));

var str = "cuijianjun";
var arr = [...str];
```

4. 箭头函数

> 用来做回调函数使用

`var f= (a,b)=>{a+b};`

- 函数体内的this对象，绑定定义时所在的对象，而不是使用时所在的对象
- 不可以当作构造函数，不可以使用new命令，否则会抛出一个错误
- 该函数体内不存在arguments

5. name

> function bread( ){}  console.log(bread.name)//bread

6. 格式写法

> 举例

    let a = "1",b= "2";
    let food= {
        a,
        b,
        breakfast(){}
    }


#### 数据结构(set)

1. set（[array]）;
> 它类似于数组，但成员的值都是唯一的，没有重复的值，Set是一个构造函数，可以传入一个数组初始化默认值

`var set = new Set([1,2,2,3])`

2. set.size()

> set的实例的成员的个数（数组的长度）

3. set.add(value);
> 为set的实例的值添加数值

4. set.delete(value);

> 删除set实例的值

5. set.has(value);

> 判断传入的参数是否为set的成员

6.set.clear()
> 清除set中的所有成员

#### 数据结构(map)

> 它类似于对象，是键值对集合，但是键的范围不限于字符串，各种类型的值都可与当作键
mapye 可以接受一个数组作为参数，该数组的成员是一个个表示键值对的数组

- [[a,b],[a1,b1]]

1. map.size() 成员总数
2. map.get(key)
3. map.set(key,value)
4. map.has(key)
5. map.clear()

#### 数据结构Symbol

> 表示独一无二的ID，它通过Symbol函数生成

```
var obj = {
    a:1,
    b:2,
    c:3
}
var s1 = Symbol("test");
var s2 = Symbol("test");
console.log(s1 == s2);
Object.prototype[Symbol.iterator] = function(){
    var keys= Object.keys(this);
    var _self = this;
    var index = 0;
    return {
        next(){
        if(index < keys.length){
            return {value :_self[keys[index++]],done:false}
        }else{
             return {value:undefined,done:false}
        }
       }
    }
}

for(var value of obj){
    console.log(value);
}
```
#### Generator函数

> 函数内部的遍历器，作用是完全控制函数内部状态的变化，依次遍历这些状态

```
function* fn(){
    yield 1;
    yield 2;
}
var f= fn();
console.log(f.next());//1
console.log(f.next());//2
console.log(f.next());//undefined
```

#### promise对象

> promise对象是一个构造函数，用来生成promise实例；代表未来某个将要发生的事件（异步操作）
   - 好处：可以将异步以同步操作的形式表达出来，避免层层嵌套的回调函数

```
var p1 = new Promise(function(resolve,reject){
    setTimeout(function(){
        resolve();
    }，500)
});
p1.then(function(){},function(){}).catch(function(){})
```

1. Promise.all()

> Promise.all方法用于将多个Promise实例，包装成一个新的Promise实例。。
    参数要求拥有iterator接口，并且每一项都是promise实例
    var p = Promise.all([p1,p2,p3])
    p的状态由p1、p2、p3决定，分成两种情况。
 - 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
 - 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

```
var p1 = new Promise(function(resolve,reject){
    setTimeout(function(){
        resolve();
    }，500)
});

var p2 = new Promise(function(resolve,reject){
    setTimeout(function(){
        resolve();
    }，500)
});

var p3 = new Promise(function(resolve,reject){
    setTimeout(function(){
        resolve();
    }，500)
});

var p4= Promise.all([p1,p2,p3]);//前三个都成功，p4成功--- 有一个失败，p4失败
p4.then(function(){}，function(){})
```

2. Promise.race()
>
    与Promise.all方法类似将多个promise包装成一个新的promise实例
    但是其中有一项的状态发生改变新的实例的状态就会随着改变。

    var p = Promise.race([p1,p2,p3])
    p的状态由p1、p2、p3决定，分成两种情况。

- 只要p1、p2、p3中一个的状态变成fulfilled，p的状态就变成fulfilled
- 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

3. async函数

>
    只要函数名之前加上async关键字，就表明该函数内部有异步操作。该异步操作应该返回一个Promise对象，前面用await关键字注明。当函数执行的时候，一旦遇到await就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句。
    例如：
        async function fn(){
            let data = await ajax ();
            return data;
        }

#### class

> 在constructor方法内，super指代父类的constructor方法；在其他方法内，super指代父类的同名方法。

```
对比
"use strict"
class Cat{
    constructor(name){
        this.name = name;
    }

    getName(){
        return this.name;
    }
    //静态方法
    static cook(food){
        console.log(food)
    }
}
Cat.cook("a")// 不用实例化即可调用

// var Cat = function(name){
// 	this.name = name;
// }
// Cat.prototype.getName = function(){
// 	return this.name;
// }
```
##### super

```
let breakfast = {
    getDrink(){
        return "coffee"
    }
}
let sunday  = {
    __proto__:breakfast;
    getDrink(){
        return super.getDrink()//在其他方法内，super指代父类的同名方法。
    }
}
```

```
继承

class Momo extends Cat{
		constructor(name,color){
			super(name);
			this.color = color;
		}
		showColor(){
			return this.color;
		}
	}
	var c1 = new Cat('leo');
	var m1 = new Momo('momo','yellow');

	// console.log(m1.getName());
	console.log(m1.showColor());
	//console.log(c1.getName())
```

#### module

> es6所提供的模块化

1. export命令,export命令用于用户自定义模块，规定对外接口

> export var name = 'leo';export {name1,name2}

2. import命令--import命令用于输入其他模块提供的接口

> import {nam1,name2} from '文件路径';输入的名称必须与输出的相同

3. as关键字 --- 为输入的变量换一个新的名字

> import { name as n } from '路径';
    整体输入模块 : import * as 变量名 from '路径';
    module 变量名 from '路径';输入的模块定义在变量名上
4.  export default --- 输出匿名函数（默认）

##### 	语法：
   	export default function(){};
   	在输入的时候可以使用任何名字指向该匿名函数
   	例如：
   	import name from '路径'；


5. 模块的继承
> 语法
  1. export * from '模块路径'
  2. 输出模块中所有方法和属性
  3. export { a as b} from '模块路径'
  4. 将模块中的a变量转为b输出
