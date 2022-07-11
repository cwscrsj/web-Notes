# querySelector和getElementById之间的区别



## 1. getElementByxxx的用法

### 1.1 getElementById()

> getElementById() 方法可返回对拥有指定 ID 的第一个对象的引用
>
> 如果没有指定 ID 的元素返回 *null*
>
> 如果存在多个指定 ID 的元素则返回第一个。

该方法将返回一个与之对应id属性的节点对象，它是document对象特有的函数，只能通过其来调用该方法。

### 1.2 getElementsByTagName()

> getElementsByTagName() 方法可返回带有指定标签名的对象的集合。
>
> **提示：** 参数值 "*" 返回文档的所有元素。

+ 该方法返回一个对象数组（是HTMLCollection集合）,返回元素的顺序是它们在文档中的顺序,**传递给 getElementsByTagName() 方法的字符串可以不区分大小写。**

## 2. 关于querySelector

### 2.1  querySelector()

> querySelector() 方法返回文档中匹配指定 CSS 选择器的一个元素。

+  返回指定元素节点的子树中匹配选择器的集合中的==第一个元素==，如果没有匹配==返回null==。

+ 由于querySelector是按css规范来实现的，所以它**传入的字符串中第一个字符不能是数字**。

### 2.2 querySelectorAll()

> querySelectorAll() 方法返回文档中匹配指定 CSS 选择器的所有元素，返回  [NodeList](https://www.runoob.com/js/js-htmldom-nodelist.html)对象(伪数组)
>
> NodeList 对象表示节点的集合。可以通过索引访问，索引值从 0 开始。

　　按文档顺序返回指定元素节点的子树中匹配选择器的元素集合（NodeList），如果没有匹配返回空集合。

## 3. 两者区别

一般说的都是getElement(s)Byxxxx获取的是==动态集合==，querySelector获取的是==静态集合==。

### 3.1 动态集合 

+ 每当你往ul里添加了新元素后，list便会更新其值，从新获取ul里的所有li,它总会随着dom结构的变化而变化。

```html
<body>
<ul id="box">
    <li class="a">测试1</li>
    <li class="a">测试2</li>
    <li class="a">测试3</li>
</ul>
</body>
<script type="text/javascript">
//获取到ul，为了之后动态的添加li
    var ul = document.getElementById('box');
//获取到现有ul里面的li
    var list = ul.getElementsByTagName('li');
     for(var i =0;i<4;i++){
        ul.appendChild(document.createElement('li')); //动态追加li
    }
console.log(list.length); //7
</script>

```

### 3.2 静态集合

+ 代码静态集合体现在.querySelectorAll('li')获取到ul里所有li后，不管后续再动态添加了多少li，都是不会对其参数影响。

```html
<body>
<ul id="box">
    <li class="a">测试1</li>
    <li class="a">测试2</li>
    <li class="a">测试3</li>
</ul>
</body>
<script type="text/javascript">
//获取到ul，为了之后动态的添加li
     var ul = document.querySelector('ul');
//获取到现有ul里面的所有li
     var list = ul.querySelectorAll('li');
     for(var i = 0;i<list.length;i++){
         ul.appendChild(document.createElement('li'));//动态追加li
     }
console.log(list.length); //输出的结果仍然是3，不是此时li的数量6
</script>
```

