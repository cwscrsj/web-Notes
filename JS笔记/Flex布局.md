# Flex布局

## 1. 概念

flex意为==“弹性布局”==，任何一个容器都可以指定为flex布局

+ 当我们为父盒子设为 flex 布局以后，子元素的 float、clear 和 vertical-align 属性将失效
+ flex布局属性主要由==容器属性==和==项目属性==构成

## 2.  布局原理

采用 Flex 布局的元素，称为 Flex 容器。它的所有**子元素**自动成为容器成员，称为 **Flex 项目**，简称"项目"

+ 子容器可以横向排列也可以纵向排列

  ![image-20220314231552048](https://gitee.com/cws201800115/typora-images/raw/master/image-20220314231552048.png)

## 3. 容器属性

### 3.1 flex-direction 属性

取值：**row(默认)** | row-reverse | column | column-reverse

+ row：横向排列，主轴为横向
+ column：纵向排列，主轴竖向
+ row-reverse/column-reverse：倒序排列

![image-20220314232026641](https://gitee.com/cws201800115/typora-images/raw/master/image-20220314232026641.png)

### 3.2 flex-wrap 属性

取值：**nowrap(默认)** | wrap | wrap-reverse

默认情况下，项目都排在一条线（又称”轴线”）上。flex-wrap属性定义，flex布局中默认是不换行的。

1. nowrap：不换行![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/20190514192934120.png)

2. wrap：换行![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/20190514192942570.png)

3. wrap-reverse：换行，第一行在下面![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/20190514192955723.png)

### 3.3 flex-flow 属性

flex-flow属性是==flex-direction==与==flex-wrap==属性的简写集合

+ 默认属性为row nowrap，即横向排列，且不换行，

+ 如果需要控制项目排列与换行，推荐使用此属性，而非单独写两个。

### 3.4 justify-content 属性

取值：**flex-start(默认)** | flex-end | center | space-between | space-around | space-evenly;

![img](https://gitee.com/cws201800115/typora-images/raw/master/c55dfe8e3422458b50e985552ef13ba5.png)

### 3.5 align-items 属性

取值：**stretch(默认)**|flex-start | flex-end | center | baseline | 

+ 控制项目在纵轴排列方式

+ 默认stretch即如果项目没设置高度，或高度为auto，则**占满整个容器**

1. flex-start![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/20190514193059217.png)
2. flex-end![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/20190514193103732.png)
3. center![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/20190514193108772.png)
4. **baseline** 项目的第一行文字的基线对齐
5. **stretch**：默认值：如果项目未设置高度或者高度为auto，将占满整个容器的高度

### 3.6 align-content 属性（多行）

取值：**stretch(默认)** | flex-start | flex-end | center | space-between | space-around | space-evenly

+ 控制多行项目的对齐方式，
+ 如果项目==只有一行则不会起作用==；
+ 默认stretch: 在项目没设置高度，或高度为auto情况下让项目填满整个容器，与==align-items==类似。



## 4. 项目属性

### 4.1 order 属性

取值：**默认0**，用于决定项目排列顺序，数值越小，项目排列越靠前。

![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/20190514193124268.png)

### 4.2 flex-grow 属性

取值：**默认0**，

+ 决定项目在有剩余空间的情况下是否放大，默认不放大；
+ **注意**，**即便设置了固定宽度，也会放大。**

假设默认4个项目中前3个个项目都是0，最后一个是1，最后的项目会沾满剩余所有空间。

```css
.green-item{
    order:-1;
    flex-grow:2;
}

```



![在这里插入图片描述](https://gitee.com/cws201800115/typora-images/raw/master/20190514193129505.png)

### 4.3 flex-shrink 属性

取值：默认1，

+ 决定项目在空间不足时是否缩小，**默认项目都是1**，即空间不足时大家一起等比缩小；
+ **注意，即便设置了固定宽度，也会缩小。**

![img](https://gitee.com/cws201800115/typora-images/raw/master/1213309-20190808191100715-1387948858.gif)

### 4.4 align-self 属性

取值：**auto(默认) |** flex-start | flex-end | center | baseline | stretch

+ 表示继承父容器的align-items属性。如果没父元素，则默认stretch。

+ 用于让个别项目拥有与其它项目不同的对齐方式，各值的表现与父容器的align-items属性完全一致。

### 4.5 flex 属性

**取值：默认 0 1 auto，**

+ flex属性是==flex-grow==，==flex-shrink==与==flex-basis==三个属性的简写，用于定义项目放大，缩小与宽度。

+ 该属性有两个快捷键值，分别是auto(1 1 auto)等分放大缩小，与none(0 0 auto)不放大不缩小。
