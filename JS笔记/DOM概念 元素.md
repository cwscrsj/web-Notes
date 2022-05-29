# DOM概念 元素

## 1.基础概念

### 1.1 什么是DOM

文档对象模型（Document Object Model，简称 DOM），是 W3C 组织推荐的处理可扩展标记语言（HTML或者XML）的标准编程接口

W3C 已经定义了一系列的 DOM 接口，通过这些 DOM 接口可以改变网页的内容  结构和样式。

![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/fc42557d25be4683881c2f0f231bc778.png)

## 2. 获取元素  

### 2.1 如何获取元素

+ 根据 ID 获取
+ 根据标签名获取
+ 通过 HTML5 新增的方法获取
+ 特殊元素获取

### 2.2 根据ID获取

`doucument.getElementByld('id名')`

+ <font color='red'>使用 `console.dir()` 可以打印我们获取的元素对象，更好的查看对象里面的属性和方法。</font>

```html
<div id="time">2019-9-9</div>
<script>
    // 1.因为我们文档页面从上往下加载，所以得先有标签，所以script写在标签下面
    // 2.get 获得 element 元素 by 通过 驼峰命名法
    // 3.参数 id是大小写敏感的字符串
    // 4.返回的是一个元素对象
    var timer = document.getElementById('time');
    console.log(timer);
    // 5. console.dir 打印我们的元素对象，更好的查看里面的属性和方法
    console.dir(timer);
</script>

```

### 2.3 根据标签名获取

+ 因为得到的是一个**<font color='red'>对象的集合</font>**，所以我们想要操作里面的元素就需要遍历
+ 得到元素对象是<font color='red'>**动态**</font>的
+ 返回的是获取过来元素对象的集合，以伪数组的形式存储
+ 如果获取不到元素，则返回为空的伪数组(因为获取不到对象)

```html
<ul>
    <li>知否知否，应是等你好久</li>
    <li>知否知否，应是等你好久</li>
    <li>知否知否，应是等你好久</li>
    <li>知否知否，应是等你好久</li>
    <li>知否知否，应是等你好久</li>
</ul>
<script>
    // 1.返回的是获取过来元素对象的集合 以伪数组的形式存储
    var lis = document.getElementsByTagName('li');
    console.log(lis);
    console.log(lis[0]);
    // 2.依次打印,遍历
    for (var i = 0; i < lis.length; i++) {
        console.log(lis[i]);
    }
    // 3.如果页面中只有 1 个 li，返回的还是伪数组的形式
    // 4.如果页面中没有这个元素，返回的是空伪数组
</script>

```

### 2.4 根据标签名获取

```javascript
element.getElementsByTagName('标签名')

ol.getElementsByTagName('li');

```

注意：父元素必须是<font color='red'>单个对象(必须指明是哪一个元素对象)</font>，获取的时候不包括父元素自己

```html
<script>
	//element.getElementsByTagName('标签名'); 父元素必须是指定的单个元素
    var ol = document.getElementById('ol');
    console.log(ol[0].getElementsByTagName('li'));
</script>

```

### 2.5 通过H5新增方法获取

#### 1. getElementsByClassName

+ **根据类名返回元素对象<font color='red'>合集</font>**

`document.getElementsByClassName('类名')`

#### 2. document.querySelector

+ **根据指定选择器返回<font color='red'>第一个元素对象</font>**

```javascript
document.querySelector('选择器');

// 切记里面的选择器需要加符号 
// 类选择器.box 
// id选择器 #nav
var firstBox = document.querySelector('.box');

③document.querySelectorAll
根据指定选择器返回所有元素对象

document.querySelectorAll('选择器');

```

#### 3. document.querySelectorAll

+ **根据指定选择器返回所有元素对象**

`document.querySelectorAll('选择器');`

注意：

`<font color='red'>` 和 `querySelectorAl`l 里面的选择器需要**<font color='red'>加符号,</font>**比如: `document.querySelector('#nav');`

#### 4. 例子

```html	
<script>
    // 1. getElementsByClassName 根据类名获得某些元素集合
    var boxs = document.getElementsByClassName('box');
    console.log(boxs);
    // 2. querySelector 返回指定选择器的第一个元素对象  切记 里面的选择器需要加符号 .box  #nav
    var firstBox = document.querySelector('.box');
    console.log(firstBox);
    var nav = document.querySelector('#nav');
    console.log(nav);
    var li = document.querySelector('li');
    console.log(li);
    // 3. querySelectorAll()返回指定选择器的所有元素对象集合
    var allBox = document.querySelectorAll('.box');
    console.log(allBox);
    var lis = document.querySelectorAll('li');
    console.log(lis);
</script>
```

### 2.6. 获取特殊元素

#### 1. 获取body元素

返回body元素对象

```javascript
document.body;
```

#### 2. 获取html元素

返回html元素对象

```javascript
document.documentElement;
```

