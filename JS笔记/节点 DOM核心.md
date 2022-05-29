# 节点 DOM核心

## 1. 节点概述

网页中的**<font color='red'>所有内容都是节点</font>**（标签、属性、文本、注释等），在DOM 中，节点使用 node 来表示。

HTML DOM 树中的所有节点均可通过 JavaScript 进行访问，所有 HTML 元素（节点）均可被修改，也可以创建或删除。

节点至少拥有nodeType（节点类型）、nodeName（节点名称）和nodeValue（节点值）这三个基本属性。

+ 元素节点：nodeType 为1
+ 属性节点：nodeType 为2
+ 文本节点：nodeType 为3(文本节点包括文字、空格、换行等)
+ 我们在实际开发中，节点操作主要操作的是**元素节点**

利用 DOM 树可以把节点划分为不同的层级关系，常见的是**父子兄层级关系。**

## 2. 父级节点

`node.parentNode`，可以返回最近的父节点

如果指定的节点没有父节点则返回NULL；	

## 3. 子节点

`parentNode.childNodes(标准)`

+ parentNode.childNodes 返回包含指定节点的子节点的集合，该集合为**即时更新**的集合
+ 返回值包含了**所有**的子结点，包括<font color='red'>**元素节点，文本节点**</font>等，因此不提倡使用



`parentNode.children(非标准)`

- `parentNode.children` 是一个只读属性，返回所有的子元素节点
- 它<font color='red'>只返回子元素节点</font>，其余节点不返回 （**<font color='red'>重点</font>**）
- 虽然 children 是一个非标准，但是得到了各个浏览器的支持，因此我们可以放心使用

###  3.1 first 和 last

+ `parentNode.firstChild`
+ `parentNode.lastChild`
+ **<font color='red'>包含所有节点</font>**

### 3.2 存在兼容性问题 first last

+ `parentNode.firstElementChild`
+ `parentNode.lastElementChild`
+ **<font color='red'>IE9以上才支持</font>**

### 3.3 解决方案

- 第一个子元素节点，可以使用 `parentNode.chilren[0]`

- 最后一个子元素节点，可以使用

  ```js
  // 数组元素个数减1 就是最后一个元素的索引号
  parentNode.chilren[parentNode.chilren.length - 1]
  ```

```html
<body>
    <ol>
        <li>我是li1</li>
        <li>我是li2</li>
        <li>我是li3</li>
        <li>我是li4</li>
    </ol>
    <script>
        var ol = document.querySelector('ol');
        // 1.firstChild 获取第一个子结点的，包含文本结点和元素结点
        console.log(ol.firstChild);
        // 返回的是文本结点 #text(第一个换行结点)
        
        console.log(ol.lastChild);
        // 返回的是文本结点 #text(最后一个换行结点)
        // 2. firstElementChild 返回第一个子元素结点
        console.log(ol.firstElementChild);
        // <li>我是li1</li>
        
        // 第2个方法有兼容性问题，需要IE9以上才支持
        // 3.实际开发中，既没有兼容性问题，又返回第一个子元素
        console.log(ol.children[0]);
        // <li>我是li1</li>
		// 4.返回最后一个
        console.log(ol.children[ol.children.length - 1]);
    </script>
</body>

```

## 4. 兄弟节点

### 4.1 下一个兄弟节点

+ `node.nextSibling`

**<font color='red'>也是包含所有的节点</font>**

### 4.2 上一个兄弟节点

+ `node.previousSibling`

**<font color='red'>也是包含所有的节点</font>**

### 4.3 上下一个兄弟节点(兼容性)

+ `node.nextElementSibling`

+ node.previousElementSibling

<font color='red'>**有兼容性问题，IE9才支持**</font>

```html
<body>
    <div>我是div</div>
    <span>我是span</span>
    <script>
        var div = document.querySelector('div');
        // 1.nextSibling 下一个兄弟节点 包含元素节点或者 文本节点等等
        console.log(div.nextSibling);		// #text
        console.log(div.previousSibling);	// #text
        // 2. nextElementSibling 得到下一个兄弟元素节点
        console.log(div.nextElementSibling);	//<span>我是span</span>
        console.log(div.previousElementSibling);//null
    </script>
</body>

```

### 4.4 解决方案（封装函数）

```javascript
function getNextElementSibling(element) {
    var el = element;
    while(el = el.nextSibling) {
        if(el.nodeType === 1){
            return el;
        }
    }
    return null;
}
```

## 5. 创建节点

`document.createElement('tagName');`

