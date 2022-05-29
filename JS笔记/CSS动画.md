# CSS动画

## **1. transition过渡**

- 过渡动画：是从一个状态渐渐的过渡到另外一个状态
- **过渡经常和：hover一起搭配使用**

```html
transition: 要过渡的属性 花费时间 运动曲线 何时开始
```

1. **要过渡的属性**：想要变化的 CSS 属性，宽度高度，背景颜色，内外边距都可以，如果想要所有的属性都变化过渡，写一个all就可以。
2. **花费时间**：单位是秒(必须写单位) 比如0.5s
3. **运动曲线**：默认是ease(可以省略)
4. **何时开始**：单位是秒(必须写单位)，可以设置延迟触发事件，默认是0s(可以省略)

过渡的口诀：**谁做过渡给谁加**

```css
<style>
        div {
            width: 200px;
            height: 100px;
            background-color: pink;
            /* transition: 变化的属性 花费时间 运动曲线 何时开始; */
            /* 如果想要写多个属性，利用逗号进行分割 */
            transition: width 0.5s, height 0.5s;
            /* 如果想要多个属性都变化，属性写all就可以了 */
            transition: all 0.5s;
        }
        
        div:hover {
            width: 400px;
            height: 200px;
            background-color: red;
        }
    </style>
```

![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/39577c89a4ed4f03968fe0927142901d.gif)

## **2. 2D转换**

- 移动：translate
- 旋转：rotate
- 缩放：scale

二维坐标系：

