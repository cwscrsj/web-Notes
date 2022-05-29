# ES5新增方法

+ 数组方法
+ 字符串方法
+ 对象方法

## 1. 数组方法

### 1.1 forEach() 遍历

```javascript
array.forEach(function(Value,index,arr))
```

- Value : 数组当前项的值
- index: 数组当前项的索引
- arr: 数组对象本身

### 1.2 filter() 筛选数组

```javascript
array.filter(function(Value,index,arr))
```

- `filter()`方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素，主要==用于筛选数组==
- 注意它==直接返回一个新数组==

### 1.3 some() 检测

- `some()`方法==用于检测数组中的元素是否满足指定条件==（查找数组中是否有满足条件的元素）
- 注意它==返回的是布尔值==，如果查找到这个元素，就返回true，如果查找不到就返回false
- 如果找到第一个满足条件的元素，则终止循环，不再继续查找

### ♥ 案例 搜索商品

![searchfowwhat](https://cws201800115.oss-cn-shenzhen.aliyuncs.com/typoraimges/searchfowwhat.gif)

+ 将数据存放到js数组
+ 利用foreach方法传入html里面
+ 通过js数组获取里面的数据，方便查找

```html
<body>
    <div class="search">
        按照价格查询: <input type="text" class="start"> - <input type="text" class="end"> <button
            class="search-price">搜索</button> 按照商品名称查询: <input type="text" class="product"> <button
            class="search-pro">查询</button>
    </div>
    <table>
        <thead>
            <tr>
                <th>id</th>
                <th>产品名称</th>
                <th>价格</th>
            </tr>
        </thead>
        <tbody>


        </tbody>
    </table>
    <script>
        // 利用新增数组方法操作数据
        var data = [{
            id: 1,
            pname: '小米',
            price: 3999
        }, {
            id: 2,
            pname: 'oppo',
            price: 999
        }, {
            id: 3,
            pname: '荣耀',
            price: 1299
        }, {
            id: 4,
            pname: '华为',
            price: 1999
        },];
        // 1. 获取相应的元素
        var tbody = document.querySelector('tbody');
        var search_price = document.querySelector('.search-price');
        var start = document.querySelector('.start');
        var end = document.querySelector('.end');
        var product = document.querySelector('.product');
        var search_pro = document.querySelector('.search-pro');
        setDate(data);
        // 2. 把数据渲染到页面中
        function setDate(mydata) {
            // 先清空原来tbody 里面的数据
            tbody.innerHTML = '';
            mydata.forEach(function (value) {
                // console.log(value);
                var tr = document.createElement('tr');
                tr.innerHTML = '<td>' + value.id + '</td><td>' + value.pname + '</td><td>' + value.price + '</td>';
                tbody.appendChild(tr);
            });
        }

        // 3. 根据价格查询商品
        // 当我们点击了按钮,就可以根据我们的商品价格去筛选数组里面的对象
        search_price.addEventListener('click', function () {
            // alert(11);
            var newDate = data.filter(function (value) {
                return value.price >= start.value && value.price <= end.value;
            });
            console.log(newDate);
            // 把筛选完之后的对象渲染到页面中
            setDate(newDate);
        });
        // 4. 根据商品名称查找商品
        // 如果查询数组中唯一的元素, 用some方法更合适,因为它找到这个元素,就不在进行循环,效率更高]
        search_pro.addEventListener('click', function () {
            var arr = [];
            data.some(function (value) {
                if (value.pname === product.value) {
                    // console.log(value);
                    arr.push(value);
                    return true; // return 后面必须写true  
                }
            });
            // 把拿到的数据渲染到页面中
            setDate(arr);
        })
    </script>
</body>
```







## 2. 字符串方法

- `trim()`方法会从一个字符串的==两端==删除空白字符
- `trim()`方法并不影响原字符串本身，==返回的是一个新的字符串==

```html
<body>
    <input type="text"> <button>点击</button>
    <div></div>
    <script>
        // trim 方法去除字符串两侧空格
        var str = '   an  dy   ';
        console.log(str);
        var str1 = str.trim();
        console.log(str1);
        var input = document.querySelector('input');
        var btn = document.querySelector('button');
        var div = document.querySelector('div');
        btn.onclick = function() {
            var str = input.value.trim();
            if (str === '') {
                alert('请输入内容');
            } else {
                console.log(str);
                console.log(str.length);
                div.innerHTML = str;
            }
        }
    </script>
</body>

```

## 3. 对象方法

### 3.1 Object.keys()

1. `Object.keys()`用于获取**对象自身所有的属性**
2. 效果类似`for...in`
3. 返回一个由属性名组成的数组

```javascript
        // 用于获取对象自身所有的属性
        var obj = {
            id: 1,
            pname: '小米',
            price: 1999,
            num: 2000
        };
        var arr = Object.keys(obj);
        console.log(arr);
```

### 3.2 Object.defineProperty()

+ 定义对象中新属性或==修改原有的属性==(了解)
+ `Object.defineProperty(obj,prop,descriptor)`
  + obj : 目标对象
  + prop : 需定义或修改的属性的名字
  + descriptor : 目标属性所拥有的特性

```javascript

        // Object.defineProperty() 定义新属性或修改原有的属性
        var obj = {
            id: 1,
            pname: '小米',
            price: 1999
        };
        // 1. 以前的对象添加和修改属性的方式
        // obj.num = 1000;
        // obj.price = 99;
        // console.log(obj);
        // 2. Object.defineProperty() 定义新属性或修改原有的属性
        Object.defineProperty(obj, 'num', {
            value: 1000,
            enumerable: true
        });
        console.log(obj);
        Object.defineProperty(obj, 'price', {
            value: 9.9
        });
        console.log(obj);
        Object.defineProperty(obj, 'id', {
            // 如果值为false 不允许修改这个属性值 默认值也是false
            writable: false,
        });
        obj.id = 2;
        console.log(obj);
        Object.defineProperty(obj, 'address', {
            value: '中国山东蓝翔技校xx单元',
            // 如果只为false 不允许修改这个属性值 默认值也是false
            writable: false,
            // enumerable 如果值为false 则不允许遍历, 默认的值是 false
            enumerable: false,
            // configurable 如果为false 则不允许删除这个属性 不允许在修改第三个参数里面的特性 默认为false
            configurable: false
        });
        console.log(obj);
        console.log(Object.keys(obj));
        delete obj.address;
        console.log(obj);
        delete obj.pname;
        console.log(obj);
        Object.defineProperty(obj, 'address', {
            value: '中国山东蓝翔技校xx单元',
            // 如果值为false 不允许修改这个属性值 默认值也是false
            writable: true,
            // enumerable 如果值为false 则不允许遍历, 默认的值是 false
            enumerable: true,
            // configurable 如果为false 则不允许删除这个属性 默认为false
            configurable: true
        });
        console.log(obj.address);
```



+ ==**value**==：设置属性的值，默认为undefined
+ **==writeable:==** 值是否可以重写 true | false 默认为false
+ **==enumerable==**: 目标属性是否可以被枚举 true | false 默认false
+ **==configurable:==** 目标属性是否可以被删除或是否可以再次修改特性 true | false 默认为false