+ `<font color='red'>document.createElement() </font>方法创建由 tagName 指定的HTML 元素

+ 根据我们的需求动态生成的，所以我们也称为动态创建元素节点

### 5.1 添加节点(先创建再添加)

+ **<font color='red'>node.appendChild() 方法</font>**将一个节点添加到指定父节点的子节点列表**<font color='red'>末尾</font>**。类似于 CSS 里面的 after 伪元素。

+ **<font color='red'>node.insertBefore() 方法</font>**将一个节点添加到父节点的指定子节点**<font color='red'>前面</font>**。类似于 CSS 里面的 before 伪元素。

  ```html
  <body>
      <ul>
          <li>123</li>
      </ul>
      <script>
          // 1. 创建节点元素节点
          var li = document.createElement('li');
          // 2. 添加节点 node.appendChild(child)  node 父级  child 是子级 后面追加元素  类似于数组中的push
          // 先获取父亲ul
          var ul = document.querySelector('ul');
          ul.appendChild(li);
          // 3. 添加节点 node.insertBefore(child, 指定元素);
          var lili = document.createElement('li');
          ul.insertBefore(lili, ul.children[0]);
          // 4. 我们想要页面添加一个新的元素分两步: 1. 创建元素 2. 添加元素
      </script>
  </body>
  
  ```

### 5.2 删除节点

`node.removeChild(child)`

+ **<font color='red'>node.removeChild()方法</font>**从 DOM 中**删除一个子节点**，**返回删除的节点**

### 5.3 复制节点(克隆节点)

`node.cloneNode()`

+ **<font color='red'>node.cloneNode()方法</font>**返回调用该方法的节点的一个副本。 也称为克隆节点/拷贝节点

+ 如果括号参数为空或者为 false ，则是浅拷贝，即只克隆复制节点本身，不克隆里面的子节点

+ 如果括号参数为 true ，则是深度拷贝，会复制节点本身以及里面所有的子节点
  示例

  ```html
  <body>
      <ul>
          <li>1111</li>
          <li>2</li>
          <li>3</li>
      </ul>
      <script>
          var ul = document.querySelector('ul');
          // 1. node.cloneNode(); 括号为空或者里面是false 浅拷贝 只复制标签不复制里面的内容
          // 2. node.cloneNode(true); 括号为true 深拷贝 复制标签复制里面的内容
          var lili = ul.children[0].cloneNode(true);
          ul.appendChild(lili);
      </script>
  </body>
  <body>
      <ul>
          <li>1111</li>
          <li>2</li>
          <li>3</li>
      </ul>
      <script>
          var ul = document.querySelector('ul');
          // 1. node.cloneNode(); 括号为空或者里面是false 浅拷贝 只复制标签不复制里面的内容
          // 2. node.cloneNode(true); 括号为true 深拷贝 复制标签复制里面的内容
          var lili = ul.children[0].cloneNode(true);
          ul.appendChild(lili);
      </script>
  </body>
  
  ```


### 5.4  三种动态创建元素的区别

- doucument.write()

​	**document.write是直接将内容写入页面的内容流，会导致页面全部重绘**

- element.innerHTML
  **使用inner.innerHTML 拼接字符串**
  **字符串是不可变的，每次创建新的字符串都需要开辟新的空间进行存储数据，因此添加1000个元素，也即需要开辟1000个空间，所以当数据量不断增加的时候，效率会变得越来越低。**
- document.createElement()

```html
<body>
    <div class="innner"></div>
    <div class="create"></div>
    <script>
        // 2. innerHTML 创建元素
        var inner = document.querySelector('.inner');
        // 2.1 innerHTML 用拼接字符串方法
        for (var i = 0; i <= 100; i++) {
            inner.innerHTML += '<a href="#">百度</a>';
        }
        // 2.2 innerHTML 用数组形式拼接
        var arr = [];
        for (var i = 0; i <= 100; i++) {
            arr.push('<a href="#">百度</a>');
        }
        inner.innerHTML = arr.join('');

        // 3.document.createElement() 创建元素
        var create = document.querySelector('.create');
        var a = document.createElement('a');
        create.appendChild(a);
    </script>
</body>

```

> 总结：不同浏览器下， innerHTML 效率要比 createElement 高

## 6. DOM核心

### 6.1 创建

+ document.write
+ innerHTML

+ createElement

### 6.2 增

+ appendChild
+ insertBefore

### 6.3 删

+ removeChild

### 6.4 改

**主要修改dom的元素属性，dom元素的内容、属性、表单的值等**

+ 修改元素属性：**src、href、title** 等
+ 修改普通元素内容：**innerHTML、innerText**
+ 修改表单元素：**value、type、disabled**
+ 修改元素样式：**style、className**

### 6.5 查

**主要获取查询dom的元素**

+ DOM提供的API方法：getElementById、getElementsByTagName 
+ H5提供的新方法：querySelector、querySelectorAll (提倡)
+ 利用节点操作获取元素：父(parentNode)、子(children)、兄(previousElementSibling、nextElementSibling) 提倡

### 6.6 属性操作

**主要针对于自定义属性**

+ setAttribute：设置dom的属性值
+ getAttribute：得到dom的属性值
+ removeAttribute：移除属性

