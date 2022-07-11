# 分页模块（简单）

## 需求

第一次尝试做分页模块（利用class封装），暂时还没有和数据结合起来，具备以下特点

+ 输入需要添加的位置，**数据信息**（数据总页数，当前页面页码）
+ 点击下一页上一页**更改页码**，单独点击页码也可以
+ 到达最后一页和第一页**隐藏按钮**

## 思路

+ 首先输入每一行 **当前页码面前后最多显示的页码数**，

  实现该效果：  [资料来源](https://juejin.cn/post/6844903646535106568#heading-3)![image-20220711213222932](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202207112132025.png)

+ 将该信息存储到数组里面，方便使用`currArray[]`
+ 使用class将下一页，上一页，点击跳转封装一个个函数

## 缺点

+ 使用太多if else进行判断，代码比较冗长
+ 还不知道怎么和获取的数据联系起来
+ 页码变化可以加点过渡，比较生硬
+ 总感觉还有很多需要补充

## 效果+代码

![123456789](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202207112141042.gif)

```javascript
        class Pages {
            // 默认配置
            static option = {
                // 每页显示数据条数（必填）
                limit: 5,
                // 数据总数
                count: 10,
                // 当前页码（选填，默认为1）
                curr: 1,
                // 当前页前后两边可显示的页码个数（选填，默认为2）
                pageShow: 2,
            }

            constructor(node, DataOptions) {
                //获取父元素
                this.container = document.getElementById(node);
                //获取对应节点
                this.list = this.container.querySelector('.main');
                this.nextBtn = this.container.querySelector('.next');
                this.pveBtn = this.container.querySelector('.last');
                //将默认数据和输入输入数据合并
                this.option = Object.assign(Pages.option, DataOptions);
                // currArray代表目前出现的页码
                this.currArray = [];
                //初始化数据
                this.init();
                // this.showPages();

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

            changePage() {

                let pageElement = this.container;
                //通过事件委派实现页码变化
                pageElement.addEventListener('click', (e) => {
                    let target = e.target;
                    if (target.tagName.toLowerCase() == 'li') {
                        //让其他item样式去掉
                        this.goPage(parseInt(target.innerHTML));
                    }
                    else if (target.className.toLowerCase() == 'next') {
                        this.nextPage();
                    }
                    else if (target.className.toLowerCase() == 'last') {
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

                //判断下一页和上一页的按钮是否需要隐藏（第一页和最后一页需要隐藏）
                if (this.option.curr == 1) {
                    this.pveBtn.style.visibility = 'hidden';
                }
                else if (this.option.count == this.option.curr) {
                    this.nextBtn.style.visibility = 'hidden';
                }
                else {
                    this.nextBtn.style.visibility = 'visible';
                    this.pveBtn.style.visibility = 'visible';
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


            init() {
                //初始化数据
                this.showPage();
                //初始化生成
                this.initPage();
                // 改变页数并触发事件
                this.changePage();
            }
        }
        
        //btn为添加的位置的id
        new Pages('btn', {
            // 数据总数
            count: 10,
            // 当前页码（选填，默认为1）
            curr: 1,
            // 当前页前后两边可显示的页码个数（选填，默认为2）
            pageShow: 3,
            
        })


```

