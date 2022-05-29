# BOM

## 1. BOM概述

+ 浏览器对象模型
+ 提供了独立于内容而与浏览器窗口进行交互的对象

![image-20220309133758054](https://gitee.com/cws201800115/typora-images/raw/master/image-20220309133758054.png)

### 1.1 BOM的构成

![img](https://gitee.com/cws201800115/typora-images/raw/master/5c83bf307ec9486687a5f52312943ecb.png)

+ BOM 比 DOM 更大。它包含 DOM。

+ <font color='red'>window 对象是浏览器的顶级对象，它具有双重角色</font>

+ 它是 JS 访问浏览器窗口的一个接口

+ 它是一个全局对象。定义在全局作用域中的变量、函数都会变成 window 对象的属性和方法

+ 在调用的时候可以省略 window，前面学习的对话框都属于 window 对象方法，如 alert()、prompt()等。

**<font color='red'>注意：</font>**window下的一个特殊属性 window.name

```javascript
// 定义在全局作用域中的变量会变成window对象的属性
var num = 10;
console.log(window.num);
// 10

// 定义在全局作用域中的函数会变成window对象的方法
function fn() {
    console.log(11);
}
console.fn();
// 11

var name = 10;  //不要用这个name变量,window下有一个特殊属性window.name
console.log(window.num);

```

## 2. window对象常见事件

### 2.1 窗口加载事件

`window.onload`是窗口（页面）加载事件，当文档内容完全加载完成会触发该事件（包括图像，脚本文件，CSS文件等），就调用的处理函数。

```javascript
window.onload = function(){
    
};

// 或者
window.addEventListener("load",function(){});

```

注意 

+ 有了`window.onload`就可以把JS代码写到页面元素的上方
+ 因为`onload`是等页面内容全部加载完毕，再去执行处理函数
+ `window.onload `传统注册事件方式，只能写一次
+ 如果有多个，会以最后一个`window.onload`为准
+ <font color='red'>**如果使用`addEventListener` 则没有限制**</font>

```javascript
document.addEventListener('DOMContentLoaded',fuction(){})
```



- `DOMCountentLoaded`**事件触发时，仅当DOM加载完成，不包括样式表，图片，flash等等**
- 如果页面的图片很多的话, 从用户访问到onload触发可能需要较长的时间

#### 2.1.1 区别

- `load`等页面内容全部加载完毕，包括页面dom元素，图片，flash，css等
- `DOMContentLoaded` 是DOM加载完毕，不包含图片 flash css 等就可以执行，加载速度比load更快一些

```html
<script>
  
    window.addEventListener('load', function() {
        var btn = document.querySelector('button');
        btn.addEventListener('click', function() {
            alert('点击我');
        })
    })
    window.addEventListener('load', function() {

        alert(22);
    })
    document.addEventListener('DOMContentLoaded', function() {
            alert(33);
        })
        // load 等页面内容全部加载完毕，包含页面dom元素 图片 flash  css 等等
        // DOMContentLoaded 是DOM 加载完毕，不包含图片 falsh css 等就可以执行 加载速度比 load更快一些
</script>

```

### 2.2 调整窗口大小事件

`window.onresize` 是调整窗口大小加载事件，当触发时就调用的处理函数

```javascript
window.onresize = function() {}

// 或者
window.addEventListener('resize',function(){});

```

+ 只要窗口大小发生像素变化，就会触发这个事件
+ 我们经常利用这个事件完成响应式布局。`window.innerWidth` 当前屏幕的宽度

```html
<body>
    <script>
        window.addEventListener('load', function() {
            var div = document.querySelector('div');
            window.addEventListener('resize', function() {
                console.log(window.innerWidth);

                console.log('变化了');
                if (window.innerWidth <= 800) {
                    div.style.display = 'none';
                } else {
                    div.style.display = 'block';
                }

            })
        })
    </script>
    <div></div>
</body>

```

## 3. 定时器

- `setTimeout()`
- `setInterval()`

### 3.1 setTimeout()定时器

`setTimeout()`方法用于设置一个定时器，该定时器在定时器到期后执行调用函数。

```javascript
window.setTimeout(调用函数,[延迟的毫秒数]);
```

<font color='red'>**注意：**</font>

+ window可以省略
+ 延迟的毫秒数省略默认是0，如果写，必须是**毫秒**
+ 因为定时器可能有很多，所以我们经常给定时器赋值一个标识符
+ 普通函数是按照代码顺序直接调用，而这个函数，需要等待事件，事件到了才会去调用这个函数，因此称为**回调函数。**
  

```html
<body>
    <script>
        // 1. setTimeout 
        // 语法规范：  window.setTimeout(调用函数, 延时时间);
        // 1. 这个window在调用的时候可以省略
        // 2. 这个延时时间单位是毫秒 但是可以省略，如果省略默认的是0
        // 3. 这个调用函数可以直接写函数 还可以写 函数名 还有一个写法 '函数名()'
        // 4. 页面中可能有很多的定时器，我们经常给定时器加标识符 （名字)
        // setTimeout(function() {
        //     console.log('时间到了');
        // }, 2000);
        function callback() {
            console.log('爆炸了');
        }
        var timer1 = setTimeout(callback, 3000);
        var timer2 = setTimeout(callback, 5000);
        // setTimeout('callback()', 3000); // 我们不提倡这个写法
    </script>
</body>

```

### 3.2 clearTimeout()停止定时器

- `clearTimeout()`方法取消了先前通过调用 `setTimeout()`建立的定时器

```javascript
window.clearTimeout(timeoutID)
```

**注意**：

- `window`可以省略
- 里面的参数就是定时器的标识符

```html
<body>
    <button>点击停止定时器</button>
    <script>
        var btn = document.querySelector('button');
        var timer = setTimeout(function() {
            console.log('爆炸了');
        }, 5000);
        btn.addEventListener('click', function() {
            clearTimeout(timer);
        })
    </script>
</body>

```

###  3.3 setlnternal()定时器

+ `setInterval()`方法重复调用一个函数，每隔这个时间，就去调用一次回调函数

```javascript
window.setInterval(回调函数,[间隔的毫秒数]);
```

- `window`可以省略
- 回调函数:
  - 可以直接写函数
  - 或者写函数名
  - 或者采取字符 ‘函数名()’
- 第一次执行也是间隔毫秒数之后执行，之后每隔毫秒数就执行一次

```html
<body>
    <script>
        // 1. setInterval 
        // 语法规范：  window.setInterval(调用函数, 延时时间);
        setInterval(function() {
            console.log('继续输出');

        }, 1000);
        // 2. setTimeout  延时时间到了，就去调用这个回调函数，只调用一次 就结束了这个定时器
        // 3. setInterval  每隔这个延时时间，就去调用这个回调函数，会调用很多次，重复调用这个函数
    </script>
</body>

```

### 3.4 clearInterval()停止定时器

- `clearInterval ( )` 方法取消了先前通过调用 `setInterval()` 建立的定时器

``` html
<body>
    <button class="begin">开启定时器</button>
    <button class="stop">停止定时器</button>
    <script>
        var begin =document.querySelector('.begin');
        var stop = document.querySelector('.stop');
        var timer = null; // 全局变量  null是一个空对象
        begin.addEventListener('click', function() {
            timer = setInterval(function() {
                console.log(hahah');
            }, 1000);
        })
        stop.addEventListener('click', function() {
            clearInterval(timer);
        })
    </script>
</body>

```

### ♥ 案例 倒计时

#### 1. 简化

```html
 <div>
        <span class="hour">1</span>
        <span class="minute">2</span>
        <span class="second">3</span>
    </div>
    <script>
        // 1. 获取元素 
        var hour = document.querySelector('.hour'); // 小时的黑色盒子
        var minute = document.querySelector('.minute'); // 分钟的黑色盒子
        var second = document.querySelector('.second'); // 秒数的黑色盒子
        var inputTime = +new Date('2019-5-1 18:00:00'); // 返回的是用户输入时间总的毫秒数
        countDown(); // 我们先调用一次这个函数，防止第一次刷新页面有空白 
        // 2. 开启定时器
        setInterval(countDown, 1000);

        function countDown() {
            var nowTime = +new Date(); // 返回的是当前时间总的毫秒数
            var times = (inputTime - nowTime) / 1000; // times是剩余时间总的秒数 
            var h = parseInt(times / 60 / 60 % 24); //时
            h = h < 10 ? '0' + h : h;
            hour.innerHTML = h; // 把剩余的小时给 小时黑色盒子
            var m = parseInt(times / 60 % 60); // 分
            m = m < 10 ? '0' + m : m;
            minute.innerHTML = m;
            var s = parseInt(times % 60); // 当前的秒
            s = s < 10 ? '0' + s : s;
            second.innerHTML = s;
        }
    </script>
```

#### 2. 加入对象（尝试）

```html
 <script>
        window.addEventListener('load', function () {
            function Rdate(times) {
                var nowtime = + new Date();
                var inputtime = + new Date(times);
                var times = (inputtime - nowtime) / 1000;//得出剩余时间
                var d = parseInt(times / 60 / 60 / 24); //天数
                d = d < 10 ? '0' + d : d;
                console.log(d);
                var h = parseInt(times / 60 / 60 % 24);//小时
                h = h < 10 ? '0' + h : h;
                console.log(h);
                var m = parseInt(times / 60 % 60); //分
                m = m < 10 ? '0' + m : m;
                var s = parseInt(times % 60); //秒
                s = s < 10 ? '0' + s : s;
                // return d + '天' + h + '时' + m + '分' + s + '秒';
                function dates() {
                    this.date = d;
                    this.hour = h;
                    this.minute = m;
                    this.second = s;
                }
                var date = new dates();
                return date;
            }
            // console.log(g);
            var bigbox = document.querySelector('.bigbox');
            var i = 0;
            fn();
            setInterval(fn, 1000);
            function fn() {
                var g = Rdate('2022-3-10 19:00:00');
                bigbox.children[0].innerHTML = g.date;
                bigbox.children[1].innerHTML = g.hour;
                bigbox.children[2].innerHTML = g.minute;
                bigbox.children[3].innerHTML = g.second;
            }

        })
    </script>
</head>

<body>
    <div class="bigbox">
        <div class="c1"></div>
        <div class="c2"></div>
        <div class="c3"></div>
        <div class="c4"></div>

    </div>
```



### 3.5 this指向

+ this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁

+ 全局作用域或者普通函数中this指向全局对象window(注意定时器里面的this指向window)
+ 方法调用中谁调用this指向谁

构造函数中this指向构造函数实例:

```html
<body>
    <button>点击</button>
    <script>
        // this 指向问题 一般情况下this的最终指向的是那个调用它的对象

        // 1. 全局作用域或者普通函数中this指向全局对象window（ 注意定时器里面的this指向window）
        console.log(this);

        function fn() {
            console.log(this);

        }
        window.fn();
        window.setTimeout(function() {
            console.log(this);

        }, 1000);
        // 2. 方法调用中谁调用this指向谁
        var o = {
            sayHi: function() {
                console.log(this); // this指向的是 o 这个对象
            }
        }
        
        o.sayHi();
        var btn = document.querySelector('button');
        // btn.onclick = function() {
        //     console.log(this); // this指向的是btn这个按钮对象
        // }
        btn.addEventListener('click', function() {
                console.log(this); // this指向的是btn这个按钮对象

            })
            // 3. 构造函数中this指向构造函数的实例
        function Fun() {
            console.log(this); // this 指向的是fun 实例对象

        }
        var fun = new Fun();
    </script>
</body>

```



​	**<font color='red'>注意: 定时器里面的this指向window!</font>**

## 4. JS执行机制

### 4.1 JS是单线程

+ JavaScript 语言的一大特点就是单线程，也就是说，同一个时

  间只能做一件事。

  + Javascript 这门脚本语言诞生的使命所致——JavaScript 是为处理页面中用户的交互，以及操作DOM 而诞生的。比如我们对某个 DOM 元素进行添加和删除操作，不能同时进行。 应该先进行添加，之后再删除。

+ 单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。这样所导致的问题是： 如果 JS 执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉。
  

### 4.2 同步和异步

#### 1. 同步

+ 前一个任务结束后再执行后一个任务
+ 同步任务都在主线程上执行，形成一个 执行栈

#### 2. 异步

- JS中的异步是通过回调函数实现的
- 异步任务有以下三种类型
  - 普通事件，如`click`,`resize`等
  - 资源加载，如`load`,`error`等
  - 定时器，包括`setInterval`,`setTimeout`等
- 异步任务相关回调函数添加到任务队列中

#### 3. 执行顺序

1. 先执行执行栈中的同步任务
2. 异步任务(回调函数)放入任务队列中
3. 一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行

![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/d337ebf7ba2c40c7a768fd91f6bfbf56.png)



例子：

```javascript

console.log(1);
document.onclick = function() {
    console.log('click');
}
console.log(2);
setTimeout(function() {
    console.log(3)
}, 3000)


//执行的结果和顺序为 1、2、3

```

## 5. location对象

+ window 对象给我们提供了一个 `location`属性**用于获取或者设置窗体的url，并且可以解析url。**因为这个属性返回的是一个对象，所以我们将这个属性也称为 location 对象。

### 5.1 url

![image-20220309152752515](https://gitee.com/cws201800115/typora-images/raw/master/image-20220309152752515.png)

### 5.2 location 对象属性

![image-20220309153128360](https://gitee.com/cws201800115/typora-images/raw/master/image-20220309153128360.png)

### ♥ 案例 5s后跳转到哈工大

```html
<body>
    <button>点击</button>
    <div></div>
    <script>
        var btn = document.querySelector('button');
        var div = document.querySelector('div');
        var timer = 5;
        setInterval(function() {
            if (timer == 0) {
                location.href = 'https://www.gdut.edu.cn/';
            } else {
                div.innerHTML = '您将在' + timer + '秒钟之后跳转到一流大学';
                timer--;
            }

        }, 1000);
    </script>
</body>

```

### 5.3 location对象方法

| **location对象方法** | **返回值**                                                   |
| -------------------- | ------------------------------------------------------------ |
| location.assign()    | 跟href一样，可以跳转页面（也称为重定向页面）                 |
| location.assign()    | 替换当前页面，因为不记录历史，所以不能后退页面               |
| location.reload()    | 重新加载页面，相当于刷新按钮或者 f5 ，如果参数为true 强制刷新 ctrl+f5 |

```html
<body>
    <button>点击</button>
    <script>
        var btn = document.querySelector('button');
        btn.addEventListener('click', function() {
            // 记录浏览历史，所以可以实现后退功能
            // location.assign('http://www.itcast.cn');
            // 不记录浏览历史，所以不可以实现后退功能
            // location.replace('http://www.itcast.cn');
            location.reload(true);
        })
    </script>
</body>

```

## 6. navigator 对象

- navigator 对象包含有关浏览器的信息，它有很多属性
- 我们常用的是`userAgent`,该属性可以返回由客户机发送服务器的`user-agent`头部的值

判断用户是用哪个终端打开页面的，如果是用 PC 打开的，就跳转到 PC 端的页面，如果是用手机打开的，就跳转到手机端页面

```javascript
if((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {
    window.location.href = "";     //手机
 } else {
    window.location.href = "";     //电脑
 }

```

## 7.history对象

- window 对象给我们提供了一个 history 对象，与浏览器历史记录进行交互
- 该对象包含用户（在浏览器窗口中）访问过的 URL。

![image-20220309153739367](https://gitee.com/cws201800115/typora-images/raw/master/image-20220309153739367.png)

```html
<body>
    <a href="list.html">点击我去往列表页</a>
    <button>前进</button>
    <script>
        var btn = document.querySelector('button');
        btn.addEventListener('click', function() {
            // history.forward();
            history.go(1);
        })
    </script>
</body>

```

