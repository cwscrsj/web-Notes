# 轮播图

## 需求

之前用函数的方式写了一个轮播图，发现可读性比较差，然后就想着用class封装一个，可自定义程度相较于之前高了一些。

+ 需要在html里面写好 小圆点，左右箭头（可选）底部小圆圈（可选）

+ 生成方式：

  ```javascript
  new Carousel({
      	//插入位置，提供id
          node: 'container',
      	//生成轮播图的长和高
          width: 800,
          height: 400,
      	//数据个数
          count: 5,
      	//插入数据（一般是图片数组）
          data: img,
      	//是否需要自动播放
          autoplay: true,
      	//是否需要左右箭头（可选），默认为true
          slider: true,
      	//是否需要下按钮的位置，默认自带添加按钮
          circle: true,
      })
  ```

## 效果

![123456789101112](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202207151700476.gif)

## 代码

+ 清除所有样式

  ```javascript
  //排他思想
  function clearOther(container, name) {
      let item = container.children;
      for (let i = 0; i < item.length; i++) {
          if (name) {
              item[i].classList.remove(name);
          }
          else {
              item[i].style.display = 'none';
          }
      }
  }
  ```

+ 加载图片

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

+ 处理图片大小

  ```javascript
  //最简单的简单处理图片大小
  function adaptImg(img, NeedH) {
      if (img.height > NeedH) {
          img.style.width = 100 + '%';
      }
      else {
          img.style.height = 100 + '%';
      }
  }
  ```

