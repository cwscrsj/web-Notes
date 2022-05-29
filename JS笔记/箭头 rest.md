# 函数

## 1.函数参数的默认值

+ 直接写在参数定义的后面。

+ 与解构参数默认值结合使用

  ```javascript
    ////第二个参数是对象，必须传递一个空对象进去，因为第二个对象作为函数参数没有默认值
  
  	function fetch(url, { body = '', method = 	'GET', headers = {} }) {
              console.log(method);
          }
  
          fetch('http://example.com', {})
          // "GET"
  
          fetch('http://example.com')
          // 报错
  
  //此时第二个参数有默认值
  function fetch(url, { body = '', method = 'GET', headers = {} } = {}) {
    console.log(method);
  }
  
  fetch('http://example.com')
  // "GET"
  ```

+ 思考

  ```javascript
  // 写法一
  function m1({x = 0, y = 0} = {}) {
    return [x, y];
  }
  
  // 写法二
  function m2({x, y} = { x: 0, y: 0 }) {
    return [x, y];
  }
  
  // 函数没有参数的情况
  m1() // [0, 0]
  m2() // [0, 0]
  
  // x 和 y 都有值的情况
  m1({x: 3, y: 8}) // [3, 8]
  m2({x: 3, y: 8}) // [3, 8]
  
  // x 有值，y 无值的情况
  m1({x: 3}) // [3, 0]
  m2({x: 3}) // [3, undefined]
  
  // x 和 y 都无值的情况
  m1({}) // [0, 0];
  m2({}) // [undefined, undefined]
  
  m1({z: 3}) // [0, 0]
  m2({z: 3}) // [undefined, undefined]
  ```

  + 写法一函数参数的默认值是空对象，但是设置了对象解构赋值的默认值；
  + 写法二函数参数的默认值是一个有具体属性的对象，但是没有设置对象解构赋值的默认值。

## 2. 函数length属性

+ 指定了默认值以后，函数的`length`属性，将返回没有指定默认值的参数个数，==指定了默认值后，length属性将失真。==

  ```javascript
  (function (a) {}).length // 1
  (function (a = 5) {}).length // 0
  (function (a, b, c = 5) {}).length // 2
  ```

  

## 3. rest参数

+ 获取函数的多余参数，==返回数组==

+ rest之后不能有其他参数，否则报错

  ```javascript
          let a = (a, b, ...c) => {
              console.log(c);
          }
  
          a(1, 2, 'a', 'b', 'c', 'd');
  
  ```

  ![image-20220414231506311](https://cws201800115.oss-cn-shenzhen.aliyuncs.com/typoraimges/image-20220414231506311.png)

## 4. 严格模式

+ 函数参数使用了默认值、解构赋值、或者扩展运算符，那么函数内部就不能显式设定为严格模式

##  5. name属性

+ 返回函数名字

+ `Function`构造函数返回的==函数实例==，`name`属性的值为`anonymous`。

  ```javascript
          function Boo() { };
  
          console.log(Boo.name);
          console.log((new Function).name);
  
  ```

  

## 6. 箭头函数

### 6.1 写法

+ 常规写法

```javascript
let fn=(a,b) => {
            console.log('这是一个箭头函数');
        }
```

+ 简写：

  + 有且只有一个形参

    ```javascript
     	let fn = a => {
                console.log(a);
            }
     
     fn(3);
    ```

  + 省略花括号和return

    ```javascript
    //求出数组的偶数        
    
    let a = [3, 6, 8, 12, 6, 43, 12];
    let newa = a.filter(value => value % 2 == 0);
    console.log(newa);
    
    ```

### 6.2 特点

+ 可以与变量解构结合使用

+ ==this是静态的==，始终指向函数声明时所在作用域下this的值，无法被更改，不管嵌套多少个箭头函数，this都是指向唯一的

  （箭头函数根本没有自己的`this`，导致内部的`this`就是外层代码块的`this`。正是因为它没有`this`，所以也就不能用作构造函数。）

  ```javascript
          let div = document.querySelector('div');
          div.addEventListener('click', function () {
              console.log(this);//div
               setTimeout(() => {
               console.log(this);//div
               this.style.background = 'pink';
            }, 1000)
          })
  
  //需要动态this的时候，也不应使用箭头函数。
  var button = document.getElementById('press');
  button.addEventListener('click', () => {
    this.classList.toggle('on');//箭头函数产生时，this指向window
  });
  ```

  

+ 不能作为构造实例化对象

+ ==不能用arguments==

+ 箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。

  ```javascript
  // 报错
  let getTempItem = id => { id: id, name: "Temp" };
  
  // 不报错
  let getTempItem = id => ({ id: id, name: "Temp" });
  ```

### 6.3 用法

+ 与变量解构结合

  ```javascript
  
  //相当于将传入的对象中的first和last属性中的内容相加
  const full = ({ first, last }) => first + ' ' + last;
  
          // 等同于
          let obj = {
              first: 'hello',
              last: 'yes'
          }
          console.log(full(obj));// hello yes
  
  
  //返回偶数
  const isEven = n => n % 2 === 0;
  ```

  