![image-20220305230913664](https://gitee.com/cws201800115/typora-images/raw/master/image-20220305230913664.png)

### 2.1 移动translate

可以改变元素在页面中的位置，**类似**定位。

```css
transform:translate(x,y); 
/* 或者分开写 */
transform:translateX(n);
transform:translateY(n);


transform:translate(100px,100px);
/* 如果只移动X轴           */
transform:translate(100px,0);
translateX(100px);

```

+ 定义2D转换中的移动，沿着X和Y轴移动元素
+ translate 最大的优点：**不会影响到其他元素的位置**
+ translate 中的百分比单位是**相对于自身元素**的
  + `translate:(50%,50%)`
+ 对**行内标签**没有效果

### 2.2 旋转rotate

2D旋转指的是让元素在2维平面内顺时针旋转或者逆时针旋转。

1. rotate 里面跟度数，单位是 deg 比如 rotate(45deg)
2. **角度为正时，顺时针，负时，为逆时针**
3. 默认旋转的中心点是元素的中心点

```html
<head>
   <style>
        img {
            width: 150px;
            border-radius: 50%;
            border: 5px solid pink;
            /* 过渡写到本身，谁做动画给谁加 */
            transition: all 0.3s;
        }
        
        img:hover {
            transform: rotate(360deg);
        }
    </style>
</head>

<body>
    <img src="#" alt="">
</body>

```

### 2.3 旋转中心点 transform-origin

2D转换中心点：我们可以设置元素转换的中心点 `transform-origin`

1. 注意后面的参数x 和 y 用空格隔开
2. x y **默认**转换的中心点是元素的中心点(50% 50%)
3. 还可以给x y 设置 像素或者方位名词(top bottom left right center)

```html
<head>
   <style>
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
            margin: 100px auto;
            transition: all 1s;
            /* 1.可以跟方位名词 ,以左下角为轴进行旋转*/
            transform-origin: left bottom;
        }
        
        div:hover {
            transform: rotate(360deg);
        }
    </style>
</head>

<body>
    <div></div>
</body>

```

### 2.4 缩放 scale

缩放：`scale`,只要给元素添加上了这个属性就能控制它放大还是缩小

1. 注意其中的x和y用逗号分割
2. `transform:scale(1,1)`: 宽和高都放大一倍，相当于没有放大
3. `transform:scale(2,2)`：宽和高都放大了2倍
4. `transform:scale(2)`：只写一个参数，第二个参数则和第一个参数一样，相当于 scale(2,2)
5. `transform:scale(0.5,0.5)`：缩小

+ **sacle缩放最大的优势：**可以设置转换中心点缩放，默认以中心点缩放的，而且不影响其他盒子

```html
<head>
   <style>
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
            margin: 100px auto;
        }
        
        div:hover {
            /* 1.里面写的数字不跟单位 就是倍数的意思 1就是1倍，2就是2倍 */
            transform: scale(2, 2);
        }
    </style>
</head>

<body>
    <div></div>
</body>

```

### 2.5  2D转换综合写法

1. 同时使用多个转换，其格式为: transform:translate() rotate() scale() 移动-旋转-缩放
2. 其顺序会影响转换的效果(先旋转会改变坐标轴方向)
3. **当我们同时有位移和其他属性时候，将位移放到最前面**

### 2.6 总结

+ 2D 移动 translate(x, y) 最大的优势是不影响其他盒子， 里面参数用%，是**相对于自身宽度和高度**来计算的
+ 可以分开写比如 translateX(x) 和 translateY(y)
+ 2D 旋转 rotate(度数) 可以实现旋转元素 度数的单位是deg
+ 2D 缩放 sacle(x,y) 里面参数是数字 不跟单位 可以是小数 最大的优势 不影响其他盒子
+ 设置转换中心点 transform-origin : x y; 参数可以百分比、像素或者是方位名词
+ **当我们进行综合写法，同时有位移和其他属性的时候，记得要将位移放到最前**

## 3. C3动画

制作动画分为两步：

- 先定义动画
- 再使用（调用）动画

### 3.1 用keyframs定义动画

```css
@keyframes 动画名称 {
   0%{
        width:100px;
   }  
   100%{
        width:200px;
   }
}
```

+ 0% 是动画的开始，100% 是动画的完成。这样的规则就是动画序列。
+ 动画是使元素从一种样式逐渐变化为另一种样式的效果。
+ 用百分比来规定变化发生的时间，或用关键词 “from” 和 “to”，等同于 0% 和 100%。
  

### 3.2 使用动画

```css
div {
       width: 200px;
       height: 200px;
       background-color: aqua;
       margin: 100px auto;
       /* 调用动画 */
       animation-name: 动画名称;
       /* 持续时间 */
       animation-duration: 持续时间;
    }

```

### 3.3 动画常用属性

![image-20220305232220363](https://gitee.com/cws201800115/typora-images/raw/master/image-20220305232220363.png)

### ♥ 案例 大数据热点图 

![热点](https://gitee.com/cws201800115/typora-images/raw/master/%E7%83%AD%E7%82%B9.gif)

```html
 <style>
        body {
            background-color: #333;
        }
        
        .map {
            position: relative;
            width: 747px;
            height: 616px;
            background: url(media/map.png) no-repeat;
            margin: 0 auto;
        }
        
        .city {
            position: absolute;
            top: 227px;
            right: 193px;
            color: #fff;
        }
        
        .tb {
            top: 500px;
            right: 80px;
        }
        
        .dotted {
            width: 8px;
            height: 8px;
            background-color: #09f;
            border-radius: 50%;
        }
        
        .city div[class^="pulse"] {
            /* 保证我们小波纹在父盒子里面水平垂直居中 放大之后就会中心向四周发散 */
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            /* 这一段的原理是首先通过top50%和left：50%使得pulse的左上角位于点的中心，然后translate的位移距离是根据自身决定的，所以在这2个的同时作用下会使点与pulse的中心重合 */
            width: 8px;
            height: 8px;
            box-shadow: 0 0 12px #009dfd;
            border-radius: 50%;
            animation: pulse 1.2s linear infinite;
        }
        
        .city div.pulse2 {
            animation-delay: 0.4s;
        }
        
        .city div.pulse3 {
            animation-delay: 0.8s;
        }
        
        @keyframes pulse {
            0% {}
            70% {
                /* transform: scale(5);  我们不要用scale 因为他会让 阴影变大*/
                width: 40px;
                height: 40px;
                opacity: 1;
            }
            100% {
                width: 70px;
                height: 70px;
                opacity: 0;
            }
        }
    </style>
</head>

<body>
    <div class="map">
        <div class="city">
            <div class="dotted"></div>
            <div class="pulse1"></div>
            <div class="pulse2"></div>
            <div class="pulse3"></div>
        </div>
        <div class="city tb">
            <div class="dotted"></div>
            <div class="pulse1"></div>
            <div class="pulse2"></div>
            <div class="pulse3"></div>
        </div>
    </div>
```



### 3.4 动画简写属性

```css
/* animation：动画名称 持续时间 运动曲线  何时开始  播放次数  是否反方向  动画起始或者结束的状态 */

animation: myfirst 5s linear 2s infinite alternate;

```

+ 简写属性里面不包含` animation-play-state`
+ 暂停动画：`animation-play-state: puased;` 经常和鼠标经过等其他配合使用
+ l想要动画走回来 ，而不是直接跳回来：`animation-direction: alternate`
+ 盒子动画结束后，停在结束位置： `animation-fill-mode : forwards`

### 3.5 速度曲线细节

- `animation-timing-function`：规定动画的速度曲线，默认是“ease”

![image-20220305232516174](https://gitee.com/cws201800115/typora-images/raw/master/image-20220305232516174.png)

### ♥ 案例 熊大？ 熊二！！

![熊大](https://gitee.com/cws201800115/typora-images/raw/master/%E7%86%8A%E5%A4%A7.gif)

```html
 <style>
        body {
            background-color: #ccc;
        }

        div {
            position: absolute;
            width: 200px;
            height: 100px;
            background: url(media/bear.png) no-repeat;
            /* 元素可以添加多个动画， 用逗号分隔 */
            animation: bear .4s steps(8) infinite, move 3s forwards;
        }

        @keyframes bear {
            0% {
                background-position: 0 0;
            }

            100% {
                background-position: -1600px 0;
            }
        }

        @keyframes move {
            0% {
                left: 0;
            }

            100% {
                left: 15%;
                /* margin-left: -100px; */
                /* transform: translateX(-50%); */
            }
        }
    </style>
```

## 4. 3D转换

3D转换的特点：

- 近大远小。
- 物体后面遮挡不可见

### **4.1 三维坐标系**

![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/8b3f76c171604b64975e75c1e3d7367b.png)

- x轴：水平向右 **注意： x 右边是正值，左边是负值**
- y轴：垂直向下 **注意： y 下面是正值，上面是负值**
- z轴：垂直屏幕 **注意： 往外面是正值，往里面是负值**

### 4.2 3D位移translate3d

**3D移动在2D移动的基础上多加了一个可以移动的方向，就是z轴方向**

+ `translform:translateX(100px)`仅仅是在x轴上移动
+ `translform:translateY(100px)：`仅仅是在Y轴上移动
+ `translform:translateZ(100px)：`仅仅是在Z轴上移动（注意translateZ一般用px单位）
+ `transform:translate3d(x,y,z)：`其中 x、y、z 分别指要移动的轴的方向的距离

所以默认是看不到元素在z轴的方向上移动

### 4.3 透视perspective

透视：在2D平面产生近大远小视觉立体，但是只是效果二维的

在网页产生3D效果需要透视（理解成3D物体投影在2D平面内）

+ 透视也称为视距：视距就是人的眼睛到屏幕的距离
+ 距离视觉点越近的在电脑平面成像越大，越远成像越小
+ 透视的单位是像素
+ 透视写**在被观察元素的父盒子上面**的

#### 1. translateZ

- translform:translateZ(100px)：仅仅是在Z轴上移动。
- 有了透视，就能看到translateZ 引起的变化了
  - translateZ：近大远小
  - translateZ：往外是正值
  - translateZ：往里是负值

![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/fc804561fd5d4f6f892404c3df05dde6.gif)

### 4.4 3D旋转rotate3d

3D旋转：3D旋转指可以让元素在三维平面内沿着 x轴，y轴，z轴或者自定义轴进行旋转。

+ `transform: rotateX(45deg) ：`沿着X轴正方向旋转45度
+ `transform: rotateY(45deg) ：`沿着Y轴正方向旋转45度
+ `transform: rotateZ(45deg) `：沿着Z轴正方向旋转45度

+ `transform: rotate3d(x,y,z,deg) `：沿着自定义轴旋转 deg为角度(了解即可)

```css
/*沿着X轴旋转45deg*/
transform: rotate3d(1,0,0,45deg) 
/*沿着对角线旋转45deg*/
transform: rotate3d(1,1,0,45deg) 
```



xyz是表示旋转轴的矢量，是标示你是否希望沿着该轴旋转，最后一个标示旋转的角度。



####  ♥ 左手准则

+ 左手的手拇指指向 x轴的正方向
+ 其余手指的弯曲方向就是该元素沿着x轴旋转的方向

### 4.5 3D呈现transform-style

3D呈现：**transform-style**

+ 控制子元素是否开启三维立体环境

+ `transform-style: flat `子元素不开启3d立体空间 默认的

+ `transform-style: preserve-3d` 子元素开启立体空间

+ **代码写给父级**，但是影响的是子盒子(<span style='color:red'>**不是后代元素!!**</span>)

+ <span style='color:red'>**透视和3D呈现不能加在同一级，透视在3D上一级盒子加**</span>

  

![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/8d5527a9b56c430e924853c5669c4d2c.png)

### ♥ 案例 翻转吧 老头！！

![老头](https://gitee.com/cws201800115/typora-images/raw/master/%E8%80%81%E5%A4%B4.gif)

```html
<style>
        body {
            perspective: 400px;
        }

        .box {
            position: relative;
            width: 300px;
            height: 300px;
            margin: 100px auto;
            transition: all .4s;
            /* 让背面的紫色盒子保留立体空间 给父级添加的 */
            transform-style: preserve-3d;
        }

        .box:hover {
            transform: rotateY(180deg);
        }

        .front,
        .back {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            border-radius: 50%;
            font-size: 30px;
            color: #fff;
            text-align: center;
            line-height: 300px;
        }

        .front {
            background-color: #666;
            z-index: 1;
        }

        .back {
            background-color: purple;
            /* 像手机一样 背靠背 旋转 */
            transform: rotateY(180deg);
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="front">今天老头在家</div>
        <div class="back">今天老头不在家</div>
    </div>
```

## ♥ 综合案例 旋转的猪狗

![狗和猪](https://gitee.com/cws201800115/typora-images/raw/master/%E7%8B%97%E5%92%8C%E7%8C%AA.gif)

```html
    <style>
        body {
            perspective: 1000px;

        }

        .box {
            position: relative;
            /* perspective: 1000px; */
            margin: 200px auto;
            width: 300px;
            height: 200px;
            background: url(media/pig.jpg) no-repeat;
            transform-style: preserve-3d;
            transform: rotateY(20deg);
            /* animation: name duration timing-function delay iteration-count direction fill-mode; */
            animation: rotate 6s linear infinite;
            /* transform: rotateY(45deg); */
        }

        .box div {
            position: absolute;
            top: 0;
            left: 0;
            width: 300px;
            height: 200px;
            background: url(media/dog.jpg);
        }

        .box:hover {
            animation-play-state: paused;
        }

        .c1 {
            transform: translateZ(400px);
            z-index: 3;
        }

        .c2 {
            transform: translateX(400px) rotateY(-90deg);
        }

        .c3 {
            transform: translateZ(-400px);
        }

        .c4 {
            transform: translateX(-400px) rotateY(90deg);
        }

        @keyframes rotate {
            0% {
                transform: rotateY(0);
            }

            100% {
                transform: rotateY(360deg);
            }

        }
    </style>

</head>

<body>
    <div class="box">
        <div class="c1">
        </div>
        <div class="c2">
        </div>
        <div class="c3"></div>
        <div class="c4"></div>
    </div>
</body>
```

