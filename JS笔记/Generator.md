# Generator

[MDN yield](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/yield)

[ES6 Generator ](https://es6.ruanyifeng.com/#docs/generator)

## 1. 概念

+ 简单理解：Generator 函数是一个状态机，封装了==多个内部状态==。

+ Generator 函数除了是状态机，还是一个遍历器对象生成函数。

```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

let hw = helloWorldGenerator();

//上面代码定义了一个 Generator 函数helloWorldGenerator，它内部有两个yield表达式（hello和world），即该函数有三个状态：hello，world 和 return 语句（结束执行）。
```

## 2. 使用

+ 调用方式：与普通函数一样，即`Generator()`;

+ 调用 Generator 函数后，==该函数并不执行==，返回的也不是函数运行结果，而是一个==指向内部状态的指针对象==

  ```javascript
  hw.next()
  // { value: 'hello', done: false }
  
  hw.next()
  // { value: 'world', done: false }
  
  hw.next()
  // { value: 'ending', done: true }
  
  hw.next()
  // { value: undefined, done: true }
  ```

> 第一次调用，Generator 函数开始执行，直到遇到第一个`yield`表达式为止。`next`方法返回一个对象，它的`value`属性就是当前`yield`表达式的值`hello`，`done`属性的值`false`，表示遍历还没有结束。
>
> ...
>
> 第四次调用，此时 Generator 函数已经运行完毕，`next`方法返回对象的`value`属性为`undefined`，`done`属性为`true`。以后再调用`next`方法，返回的都是这个值。



## 3. yield 表达式

+ `yield`表达式是`Generator函数`的暂停标志，遇到yield表达式，就把它后面表达式的值作为返回对象的value值。

![image-20220527001134289](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202205270011377.png)

+ 与return的区别：`return`语句不具备位置记忆的功能。一个函数里面，只能执行一次。

+ 不使用yield表达式即为 ==暂缓执行函数==

  ```javascript
          function fn() {
              console.log('这个是普通函数');
          }
  
          function* gene() {
              console.log('这个是generator函数');
  
          }
  
          let normal = fn(); //此时就执行了fn函数
          let geneFn = gene();//还未执行
  
          setTimeout(() => {
              geneFn.next();
          }, 2000);
  ```

+ `yield`表达式如果用在另一个表达式之中，必须放在==圆括号==里面。

+ `yield`表达式用作函数参数或放在赋值表达式的右边，可以不加括号。

  ```javascript
  function* demo() {
    foo(yield 'a', yield 'b'); // OK
    let input = yield; // OK
  }
  ```

  

## 4. 异步应用

+ 使用generator函数解决**代码冗余**的问题

### 4.1. 协程

#### a. 基本概念

意思：多个线程互相协作，完成异步任务。

> 运行流程：
>
> - 第一步，协程`A`开始执行。
> - 第二步，协程`A`执行到一半，进入暂停，执行权转移到协程`B`。
> - 第三步，（一段时间后）协程`B`交还执行权。
> - 第四步，协程`A`恢复执行。



```javascript
function* asyncJob() {
  // ...其他代码
  var f = yield readFile(fileA);
  // ...其他代码
}

//yield命令表示执行到此处，执行权将交给其他协程。
```

#### b. 函数实现

+ 整个 Generator 函数就是==一个封装的异步任务==（异步任务的容器）。

+ 异步操作需要暂停的地方，都用`yield`语句注明

```javascript
            let y = yield x + 2;
            return y;
        }

        let g = gen(1);
        console.log(g.next());// { value: 3, done: false }
        console.log(g.next()); // { value: undefined, done: true }


//，next方法的作用是分阶段执行Generator函数。每次调用next方法，会返回一个对象，表示当前阶段的信息（value属性和done属性）。value属性是yield语句后面表达式的值，表示当前阶段的值；done属性是一个布尔值，表示 Generator 函数是否执行完毕，即是否还有下一个阶段。
```

#### c. 数据交换和错误处理

+ `next`返回值的 value 属性，是 Generator 函数向外输出数据；
+ `next`方法还可以接受参数，向 Generator 函数体内输入数据。

```javascript
function* gen(x){
  var y = yield x + 2;
  return y;
}

var g = gen(1);
g.next() // { value: 3, done: false }
g.next(2) // { value: 2, done: true }

//上面代码中，第一个next方法的value属性，返回表达式x + 2的值3。第二个next方法带有参数2，这个参数可以传入 Generator 函数，作为上个阶段异步任务的返回结果，被函数体内的变量y接收。因此，这一步的value属性，返回的就是2（变量y的值）。
```

+ 捕获函数体外抛出的错误（暂时不太懂）

  ```javascript
  function* gen(x){
    try {
      var y = yield x + 2;
    } catch (e){
      console.log(e);
    }
    return y;
  }
  
  var g = gen(1);
  g.next();
  g.throw('出错了');
  // 出错了
  ```

  

#### d. 异步任务的封装

```javascript
var fetch = require('node-fetch');

function* gen(){
  var url = 'https://api.github.com/users/github';
  var result = yield fetch(url);
  console.log(result.bio);
}

var g = gen();
var result = g.next();

result.value.then(function(data){
  return data.json();
}).then(function(data){
  g.next(data);
});
```

+ 上面代码首先执行 Generator 函数，获取遍历器对象，然后使用`next`方法（第二行），==执行异步任务的第一阶段==。由于`Fetch`模块返回的是一个 Promise 对象，因此要用`then`方法调用下一个`next`方法。

#### e. next的传值

```javascript
        function* generator() {
            const a = yield 1
            console.log(`我是a: ${a}`)
        }

        const gen = generator()
        console.log(gen.next())  // { value: 1, done: false }
        console.log(gen.next(2)) // 我是a: 2 { value: undefined, done: true }
```

![image.png](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202206081629204.webp)

### 4.2 Thunk 函数

[Thunk函数 文档](https://es6.ruanyifeng.com/#docs/generator-async#Generator-%E5%87%BD%E6%95%B0)

#### a. 含义

将参数放到一个==临时函数==之中，再将这个临时函数传入函数体。这个临时函数就叫做 Thunk 函数。

#### b. 作用

Thunk 函数现在可以用于 Generator 函数的==自动==流程管理。

### 4.3 co模块

[co模块 文档](https://es6.ruanyifeng.com/#docs/generator-async#Generator-%E5%87%BD%E6%95%B0)

####  
