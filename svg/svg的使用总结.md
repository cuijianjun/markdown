#### svg的使用总结

##### 1. CSS 属性-webkit-tap-highlight-color的理解

1. -webkit-tap-highlight-color
   这个属性只用于iOS (iPhone和iPad)。当你点击一个链接或者通过Javascript定义的可点击元素的时候，它就会出现一个半透明的灰色背景。要重设这个表现，你可以设置-webkit-tap-highlight-color为任何颜色。
   想要禁用这个高亮，设置颜色的alpha值为0即可。
   示例：设置高亮色为50%透明的红色:

```css
-webkit-tap-highlight-color: rgba(255,0,0,0.5);
```

浏览器支持: 只有iOS(iPhone和iPad)。

2. -webkit-text-size-adjust  

有些字体大小的Safari浏览器上呈现较大（iPhone）

```css
示例：-webkit-text-size-adjust: none;
```

3. 上述两个属性在svg里面直接这样用是不行的，必须以行内的形式写入才可以生效

##### 2. 对于点击范围的扩大

1. 由于时间和资料的有限，目前没有想到在鼠标模拟点上下的办法，目前没有属性直接扩大元素的点击范围
2. 比较实用的办法是： 在目标元素的周围生成一个透明的遮罩，点击事件直接触发遮罩，然后响应点击事件，能扩大点击响应的范围（svg是新建path）
3. 办法借鉴上面的，用一个父div包着目标元素，给父元素上添加事件，去响应，也能达到同样的效果

##### 3.关于像素点缩小内存的优化

>  场景是用svg/canvas做画笔，需要实时和native/服务器交互

- 难点：点密集，数据量就会大，导致服务器和native都会对性能是个很大的考验，为了保证实时的传输和良好的用户体验（实时同步，减少流量的消耗）
- 解决办法：
  1. 在svg画线如果原本用三次贝塞尔曲线，可以改成两次贝塞尔曲线，如果原来就是两次，可以直接在循环点的时候，设计计数器，每3到4次（看项目要求）渲染一次曲线，能使线的点数降低至2/5,而且基本保证曲线不会出现毛刺（棱角),这样达到了前面提到的目的
  2. 如果技术过关的话，直接和服务器/native传输使用坐标，能及时的传输

##### 4. svg的button点击效果

https://www.html5tricks.com/demo/svg-css3-ripple-button/index.html

##### 5. 解析path...

解析svg里面的path 需要创建的svg标签，而不是div标签

##### 6. **鼠标滑过事件**

https://www.html5tricks.com/demo/svg-border-animate-button/index.html

##### 7. 查找dom元素（虚拟dom）

在html插入svg时，会有父子节点的互相查找，定位，但是svg的object标签生成的会有自己的document，现有的方法不方便查找，我们可以直接在父子元素添加自定义属性，eg:this.parent = document.createElementNS('XXX','svg'),这样在查找的时候就会直接跳转，可以说事虚拟dom的雏形

##### ios兼容的问题

1.使用所有的包，必须经过babel编译过后的才能直接调用（尤其是我们自己的同事写过得包）







