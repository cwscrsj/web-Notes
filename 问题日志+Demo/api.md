## offset 偏移量

### 特点

+ 属性是只读的不可修改
+ 包括padding+border+width

![img](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fgitee.com%2Fcws201800115%2Ftypora-images%2Fraw%2Fmaster%2Fimage-20220313102340047.png&sign=cfdacff404f4891e85c8e513568c140e66ac6f12a17a529d846c90c00037f9b2)

### 应用场景

+ 轮播图获取宽度

## client 可视区

### 特点 

与`offsetWidth`最大的区别：==不包含边框==

![img](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fgitee.com%2Fcws201800115%2Ftypora-images%2Fraw%2Fmaster%2Fimage-20220313104148386.png&sign=35b539ae2102bdc7442b134e207d7ce2b6c81cd51a8d8ae732661aeb596b607b)

## scroll 滚动

### 特点

![img](https://www.yuque.com/api/filetransfer/images?url=https%3A%2F%2Fgitee.com%2Fcws201800115%2Ftypora-images%2Fraw%2Fmaster%2Fimage-20220313104346897.png&sign=0ba4710749ba1d4ad9d41d9e8897649e4629b66c44ba9e9bb127840cbec9aa26)

+ pageYOffset 返回元素被卷去头部高度

### 应用场景

+ 返回到顶部

  ```javascript
   //----------------- 让页面逐渐滚动到指定区域的函数--------
      function animate_scroll(move_scrollbox, final, callback) {
          // 先清除以前的定时器，只保留当前的一个定时器执行
          clearInterval(move_scrollbox.timer);
          move_scrollbox.timer = setInterval(function () {
              var step = (final - window.pageYOffset) / 10;
              if (step > 0)
                  step = Math.ceil(step);
              else
                  step = Math.floor(step);
              if (move_scrollbox.pageYOffset == final) {
                  // 停止动画
                  clearInterval(move_scrollbox.timer);
                  callback && callback();
              }
              // 这个步长值为一个慢慢变小的值  步长公式：(目标值 - 现在的位置) / 10
              // obj.style.left = window.pageYOffset + step + 'px';
              window.scroll(0, window.pageYOffset + step);
          }, 15);
      }
  ```

  

## getBoundingClientRect 视口

返回相对于浏览器视窗的相对位置

![image](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202205272224731.webp)

### 特点

+ 当滚动位置发生了变化，top和left的属性值就会随之发生变化

+ 如果你需要获得相对于整个网页左上角定位的属性值，那么只要给top、left属性值加上当前的滚动位置（通过window.scrollX和window.scrollY）

### 应用场景

底部无限加载（瀑布流）

```javascript
 window.addEventListener('scroll', throttle(() => {
                // 获取到所有子元素
                let item = document.getElementsByClassName('item');
                // 当页面滚轮到最后一个元素时发送请求加载数据
                if (item[item.length - 1].getBoundingClientRect().top < window.innerHeight) {
                    addItem();
                    waterFall();
                }
            }, 300))
```

## insertAdjacentHTML 追加元素

将指定的文本解析为 `Element`元素，并将结果节点插入到DOM树中的指定位置

### 特点

不会破坏元素内现有元素，比`innerHTML`更快

+ ` 'beforebegin'`：元素自身的前面。
+ `'afterbegin'`：插入元素内部的第一个子节点之前。
+ `'beforeend'`：插入元素内部的最后一个子节点之后。
+ `'afterend'`：元素自身的后面。 

### 应用场景

可以结合==模板字符串==向页面追加元素

```javascript
 btn.onclick = () => {
            let item = `
         <li class="item" id="${selectFrom(10, 30)}}">
            <span>获取id的按钮</span>
            <i>按钮</i>
        </li>`
            ul.insertAdjacentHTML('beforeend', item);
        }
```

## file 上传文件

以`<input type="file"id="avatar" name="avatar"accept="image/png, image/jpeg">`提交表单的形式上传

### 特点

![image-20220528090721349](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202205280907452.png)

+ 包括`change` 事件和`input`事件
+ 通过`FileList`对象操纵文件

### 应用场景

+ 通过`input file`结合表单通过ajax上传文件

```javascript
//保存图片的form表单        
let form = document.querySelector('#form');
let files = form.querySelector('input').files;
        // 确保有选择文件才开始
        if (files.length != 0) {
            ajax({
                type: 'formdata',
                method: 'post',
                url: url + '/upload/image',
                data: form,
                success: (res) => {
                 	console.log(res);
                }
            })

```

+ 在上传图片前预览图片

```javascript
//通过onchange判断是否有文件传入
avatarinput.onchange=()=> {
                    let file = avatarinput.files;
        for (let i = 0; i < file.length; i++) {
            avatar.src = window.URL.createObjectURL(file[i]);
            avatar.onload = function () {
                //照片显示之后就释放。
                window.URL.revokeObjectURL(this.src);
            }
        }
        }
```

## URL.createObjectURL() 预览图片

[MDN URL.createObjectURL()](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/createObjectURL)

[MDN 在web应用程序中使用文件](https://developer.mozilla.org/zh-CN/docs/Web/API/File/Using_files_from_web_applications#example.3a_using_object_urls_to_display_images)

**`URL.createObjectURL()`** 静态方法会创建一个 [`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)，其中包含一个表示参数中给出的对象的URL。

### 特点

用于创建 URL 的 [`File`](https://developer.mozilla.org/zh-CN/docs/Web/API/File) 对象、[`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 对象或者 [`MediaSource`](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaSource) 对象。

## localStorage 本地存储

存储的数据将保存在浏览器会话中,存储在 `localStorage` 的数据可以==长期保留==

### 特点

+ 设置：`localStorage.setItem('myCat', 'Tom');`

+ 读取：`let cat = localStorage.getItem('myCat');`

+ 移除：` localStorage.removeItem('myCat');` 

  ​			`//localStorage.clear();`

#### 与 cookie ，sessionStorage区别

##### 生命周期

cookie：==可设置失效时间==，没有设置的话，默认是关闭浏览器后失效

localStorage：除非被手动清除，否则将会永久保存。

sessionStorage： 仅在当前网页会话下有效，关闭页面或浏览器后就会被清除。

##### 存放数据大小：

cookie：4KB左右

localStorage和sessionStorage：可以保存5MB的信息。

##### http请求：

cookie：每次都会==携带在HTTP头中==，如果使用cookie保存过多数据会带来性能问题

localStorage和sessionStorage：仅在客户端（即浏览器）中保存，不参与和服务器的通信

##### 易用性：

cookie：需要程序员自己封装，源生的Cookie接口不友好

localStorage和sessionStorage：源生接口可以接受，亦可再次封装来对Object和Array有更好的支持



## DocumentFragment 添加元素

存储由节点（nodes）组成的文档结构。，它的变化不会触发 DOM 树的[重新渲染](https://developer.mozilla.org/zh-CN/docs/Glossary/Reflow)，且不会导致性能等问题。

### 特点

+ 添加方法类似`node.appendChild`和`Node.insertBefore`,

+ 所有节点会被一次插入到文档当中。而不是每个节点分别插入

### 应用场景

通过ajax获取过来数据，通过该api添加完毕后再渲染进dom

```javascript
    // 获取点赞列表
    getlike(MyuserId, (res) => {
        const fragment = document.createDocumentFragment();
        like.forEach(value => {
            const { articleId, images } = value.articleInfo;
            const { nickname, avatar, userId } = value.userInfo;
            let li = document.createElement('li');

            li.innerHTML = agreeStarItem(avatar, nickname, '赞了你的笔记', images[0]);
            fragment.appendChild(li);
        })
        agreeList.append(fragment);

```

## classList 类名

相比将 [`element.className`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/className) 作为以空格分隔的字符串来使用，`classList` 是一种更方便的访问元素的类列表的方法。

### 特点

1. 删除

   ```javascript
   div.classList.remove("foo");
   ```

2. 增加

   ```javascript
   div.classList.add("foo")
   ```

3. 有就添加没有就删除

   ```javascript
   div.classList.toggle("fool");
   ```

### 应用场景

利用toggle实现点击按钮就翻转盒子的功能

