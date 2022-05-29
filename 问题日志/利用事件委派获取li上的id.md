## 点击特定模块进入文章详情页

![image-20220527210547872](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202205272105015.png)

+ 给每一个li添加自定义属性，值为获取过来的数据

+ 给父元素添加事件委派、保证点击item的==某一个位置都能进行跳转==

+ 利用递归或循环查询父节点的ClassName，获取到item的数据

  ```javascript
  let ul = document.getElementById('ul');
          let btn = document.getElementById('BTN');
          ul.addEventListener('click', (ev) => {
              let target = ev.target;
              while (target !== ul) {
                  if (target.className.toLowerCase() == 'item') {
                      console.log(target);
                      break;
                  }
  
                  target = target.parentNode;
  
              }
          })
  ```

  
