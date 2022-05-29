# PC端网页特效

## 1. 元素偏移量offset

### 1.1 offset概述

使用 offset 系列相关属性可以动态的得到该元素的位置（偏移）、大小等。

+ 获得元素距离带有定位父元素的位置 
+  获得元素自身的大小（**宽度高度**） 
+ 注意： 返回的数值都<font color='red'>**不带单位**</font>

常用属性

![image-20220313102340047](https://gitee.com/cws201800115/typora-images/raw/master/image-20220313102340047.png)

### 1.2 offset与style区别

#### offset特点

+ offset包含：padding+border+width
+ offset是只读属性，只能获取不能赋值（不带单位）

#### style特点

+ 不包含padding+border
+ style可读属性，可赋值

### ♥ 案例 放大镜放大镜放大镜



![放大镜](C:\Users\cws\Desktop\放大镜.gif)

```javascript
window.addEventListener('load', function() {
    var preview_img = document.querySelector('.preview_img');
    var mask = document.querySelector('.mask');
    var big = document.querySelector('.big');
    // 1. 当我们鼠标经过 preview_img 就显示和隐藏 mask 遮挡层 和 big 大盒子
    preview_img.addEventListener('mouseover', function() {
        mask.style.display = 'block';
        big.style.display = 'block';
    })
    preview_img.addEventListener('mouseout', function() {
            mask.style.display = 'none';
            big.style.display = 'none';
        })
        // 2. 鼠标移动的时候，让黄色的盒子跟着鼠标来走
    preview_img.addEventListener('mousemove', function(e) {
        // (1). 先计算出鼠标在盒子内的坐标
        var x = e.pageX - this.offsetLeft;
        var y = e.pageY - this.offsetTop;
        // console.log(x, y);
        // (2) 减去盒子高度 300的一半 是 150 就是我们mask 的最终 left 和top值了
        // (3) 我们mask 移动的距离
        var maskX = x - mask.offsetWidth / 2;
        var maskY = y - mask.offsetHeight / 2;
        // (4) 如果x 坐标小于了0 就让他停在0 的位置
        // 遮挡层的最大移动距离
        var maskMax = preview_img.offsetWidth - mask.offsetWidth;
        if (maskX <= 0) {
            maskX = 0;
        } else if (maskX >= maskMax) {
            maskX = maskMax;
        }
        if (maskY <= 0) {
            maskY = 0;
        } else if (maskY >= maskMax) {
            maskY = maskMax;
        }
        mask.style.left = maskX + 'px';
        mask.style.top = maskY + 'px';
        // 3. 大图片的移动距离 = 遮挡层移动距离 * 大图片最大移动距离 / 遮挡层的最大移动距离
        // 大图
        var bigIMg = document.querySelector('.bigImg');
        // 大图片最大移动距离
        var bigMax = bigIMg.offsetWidth - big.offsetWidth;
        // 大图片的移动距离 X Y
        var bigX = maskX * bigMax / maskMax;
        var bigY = maskY * bigMax / maskMax;
        bigIMg.style.left = -bigX + 'px';
        bigIMg.style.top = -bigY + 'px';

    })

})
```



## 2. 元素可视区client

![image-20220313104148386](https://gitee.com/cws201800115/typora-images/raw/master/image-20220313104148386.png)

client 宽度 和我们offsetWidth **最大的区别**就是 **不包含边框**

## 3. 元素scroll属性

![image-20220313104346897](https://gitee.com/cws201800115/typora-images/raw/master/image-20220313104346897.png)

### 3.1 元素scroll系列属性

scroll 翻译过来就是滚动的，我们使用 scroll 系列的相关属性可以动态的得到该元素的大小、滚动距离等

+ offestTop 就是被卷去头部的大小
+ **window.pageYOffset** 获得元素被卷去头部高度

### ♥ 案例 返回顶部

![返回顶部](https://gitee.com/cws201800115/typora-images/raw/master/%E8%BF%94%E5%9B%9E%E9%A1%B6%E9%83%A8.gif)

```javascript
  //1. 获取元素
        var sliderbar = document.querySelector('.slider-bar');
        var banner = document.querySelector('.banner');
        // banner.offestTop 就是被卷去头部的大小 一定要写到滚动的外面
        var bannerTop = banner.offsetTop
            // 当我们侧边栏固定定位之后应该变化的数值
        var sliderbarTop = sliderbar.offsetTop - bannerTop;
        // 获取main 主体元素
        var main = document.querySelector('.main');
        var goBack = document.querySelector('.goBack');
        var mainTop = main.offsetTop;
        // 2. 页面滚动事件 scroll
        document.addEventListener('scroll', function() {
                // console.log(11);
                // window.pageYOffset 页面被卷去的头部
                // console.log(window.pageYOffset);
                // 3 .当我们页面被卷去的头部大于等于了 172 此时 侧边栏就要改为固定定位
                if (window.pageYOffset >= bannerTop) {
                    sliderbar.style.position = 'fixed';
                    sliderbar.style.top = sliderbarTop + 'px';
                } else {
                    sliderbar.style.position = 'absolute';
                    sliderbar.style.top = '300px';
                }
                // 4. 当我们页面滚动到main盒子，就显示 goback模块
                if (window.pageYOffset >= mainTop) {
                    goBack.style.display = 'block';
                } else {
                    goBack.style.display = 'none';
                }

            })
            // 3. 当我们点击了返回顶部模块，就让窗口滚动的页面的最上方
        goBack.addEventListener('click', function() {
            // 里面的x和y 不跟单位的 直接写数字即可
            // window.scroll(0, 0);
            // 因为是窗口滚动 所以对象是window
        });
```

## 4. 三大系列总结

![image-20220313105528075](https://gitee.com/cws201800115/typora-images/raw/master/image-20220313105528075.png)

### 4.1 用法区别

+ offset系列 经常用于获得<font color='red'>元素位置</font>` offsetLeft offsetTop `
+  client 经常用于获取<font color='red'>元素大小</font> `clientWidth clientHeight `
+  scroll 经常用于获取<font color='red'>滚动距离</font> `scrollTop scrollLeft  `
+ 注意<font color='red'>页面滚动的距离</font>通过 `window.pageXOffset `获得

## 5. mouseover和mouseenter

#### mouseover事件：

 mouseover 鼠标经过自身盒子会触发，经过子盒子还会触发。

#### mouseenter事件：

+ mouseenter 只会经过自身盒子触发**<font color='red'>（不会冒泡）</font>**
+ mouseleave也不会冒泡

## 6. 节流阀

+ **作用**：防止轮播图按钮连续点击造成播放过快。 

+ **目的**：当上一个函数动画内容执行完毕，再去执行下一个函数动画，让事件无法连续触发。
+  **核心实现思路**：利用回调函数，添加一个变量来控制，锁住函数和解锁函数。 开始设置一个变量 `var flag = true; If(flag) {flag = false; do something} `关闭水龙头 利用回调函数 动画执行完毕，

## ！♥ ！综合案例：烦躁的轮播图 

### 1. 效果

![轮播图4](https://gitee.com/cws201800115/typora-images/raw/master/%E8%BD%AE%E6%92%AD%E5%9B%BE4.gif)

### 2. 要求

1. 每隔两秒自动播放
2. 鼠标经过轮播图模块，左右按钮显示，离开隐藏左右按钮
3. 点击下方小圆圈切换图片，点击左右按钮切换图片
4. 从最后一张图无缝转换到第一张图

### 3. 尝试

在使用==排他思想==对下方小圆圈进行“变白色”处理时，用==事件委派==而不是==for循环==的方式实现

```javascript
circle.addEventListener('click', function (e) {
        for (var i = 0; i < ul.children.length; i++) {
             circle.children[i].className = '';
         }
         e.target.className = 'current';
     })
```

### 4. 完整js代码

```javascript
window.addEventListener('load', function() {
    // 1. 获取元素
    var arrow_l = document.querySelector('.arrow-l');
    var arrow_r = document.querySelector('.arrow-r');
    var focus = document.querySelector('.focus');
    var focusWidth = focus.offsetWidth;
    // 2. 鼠标经过focus 就显示隐藏左右按钮
    focus.addEventListener('mouseenter', function() {
        arrow_l.style.display = 'block';
        arrow_r.style.display = 'block';
        clearInterval(timer);
        timer = null; // 清除定时器变量
    });
    focus.addEventListener('mouseleave', function() {
        arrow_l.style.display = 'none';
        arrow_r.style.display = 'none';
        timer = setInterval(function() {
            //手动调用点击事件
            arrow_r.click();
        }, 2000);
    });
    // 3. 动态生成小圆圈  有几张图片，我就生成几个小圆圈
    var ul = focus.querySelector('ul');
    var ol = focus.querySelector('.circle');
    // console.log(ul.children.length);
    for (var i = 0; i < ul.children.length; i++) {
        // 创建一个小li 
        var li = document.createElement('li');
        // 记录当前小圆圈的索引号 通过自定义属性来做 
        li.setAttribute('index', i);
        // 把小li插入到ol 里面
        ol.appendChild(li);
        // 4. 小圆圈的排他思想 我们可以直接在生成小圆圈的同时直接绑定点击事件
        li.addEventListener('click', function() {
            // 干掉所有人 把所有的小li 清除 current 类名
            for (var i = 0; i < ol.children.length; i++) {
                ol.children[i].className = '';
            }
            // 留下我自己  当前的小li 设置current 类名
            this.className = 'current';
            // 5. 点击小圆圈，移动图片 当然移动的是 ul 
            // ul 的移动距离 小圆圈的索引号 乘以 图片的宽度 注意是负值
            // 当我们点击了某个小li 就拿到当前小li 的索引号
            var index = this.getAttribute('index');
            // 当我们点击了某个小li 就要把这个li 的索引号给 num  
            num = index;
            // 当我们点击了某个小li 就要把这个li 的索引号给 circle  
            circle = index;
            // num = circle = index;
            console.log(focusWidth);
            console.log(index);

            animate(ul, -index * focusWidth);
        })
    }
    // 把ol里面的第一个小li设置类名为 current
    ol.children[0].className = 'current';
    // 6. 克隆第一张图片(li)放到ul 最后面
    var first = ul.children[0].cloneNode(true);
    ul.appendChild(first);
    // 7. 点击右侧按钮， 图片滚动一张
    var num = 0;
    // circle 控制小圆圈的播放
    var circle = 0;
    // flag 节流阀
    var flag = true;
    arrow_r.addEventListener('click', function() {
        if (flag) {
            flag = false; // 关闭节流阀
            // 如果走到了最后复制的一张图片，此时 我们的ul 要快速复原 left 改为 0
            if (num == ul.children.length - 1) {
                ul.style.left = 0;
                num = 0;
            }
            num++;
            animate(ul, -num * focusWidth, function() {
                flag = true; // 打开节流阀
            });
            // 8. 点击右侧按钮，小圆圈跟随一起变化 可以再声明一个变量控制小圆圈的播放
            circle++;
            // 如果circle == 4 说明走到最后我们克隆的这张图片了 我们就复原
            if (circle == ol.children.length) {
                circle = 0;
            }
            // 调用函数
            circleChange();
        }
    });

    // 9. 左侧按钮做法
    arrow_l.addEventListener('click', function() {
        if (flag) {
            flag = false;
            if (num == 0) {
                num = ul.children.length - 1;
                ul.style.left = -num * focusWidth + 'px';

            }
            num--;
            animate(ul, -num * focusWidth, function() {
                flag = true;
            });
            // 点击左侧按钮，小圆圈跟随一起变化 可以再声明一个变量控制小圆圈的播放
            circle--;
            // 如果circle < 0  说明第一张图片，则小圆圈要改为第4个小圆圈（3）
            // if (circle < 0) {
            //     circle = ol.children.length - 1;
            // }
            circle = circle < 0 ? ol.children.length - 1 : circle;
            // 调用函数
            circleChange();
        }
    });

    function circleChange() {
        // 先清除其余小圆圈的current类名
        for (var i = 0; i < ol.children.length; i++) {
            ol.children[i].className = '';
        }
        // 留下当前的小圆圈的current类名
        ol.children[circle].className = 'current';
    }
    // 10. 自动播放轮播图
    var timer = setInterval(function() {
        //手动调用点击事件
        arrow_r.click();
    }, 2000);

})
```

