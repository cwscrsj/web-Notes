# async函数

[async 阮一峰](https://es6.ruanyifeng.com/#docs/async#%E8%AF%AD%E6%B3%95)

## 一、含义

Async 函数是 Generator 函数的语法糖。

## 二、 优势

`async`函数对 Generator 函数的改进，体现在以下四点。

### 1. 内置执行器

`async`函数的执行，与普通函数一模一样，只要一行。

（不需要调用next方法就能得到最后结果）

### 2. 语义更好

`async`表示函数里有异步操作

`await`表示紧跟在后面的表达式需要等待结果。

### 3. 更好的适用性

`async`函数的`await`命令后面，可以是 ==Promise 对象和原始类型的值==（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。

### 4. 返回值为Promise

`async`函数的==返回值是 Promise 对象==，`async`函数完全可以看作多个异步操作，包装成的一个 Promise 对象

## 三、 用法

+ `async`函数返回一个 Promise 对象，可以使用`then`方法添加回调函数。
+ 当函数执行的时候，一旦遇到`await`就会先返回，等到**==异步操作完成==**，再接着执行函数体内后面的语句。

```javascript
        function timeout(ms) {
            return new Promise((resolve) => {
                setTimeout(resolve, ms);
            });
        }

        async function asyncPrint(value, ms) {
            //遇到await执行里面的promise对象
            await timeout(ms).then(res=>{
                console.log(111);
            });
            console.log(value);
        }

        asyncPrint('hello world', 50);
```

+ 常见使用场景

  ```javascript
  // 函数声明
  async function foo() {}
  
  // 函数表达式
  const foo = async function () {};
  
  // 对象的方法
  let obj = { async foo() {} };
  obj.foo().then(...)
  
  // Class 的方法
  class Storage {
    constructor() {
      this.cachePromise = caches.open('avatars');
    }
  
    async getAvatar(name) {
      const cache = await this.cachePromise;
      return cache.match(`/avatars/${name}.jpg`);
    }
  }
  
  const storage = new Storage();
  storage.getAvatar('jake').then(…);
  
  // 箭头函数
  const foo = async () => {};
  ```

  

## 四、 语法

`async`函数的语法规则总体上比较简单，难点是==错误处理机制==。

### 1.  返回promise对象

+ `async`函数返回一个 Promise 对象。
+ `async`函数==内部`return`语句返回的值，会成为`then`方法回调函数的参数。==

```javascript
async function f() {
  return 'hello world';
}

f().then(v => console.log(v))
// "hello world"
```

+ `async`函数内部抛出错误， Promise 对象变为`reject`状态。抛出的错误对象会被`catch`方法回调函数接收到。

```javascript
        async function f() {
            throw new Error('出错了');
        }

        f().then(
            // v,e分别代表成功和报错的信息
            v => console.log('resolve', v),
        ).catch(
            e => console.log('reject', e)
        )
```

### 2. Promise对象的状态变化

`async`函数==内部的异步操作执行完==，才会执行`then`方法指定的回调函数。

```javascript
async function getTitle(url) {
  let response = await fetch(url);
  let html = await response.text();
  return html.match(/<title>([\s\S]+)<\/title>/i)[1];
}
getTitle('https://tc39.github.io/ecma262/').then(console.log)
// "ECMAScript 2017 Language Specification"
```

### 3. await 命令

+ 如果`awaith`后面为`Promise`对象，返回该对象结果，如果不是则直接返回对应值。

```javascript
async function f() {
  // 等同于
  // return 123;
  return await 123;
}

f().then(v => console.log(v))
// 123
```

+ `await`命令后面的 Promise 对象变为`reject`状态，则`reject`的参数会被==catch方法的回调函数接收到==。

```javascript
async function f() {
  await Promise.reject('出错了');
}

f()
.then(v => console.log(v))
.catch(e => console.log(e))
// 出错了

//注意，上面代码中，await语句前面没有return，但是reject方法的参数依然传入了catch方法的回调函数。这里如果在await前面加上return，效果是一样的。
```

+ **任何一个`await`语句后面的 Promise 对象变为`reject`状态，那么整个`async`函数都会中断执行。**

```javascript
async function f() {
  await Promise.reject('出错了');
  await Promise.resolve('hello world'); // 不会执行
}
```

+ 可以使用 `try...catch`不管异步操作是否成功都执行第二个await （或在await后面的`Promise`对象跟一个`catch`方法）

```javascript
async function f() {
  try {
    await Promise.reject('出错了');
  } catch(e) {
  }
  return await Promise.resolve('hello world');
}

f()
.then(v => console.log(v))
// hello world
```

### 4. 错误处理

解决方法：

+ 将`await`放在`try...catch`代码块中

  ```javascript
  async function main() {
    try {
      const val1 = await firstStep();
      const val2 = await secondStep(val1);
      const val3 = await thirdStep(val1, val2);
  
      console.log('Final: ', val3);
    }
    catch (err) {
      console.error(err);
    }
  }
  ```

  

+ 多次反复尝试

  ```javascript
  const superagent = require('superagent');
  const NUM_RETRIES = 3;
  
  async function test() {
    let i;
    for (i = 0; i < NUM_RETRIES; ++i) {
      try {
        await superagent.get('http://google.com/this-throws-an-error');
        break;
      } catch(err) {}
    }
    console.log(i); // 3
  }
  
  test();
  
  //上面代码中，如果await操作成功，就会使用break语句退出循环；如果失败，会被catch语句捕捉，然后进入下一轮循环。
  ```

### 5. 使用注意点

+ 最好把`await`命令放在`try...catch`代码块里
+ 多个await命令可以同时触发（`all`）

```javascript
let foo = await getFoo();
let bar = await getBar();


// 写法一
let [foo, bar] = await Promise.all([getFoo(), getBar()]);

// 写法二
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bar = await barPromise;
```

+ await命令只能用在async函数中

+ [堆栈问题](https://es6.ruanyifeng.com/#docs/async#%E8%AF%AD%E6%B3%95)（暂时不懂）

## 五、async实现原理

+ 将 Generator 函数和自动执行器，包装在一个函数里。

//spawn函数（暂时不懂）

```javascript
function spawn(genF) {
  return new Promise(function(resolve, reject) {
    const gen = genF();
    function step(nextF) {
      let next;
      try {
        next = nextF();
      } catch(e) {
        return reject(e);
      }
      if(next.done) {
        return resolve(next.value);
      }
      Promise.resolve(next.value).then(function(v) {
        step(function() { return gen.next(v); });
      }, function(e) {
        step(function() { return gen.throw(e); });
      });
    }
    step(function() { return gen.next(undefined); });
  });
}
```

## 六、异步处理比较

| 操作方法  | 优点                                                         | 缺点                                          |
| --------- | ------------------------------------------------------------ | --------------------------------------------- |
| Promise   | 相比于回调函数写法改进                                       | then,catch,无法看出操作语义                   |
| Generator | 语义比 Promise 写法更清晰，用户定义的操作全部都出现在`spawn`函数的内部 | 必须有一个任务运行器，自动执行 Generator 函数 |
| Async     | 实现最简洁，最符合语义，几乎没有语义不相关的代码。           | 暂时不知道 😀                                  |

## 七、实例

+ 依次远程读取一组 URL，然后按照读取的顺序输出结果。

思路：==并发执行获取url的信息==，传入map当中。通过forof继发按顺序输出。（在`async`函数内部是继发执行，外部不受影响）

```javascript
async function logInOrder(urls) {
  // 并发读取远程URL
  const textPromises = urls.map(async url => {
    const response = await fetch(url);
    return response.text();
  });

  // 按次序输出
  for (const textPromise of textPromises) {
    console.log(await textPromise);
  }
}
```

