#### 经典动画算法名称
+ Linear：线性匀速运动效果；
+ Quadratic：二次方的缓动（t^2）；
+ Cubic：三次方的缓动（t^3）；
+ Quartic：四次方的缓动（t^4）；
+ Quintic：五次方的缓动（t^5）；
+ Sinusoidal：正弦曲线的缓动（sin(t)）；
+ Exponential：指数曲线的缓动（2^t）；
+ Circular：圆形曲线的缓动（sqrt(1-t^2)）；
+ Elastic：指数衰减的正弦曲线缓动；
+ Back：超过范围的三次方缓动（(s+1)*t^3 – s*t^2）；
+ Bounce：指数衰减的反弹缓动。


#### 算法公式

- easeIn：从0开始加速的缓动，也就是先慢后快；
- easeOut：减速到0的缓动，也就是先快后慢；
- easeInOut：前半段从0开始加速，后半段减速到0的缓动。