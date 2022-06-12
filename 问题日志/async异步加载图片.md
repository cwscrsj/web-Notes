# async异步加载图片

## 需求

+ 在做瀑布流的时候经常会遇到一个问题：由于图片还未加载完的时候盒子的==高度是不确定==，不能使用瀑布流给他进行布局（==会导致页面乱掉==）。所以希望能监控图片的加载，等到加载完毕立刻让他变成瀑布流的形式

## 思路

+ 写个函数，用promise 代表加载图片，当==图片加载完成就resolve==
+ 从api获取数据，循环获取每一个数据，用await等到==每一张图片加载完成==才进行下一次循环
+ 图片全部加载完成用`document.createDocumentFragment()`添加到dom当中

## 缺点

+ 出现一张照片就开始加载，时间太长了，可以尝试用`promise.all`所有图片一起加载，节省一些时间
+ 代码写的比较繁琐，后面改改

## 代码

```javascript
    	 //用json-server随便写的数据   
		const url = 'http://localhost:3000';

        //获取按钮和过场动画（不重要）
        let btn = document.getElementById('btn');
        let div = document.getElementById('div');
        // 设置监听事件
        btn.addEventListener('click', async function click(e) {
            // 显示加载动画
            div.style.display = 'block';

            // 先将获取到的信息存放到内存空间
            const fragment = document.createDocumentFragment();
            //利用res接收获取到的数据
            let res = await myAjax({
                url: url + '/comments',
                method: 'get',
            })

            //将获取到的数据 提取照片出来
            for (item of res) {
                let { images } = item;//解构赋值

                //将加载完毕的照片 用img接收
                let img = await loadImg(images);

                // 添加到fragment当中
                fragment.appendChild(img);
            }

            //关闭加载动画
            div.style.display = 'none';

            // 所有照片加载完毕后添加到dom里
            document.body.appendChild(fragment);
        })



        // 利用promise：只有图片加载完毕才退出异步
        function loadImg(src) {
            return new Promise((resolve, reject) => {
                var img = new Image();
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

## 效果

（获取的图片大小在2m-4m左右）

![动画](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202206122016704.gif)

