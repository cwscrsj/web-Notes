# 瀑布流+async

## 需求

之前尝试做了一个async异步加载的demo，这一次做一个升级版，使用`json-server`写几个接口，ajax获取，具有以下功能：

+ 图片还没出现有**过渡动画**
+ **瀑布流**展示所有图片
+ 图片滑到最底部有**懒加载**
+ 添加**错误处理机制**（try..catch），图片如果加载失败则会重复尝试三次

## api

+ `getBoundingClientRect`视口大小
+ `document.createDocumentFragment()`不会触发 DOM 树的重新新渲染
+ `insertAdjacentHTML`将字符串 添加到html

## 问题/解决

+ 最开始是用`for of`来添加动态，即每一张图片加载完后才加载下一张。

  后面发现可以用 `promise.all+array.map `并发让所有图片一起加载

+ 由于瀑布流是利用`position:absolute`做的，后面想要添加东西就会被覆盖（脱离标准流）

  所有加了一个判断条件：在最后一个item的时候给父盒子添加当前高度

  ```javascript
  if (i + 1 == item.length) {
                  let max2 = Math.max(...ar);
                  // 通过indexOf得到高度最大元素的索引号
                  let indexMax = ar.indexOf(max2);
                  container.style.height = ar[indexMax] + 'px';
              }
  ```

## 缺点

+ 代码还是写的比较繁琐。很多还不知道怎么优化
+ 可以用模块化试试
+ 代码命名比较随意。。。还是不爱写注释，以后一定改

## 效果

（限制网络为高速3g）

![12345678](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202207110903080.gif)

## 代码

[cws的仓库](https://github.com/cwscrsj/Demo)