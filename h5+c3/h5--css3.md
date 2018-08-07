####  一. 属性选择器

1. 查找拥有style属性的li ,  ```E[attr]```,查找拥有attr属性的E标签

   ``````css
   li[style]{
   	color:#fff
   }
   ``````

2. ```E[attr=value]```,查找拥有指定的Attr属性并且属性值为value的E标签

   ``````css
   li[class=red]{}//严格匹配
   ``````

3. ```E[attr*=value]```,查找拥有指定的Attr属性并且属性中包含（可以在任意的位置）为value的E标签

   `````css
   li[class*=red]{}//包含
   `````

4. ```E[attr^=value]```,查找拥有指定的Attr属性并且属性值以value开头的E标签

   ```css
   li[class^=red]{}//以value值开头的
   ```

5. ```E[attr$=value]```,查找拥有指定的Attr属性并且属性值以value结尾的E标签

   ```css
   li[class$=red]{}//以value值结尾的
   ```

####  二. CSS3

1. 文本阴影

   ````css
   text-shadow:offsetX offsetY blur(模糊值) color 
   ````

2. 边框阴影

   ```css
   box-shadow: h v blur(模糊) spread(扩展--阴影的尺寸) color inset(内阴影)
   阴影可以多次添加
   ```

####  三.伪类选择器（结构伪类）

1. 兄弟伪类

   > +号 获取当前元素相邻的的满足条件的元素   

   eg: 添加了.first样式的标签的相邻的li元素

   ````css
   .first + li {}
   ````

   > ~ 获取当前元素的所有兄弟元素

   ````css
   .first ~ li {//必须是指定类型的元素 eg:li}
   ````

2. 相对于父元素的伪类

   -  E:first-child — 查找E元素的父级元素中的第一个E元素（不过滤其他类型的元素）

     ````css
     li:first-child{//查找li元素中的第一个li元素}
     ````

   - E:last-child 查找E元素的父元素中的最后一个指定类型的子元素（不过滤其他类型的元素）

     ````css
     li:last-child{//查找li元素中的最后一个li元素}
     ````

   - **查找时候 first-of-type（限制类型）**

     > 1. 也是相对与父元素 2. 在查找的时候只会找到满足类型条件的元素，过滤掉其他类型的元素

     ```css
     li:first-of-type{
         color:red
     }
     li:last-of-type{}
     ```

   - 指定索引的位置 nth-child（从1开始的索引 || 关键字 || 表达式）

      + 索引

        ````css
        li:nth-child(5){}
        ````

     + 关键字(even/odd)

       ````css
       li:nth-child(even){}//偶数
       li:nth-child(odd){}//奇数
       ````

     + 表达式

       ````css
       想为前面的5个元素添加样式
       n是默认取值范围为0~~~子元素的长度，但是当n<=0,选取无效
       li:nth-last-of-type(-n+5){}//后5个
       li:nth-of-type(-n+5){}//前5个
       
       //空值：没有任何的内容，连空格都没有
       li:empty{}
       ````

   - **type（限制类型）**

     ````css
     li:nth-of-type(even){}
     li:nth-of-type(odd){} 
     ````

####  四.伪类选择器（其他）

 - E:target 可以为锚点目标元素添加样式，当目标元素被触发为当前锚链接的目标时候，调用此伪类样式

   ````css
   h2:target{color:red}
   ````

####  五.伪元素选择器

 1. 是一个行内元素，如果想设置宽高则需要转换成块：display：block ；float：** position

 2. 必须添加content，哪怕不设置内容，也需要content：‘’‘’

 3. E:after、E:before在旧版本里面是伪类，新版本里面是伪元素，新版本下 E:after 会被自动识别为E::after，按伪元素来对待，这样做的目的是兼容处理

 4. ![image-20180801145940564](/var/folders/jp/36l2pbb13xx5jqwst3cdk_sr0000gn/T/abnerworks.Typora/image-20180801145940564.png)

	5.  

    - E::first-letter (获取第一个字符)

    `````css
    P::first-letter{
        color:red;
        font-size:30px;
        float:left;//文本环绕
    }
    `````

    - 获取第一行内容，如果设置了::first-letter，那么就无法同时设置它的样式

      ````css
      p::first-line{
          text-decoration:undeline
      }
      ````

    - 设置当前选中的内容的样式

      ````css
      P::selection{
          color:red;
      }
      ````



#### 六.颜色

1. 透明度
   - opacity 只能针对整个盒子设置透明度，子盒子及内容会继承父盒子的透明度
   - Transparent 不可调节透明度，始终完全透明
   - 使用rgba来控制颜色，相对opacity不具有继承性













