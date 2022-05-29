# CSS第三天
## 1. css的三大特性 
### 1.1 层叠性
相同选择器设置相同样式,<span style="color:red"> 覆盖 </span>,主要解决样式内容的冲突。  
**原则**  
+ 就近原则,执行离结构更近的样式
+ 样式不冲突则不会重叠  
### 1.2 继承性  
子标签会继承父标签的某些样式，如文本颜色与子号
``` html
<style>
    <div> color:pink </div>
</style>

    <body> 
    <div> 
     <p>小陈小陈心想事成</p>  
     </div>  
    </body>
/* p内的样式与div中规定的样式相同 */ 
```
+ 可降低样式复杂性
+ 子元素可继承（text- font-,line及 color）
  
  ### 1.3优先级
  + 选择器相同，执行层叠性
  + 选择器不同，则根据权重决定  

| 样式           | 权重       |
| -------------- | ---------- |
| 继承式         | 0，0，0，0 |
| 元素选择器     | 0，0，0，1 |
| 类选择器       | 0，0，1，0 |
| ID选择器       | 0，1，0，0 |
| 行内样式 style | 1，0，0，0 |
| ！important    | 无穷大     |
&nbsp; &nbsp; **注意点**  
   1. 权重有4组数字，不会进位
   2. 类>元素，id>类
   3. <span style="color:red"> 继承的权重为0，不管父元素权重有多高，子元素权重为0 </span>  
### 1.4 权重的叠加
```html 
ul li{ };
<!-- 权重0,0,0,1+0,0,0,1=0,0,0,2 -->
```



## 2.盒子模型  

页面布局三大核心：**盒子模型**,**浮动**和**定位**

### 2.1看透页面布局本质

​	**布局**

  		1. 准备好相关网页元素，网页元素基本为box。
  		2. 利用css设置盒子央视
  		3. 往盒子里面添加内容

### 2.2 盒子模型的组成

**border边框,content内容，padding内边距，margin外边距**

### 2.3 边框（border）

| 名称         | 作用                        |
| ------------ | --------------------------- |
| border-width | 控制边框的粗细，单位为px    |
| border-style | 边框的样式，（solid为实线） |
| border-color | 边框颜色                    |

边框简写

``` css
border: 1px solid red;  
/* 粗细 样式 颜色 */
```

 ### 2.4 表格的细线边框

<span style="color:red">border-collapse</span> 控制相邻单元格边框

（两个单元格边框重合会导致边框过粗）

``` css
border-collapse:collapse;
```

### 2.5 边框会影响盒子大小

边框会额外增加盒子的实际大小。

- 测量盒子大小的时候,不量边框。
- 如果测量的时候包含了边框,则需要 width/height 减去边框宽度

### 2.6 内边距

#### 2.6.1 padding（边框与内容的距离）

```css
padding-left:20px;
padding-top:30px;
```

#### 2.6.2 padding的简写(<span style="color:red">重要</span>)

![](C:\Users\cws\Desktop\前端\前端资料\pink前端基础带资料\基础部分\05-前端基础CSS第三天\笔记\images\1571493298248.png)

 #### 2.6.3 padding 影响盒子的实际大小

+ 如果盒子有了宽度和高度，此时指定内边距会使得盒子变大
+ 让<span style="color:red">width/height减去多出来的内边距大小</span>

**<span style="color:red">若每个盒子字数一致，可以利用padding使得盒子变大</span>**

(案例：微博导航栏)

#### 2.6.4 特殊情况

如果盒子本身没有设定width height 属性，则padding 不会改变盒子大小。

### 2.7 外边距

```css
margin-left/right/top/bottom
```

 简写方式与padding一致.

#### 2.7.1 典型应用

**水平居中**

+ 盒子必须指定了宽度
+ 盒子左右外边距设置为auto

```css
magin-left:auto
/* 常用方式*/
margin：0 auto
```

<span style="color:red">注意：</span><u>以上方法是让块级元素水平居中，**行内元素**或者**行内块元素**水平居中给其父元素添加 **text-align:center** 即可。</u>

```css
<style>
      .header {
          width: 900px;
          height: 200px;
          background-color: green;
          margin: 100px auto;
  </style>
```



### 2.8 外边距的合并

使用margin定义块级元素垂直外边距，会出现外边距的合并

#### 2.8.1 相邻块元素垂直外边距的合并

![1571494239103.png](C:\Users\cws\Desktop\前端\前端资料\pink前端基础带资料\基础部分\05-前端基础CSS第三天\笔记\images\1571494239103.png)

解决方案：尽量只给一个盒子添加 margin

#### 2.8.2 嵌套块元素的塌陷

![1571494373778](C:\Users\cws\Desktop\前端\前端资料\pink前端基础带资料\基础部分\05-前端基础CSS第三天\笔记\images\1571494373778.png)

解决方案：

- 可以为父元素定义上边框。
- 可以为父元素定义上内边距。
- 可以为父元素添加 overflow:hidden。

### 2.9 清除内外边距 

```css
 * {
    padding:0;   /* 清除内边距 */
    margin:0;    /* 清除外边距 */
  }
```