+ class完整

  ```javascript
  //轮播图模块
  class Carousel {
      //节流按钮
      static lock = true;
      static option = {
          // 插入位置
          node: '',
          //是否需要下按钮的位置，默认自带添加按钮
          circle: true,
          //左右按钮
          slider: true,
          // 限定的宽高
          width: 100,
          height: 100,
          //图片总数
          count: 5,
          // 自动播放 默认为否
          autoplay: false,
          //图片来源
          data: '',
          //当前页面,默认为0
          curr: 0,
          //小圆圈的当前页面
          circleCurr: 0,
      }
      constructor(options) {
          //获取节点
          this.option = Object.assign(Carousel.option, options);
          this.container = document.getElementById(this.option.node);
          this.imgContainer = this.container.querySelector('.caro-con');
          if (this.option.circle) {
              this.circle = this.container.querySelector('.caro-cle-b');
          }
  
          if (this.option.slider) {
              this.nextBtn = this.container.querySelector('.caro-sli-next');
              this.lastBtn = this.container.querySelector('.caro-sli-last');
          }
          this.init();
      }
  
      init() {
          // 将图片添加到页面当中
          this.initCaro();
          
  	// 将底部小圆点添加到页面
          if (this.option.circle) {
              this.initCirle();
          }
          //让轮播图动起来
          this.moveCaro();
  
      }
  
      async initCaro() {
          
  		//加载动画
          loadAnimate(this.option.node, true, 'center');
          Carousel.lock = false;
          //给盒子初始化宽度
          const conWidth = (this.option.count + 1) * this.option.width;
          this.imgContainer.style.width = conWidth + 'px';
          this.imgContainer.style.transition = '0.5s ease';
  
          const fragment = document.createDocumentFragment();
          await Promise.all(this.option.data.map(async value => {
              let li = document.createElement('li');
  
              li.style.width = this.option.width + 'px';
              li.style.height = this.option.height + 'px';
  
  
              let img = await loadImg(value);
              //简单处理一下图片大小
              adaptImg(img, this.option.height);
              li.appendChild(img);
  
              fragment.appendChild(li);
          }))
          //克隆第一个图片放到最后
          let item = fragment.children[0].cloneNode(true);
          fragment.appendChild(item);
  
          // 关闭动画
          loadAnimate(this.option.node, false, 'center');
          Carousel.lock = true;
  
          this.imgContainer.appendChild(fragment);
  
  
      }
  	
      //把下方小圆点进去
      initCirle() {
          const fragment = document.createDocumentFragment();
          for (let i = 0; i < this.option.count; i++) {
              let li = document.createElement('li');
              fragment.appendChild(li);
          }
          fragment.children[0].classList.add('current');
          this.circle.appendChild(fragment);
      }
      moveCaro() {
  
          if (this.option.circle) {
              for (let i = 0; i < this.option.count; i++) {
                  //点击下方悬浮按钮
                  this.circle.children[i].addEventListener('click', () => {
                      this.option.curr = i;
                      this.option.circleCurr = i;
                      this.goCar();
                  })
              }
          }
  
          if (this.option.slider) {
              //点击左右按钮
              this.nextBtn.addEventListener('click', this.nextCar.bind(this));
              this.lastBtn.addEventListener('click', this.lastCar.bind(this));
  
          }
          //开启自动播放
          if (this.option.autoplay)
              this.autoplayCaro();
      }
      nextCar() {
          if (!Carousel.lock) return;
          this.option.curr++;
          this.option.circleCurr++;
  
          //移动轮播图
          this.imgContainer.style.left = -(this.option.curr) * this.option.width + 'px';
          this.imgContainer.style.transition = "ease 0.5s";
          //判断临界值
          if (this.option.curr == this.option.count) {
              this.option.circleCurr = 0;
              this.option.curr = 0;
              setTimeout(() => {
                  this.imgContainer.style.transition = "none";
                  this.imgContainer.style.left = 0;
              }, 500)
          }
          if (this.option.circle) {
              clearOther(this.circle, 'current');
              this.circle.children[this.option.circleCurr].className = 'current';
          }
          Carousel.lock = false;
          setTimeout(() => {
              Carousel.lock = true;
          }, 500);
      }
  
      lastCar() {
          if (!Carousel.lock) return;
  
          this.option.curr--;
          this.option.circleCurr--;
          // 页面从第一张到最后一张
          if (this.option.curr == -1) {
              //更新下方圆圈和当前页面的下标
              this.option.circleCurr = this.option.count - 1;
              this.option.curr = this.option.count - 1;
  
              //关闭动画
              this.imgContainer.style.transition = "none";
              //将容器移动至最后一张假图片
              this.imgContainer.style.left = -(this.option.count) * this.option.width + 'px';
              setTimeout(() => {
                  this.imgContainer.style.left = -(this.option.curr) * this.option.width + 'px';
                  this.imgContainer.style.transition = "ease 0.5s";
  
              }, 0)
          }
          else {
              this.imgContainer.style.transition = "ease 0.5s";
              this.imgContainer.style.left = -(this.option.curr) * this.option.width + 'px';
          }
  
  
          if (this.option.circle) {
              clearOther(this.circle, 'current');
              this.circle.children[this.option.circleCurr].className = 'current';
          }
  
          // 关锁
          Carousel.lock = false;
          setTimeout(() => {
              Carousel.lock = true;
          }, 500);
      }
  
      goCar() {
          if (!Carousel.lock) return;
  
          if (this.option.circle) {
              clearOther(this.circle, 'current');
              this.circle.children[this.option.circleCurr].className = 'current';
          }
          this.imgContainer.style.left = -(this.option.curr) * this.option.width + 'px';
  
          // 关锁
          Carousel.lock = false;
          setTimeout(() => {
              Carousel.lock = true;
          }, 500);
      }
  
      autoplayCaro() {
          let timer = setInterval(this.nextCar.bind(this), 2000);
          this.container.addEventListener('mouseenter', () => {
              clearInterval(timer);
              timer = null;
  
          })
          // 保证鼠标经过按钮时也不会重启定时器
          if (this.option.circle) {
              this.circle.addEventListener('mouseenter', () => {
                  clearInterval(timer);
                  timer = null;
              })
          }
          this.container.addEventListener('mouseleave', () => {
              // 先关掉定时器！！！！！！
              clearInterval(timer);
              timer = setInterval(this.nextCar.bind(this), 1000)
          })
      }
  }
  
  
  
  ```
  
  