# async异步加载图片

## 需求

+ 在做瀑布流的时候经常会遇到一个问题：由于图片还未加载完的时候盒子的==高度是不确定==，不能使用瀑布流给他进行布局（==会导致页面乱掉==）。所以希望能监控图片的加载，等到加载完毕立刻让他变成瀑布流的形式

## 思路

+ 写个图片加载函数，当==图片加载完成就resolve，返回new image()==

+ 从api获取数据，用await接收获取过来的数据。

+ 使用map方法（并发），使得数据里面的所有图片==一起开始加载==，用一个变量存储所有promise结果

  （最开始用`for..of`加载图片，会导致第一张图片加载完之后才开始加载第二张图片，他是继发的）

+ 利用`promise.all`等待所有图片加载完成之后就添加到页面，利用` document.createDocumentFragment()`

## 缺点

+ 暂时还没有添加错误处理，如果有一张图片加载出错了可能都不显示了
+ 代码还是有点繁琐，后面改改。

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

            //将获取到的数据 提取照片出来,
            const imgs = res.map(async item => {
                let { images } = item;
                // 并发让所有照片进入加载
                //  使用for of的话，就必须等到每一张图片加载完成后才能继续下一张加载
                let img = await loadImg(images);
                // 添加到fragment当中
                fragment.appendChild(img);
                return img;

            })

            //利用promise.all保证所有照片加载完成后就执行操作
            Promise.all(imgs).then(function (e) {
                //关闭加载动画
                div.style.display = 'none';
                // 所有照片加载完毕后添加到dom里
                document.body.appendChild(fragment);
            });



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



![1234](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202206161802909.gif)
