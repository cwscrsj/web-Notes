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

  