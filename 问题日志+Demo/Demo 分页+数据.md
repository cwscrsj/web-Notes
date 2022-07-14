# 分页+数据

## 思路

+ 获取数据，得到需要的页数，用一个变量存储数据
+ 写一个函数（下文叫：abc）：根据所得值向数据里面生成一个个item，默认值为页数，返回一个数组，该数组里面为每一页里面的所有节点
+ 在分页模块里添加一个事件：点击每一页触发abc函数，并通过排他思想显示出来。
+ 已经所得数据，创建分页表

## 效果

（暂时还没有数据）

![12345678910](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202207141056110.gif)

## 代码

+ 相关函数

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

+ 分页模块、

  ```javascript
  class Pages {
      // 默认配置
      static option = {
          //放入分页表的位置
          node: '',
          //页面的位置，
          container: '',
          // 每页显示数据条数（必填）
          limit: 5,
          // 数据总数
          count: 10,
          // 当前页码（选填，默认为1）
          curr: 1,
          // 当前页前后两边可显示的页码个数（选填，默认为2）
          pageShow: 2,
          //放入页面的数据
          data: '',
  
      }
  
      constructor(DataOptions) {
          //将默认数据和输入输入数据合并
          this.option = Object.assign(Pages.option, DataOptions);
          //插入位置
          this.node = document.getElementById(this.option.node);
          //页面位置
          this.container = document.getElementById(this.option.container);
          //获取对应节点
          this.list = this.node.querySelector('.main');
          this.nextBtn = this.node.querySelector('.next');
          this.pveBtn = this.node.querySelector('.last');
  
          // currArray代表目前出现的页码
          this.currArray = [];
          //初始化数据
          this.init();
          // this.showPages();
  
      }
  
      init() {
          //初始化数据
          this.showPage();
          //生成页面
          this.initCon();
          //初始化生成
          this.initPage();
          // 改变页数并触发事件
          this.changePage();
      }
      showPage() {
          this.currArray = [];
          const showOption = this.option;
          //第一种情况：当前页码还没到中间,处于左侧·
          if (showOption.curr < showOption.pageShow + 1) {
              for (let i = 1; i <= showOption.pageShow * 2 + 1; i++) {
                  this.currArray.push(i);
              }
              //第二种情况：当前页码大于中间，处于右侧
          } else if (showOption.curr > showOption.count - showOption.pageShow) {
              for (let i = showOption.count - showOption.pageShow * 2; i <= showOption.count; i++) {
                  this.currArray.push(i);
              }
          } else {
              for (let i = showOption.curr - showOption.pageShow; i <= showOption.curr + showOption.pageShow; i++) {
                  this.currArray.push(i);
              }
          }
  
      }
      initCon() {
          console.log(this.option.count);
          for (let i = 0; i < this.option.count; i++) {
  
              let item = `<ul class="list" page=${i + 1} style="width: 300px; height:200px">${i + 1}</ul>`;
              if (i == 0) item = `<ul class="list current" page=${i + 1} style="width: 300px; height:200px">${i + 1}</ul>`;
              this.container.insertAdjacentHTML('beforeend', item);
          }
  
          this.conList = this.container.querySelectorAll('.list');
  
      }
      changePage() {
  
          let pageElement = this.node;
          //通过事件委派实现页码变化
          pageElement.addEventListener('click', (e) => {
              let target = e.target;
              if (target.tagName.toLowerCase() == 'li') {
                  //让其他item样式去掉
                  this.goPage(parseInt(target.innerHTML));
              }
              else if (target.classList.contains('next')) {
                  this.nextPage();
              }
              else if (target.classList.contains('last')) {
                  this.pvePage();
              }
          })
      }
      //初始化分页表，跳转页码的主要函数
      initPage() {
          // 让页码全部清空
          this.list.innerHTML = '';
          //消除所有页码的点击样式（排他思想）
          clearOther(this.list, 'current');
  
          //把当前要显示的页码添加到数组中 currArray
          this.showPage();
          //和页面相绑定
          this.changeCon();
  
  
          //判断下一页和上一页的按钮是否需要隐藏（第一页和最后一页需要隐藏）
          if (this.option.curr == 1) {
              changeBtnAllow(this.pveBtn, 'unique', false);
          }
          else if (this.option.count == this.option.curr) {
              changeBtnAllow(this.nextBtn, 'unique', false);
          }
          else {
              changeBtnAllow(this.pveBtn, 'unique', true);
              changeBtnAllow(this.nextBtn, 'unique', true);
          }
  
          // 通过循环将页码添加到页码当中
          const fragment = document.createDocumentFragment();
          for (let i = 0; i < this.currArray.length; i++) {
              let li = document.createElement('li');
              li.innerHTML = this.currArray[i];
              if (this.currArray[i] == this.option.curr)
                  li.classList.add('current');
              fragment.appendChild(li);
          }
          this.list.append(fragment);
      }
      goPage(num) {
          this.option.curr = num;
          this.initPage();
      }
      pvePage() {
          this.option.curr--;
          this.initPage();
  
      }
  
      nextPage() {
          this.option.curr++;
          this.initPage();
      }
  
      changeCon() {
          clearOther(this.container, 'current');
          clearOther(this.container);
          this.conList[this.option.curr - 1].classList.add('current');
          this.conList[this.option.curr - 1].style.display = 'block';
      }
  }
  
  ```

  