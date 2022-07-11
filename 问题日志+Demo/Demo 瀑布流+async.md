# 瀑布流+async

## 需求

之前尝试做了一个async异步加载的demo，这一次做一个升级版结合一下瀑布流，使用`json-server`写几个接口，ajax获取，具有以下功能：

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

+ 瀑布流

  ```javascript
  function waterFall(ContainerName) {
      // 获取父元素
      let container = document.getElementById(ContainerName);
      //获取子元素；
      let item = document.getElementsByClassName('item');
      // 定义一个数组，存放元素的高度
      let ar = [];
  
      //获取每一行最多能存放的元素    
      // 浏览器宽度/元素宽度 向下取整
      let clientWidth = document.documentElement.clientWidth;
      let columnCount = Math.floor(clientWidth / item[0].offsetWidth);
      container.style.width = columnCount * item[0].offsetWidth + "px";
  
      // 添加一个for循环，选取高度最短的放到数组当中
      for (let i = 0; i < item.length; i++) {
          if (i < columnCount) {
              item[i].style.top = "0px";
              item[i].style.left = i * item[0].offsetWidth + "px";
              ar.push(item[i].offsetHeight);
          }
  
          else {
              // Math作为内置对象不能处理数组，可以用扩展运算符的方法转化格式
              let min = Math.min(...ar);
              // 通过indexOf得到高度最小元素的索引号
              let index = ar.indexOf(min);
              // 将元素放到该元素下方
              item[i].style.top = ar[index] + 'px';
              item[i].style.left = item[index].style.left;
              //将元素放到对应后需要重新找到高度最小的那一个
              ar[index] += item[i].offsetHeight;
              // 给父盒子添加一个高度
              if (i + 1 == item.length) {
                  let max2 = Math.max(...ar);
                  // 通过indexOf得到高度最大元素的索引号
                  let indexMax = ar.indexOf(max2);
                  container.style.height = ar[indexMax] + 'px';
              }
          }
  
      }
  }
  ```

  

+ 节流函数

  ```javascript
  function throttle(callback, delay) {
      let flag = true;
      return function () {
          if (flag) {
              flag = false;
              setTimeout(() => {
                  callback();
                  flag = true;
              }, delay)
          }
  
      }
  }
  
  ```



+ 图片加载

  ```javascript
  // 利用promise：只有图片加载完毕才退出异步
  function loadImg(src) {
      return new Promise((resolve, reject) => {
          let img = new Image();
          img.src = src;
  
          img.onload = function () {
              resolve(img);
          };
          img.onerror = function (img) {
              reject('加载出错啦');
  
          };
      });
  
  
  }
  ```



+ 加载动画函数

  ```javascript
  function loadAnimate(fatherbox, flag) {
      const container = document.getElementById(fatherbox);
      if (flag) {
          let box = `
           <div class="spinner">
              <div class="rect1"></div>
              <div class="rect2"></div>
              <div class="rect3"></div>
              <div class="rect4"></div>
              <div class="rect5"></div>
          </div> `
  
          container.insertAdjacentHTML('beforeend', box);
      }
      else {
          let box = container.querySelector('.spinner');
          box.remove();
      }
  }
  ```

  



+ 最终完整代码

  ```javascript
          // 需要用到的全局变量
          let global_var = {
              // 判断变量，防止多次触发 
              init: true,
              //错误次数
              NUM_RETRIES: 3,
              url: ' http://localhost:3000',
          }
  
  
          // 底部无限加载
          window.addEventListener('scroll', throttle(() => {
              // 获取到所有子元素
              let item = document.getElementsByClassName('item');
              // 当页面滚轮到最后一个元素时发送请求加载数据
              if (item[item.length - 1].getBoundingClientRect().top < window.innerHeight && global_var.init == true) {
                  itemin(global_var.init);
              }
          }, 300))
  
  
  	//利用模板字符串放一个模板，返回结果
          function item1(title, avatar, name,) {
              return `            
                  <div class="user">
                      <h4 class="title">${title}</h4>
                      <div class="name">
                          <img class="head" src="${avatar}">
                          <p>${name}</p>
                      </div>
                  </div>
  
              `;
          }
          let container = document.getElementById('container');
          let img = document.getElementById('img');
  
  
          async function itemin() {
  
              global_var.init = false;
              loadAnimate('container', true);
              const fragment = document.createDocumentFragment();
              // 接收数据结果
              let response = await myAjax({
                  url: global_var.url + '/comments',
                  method: "get",
              });
  
              // 遍历获取数据，添加到内存空间当中
              await Promise.all(response.map(async value => {
                  // 获得文章基本信息
                  let { authorId, content, images } = value;
  
                  // 根据文章信息获取作者信息
                  let authorInfor = await myAjax({
                      url: global_var.url + '/authorBase',
                      method: 'get',
                      data: {
                          authorId: authorId,
                      }
                  });
  
                  // 获取作者头像和昵称
                  let { nickname, avatar } = authorInfor[0];
  
  
                  let itemAdd = item1(content, avatar, nickname);
  
                  // 创造一个item节点
                  let item = document.createElement('div');
                  let picture = document.createElement('div');
  
                  item.className = 'item';
                  picture.className = 'picture';
  
  
                  // 等到图片加载完成就添加到盒子里,如果发生错误则重试3次
                  for (let i = 0; i < global_var.NUM_RETRIES; ++i) {
                      try {
                          let finishLoadImg = await loadImg(images);
                          picture.appendChild(finishLoadImg);
                          break;
                      } catch (error) {
  
                      }
                  }
  
                  // picture.appendChild(await loadImg(images));
  
                  // 将初始化后的数据丢到盒子里
                  item.appendChild(picture);//放照片
                  item.insertAdjacentHTML('beforeend', itemAdd);//放文章和作者信息
  
                  fragment.appendChild(item);
  
  
              }))
  
              container.appendChild(fragment);
              // 执行瀑布流函数
              waterFall('container');
              //执行加载函数,把他关了
              loadAnimate('container', false);
              global_var.init = true;
  
  
          }
  
  
          itemin();
  ```

  