# Promise

文档：[ES6 Promise 阮一峰](https://es6.ruanyifeng.com/#docs/promise)

## 1. 基本含义

+ 简单理解：一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。

## 2. 特点

1. **对象的状态不受外界影响**。任何其他操作都不能改变这个状态
2. **promise有三种状态**：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。====
2. **一旦状态改变，就不会再变，任何时候都可以得到这个结果。**

+ 有了`Promise`对象，就可以==将异步操作以同步操作的流程表达出来==，避免了层层嵌套的回调函数。

## 3. 基本用法

+ promise对象是一个构造函数，用来生成promise实例

  ```javascript
  const promise = new Promise(function(resolve, reject) {
    // ... some code
  
    if (/* 异步操作成功 */){
      resolve(value);
    } else {
      reject(error);
    }
  });
  ```

### 3.1 resolve / reject

+ **`resolve`函数：**将`Promise`对象的状态从==“未完成”==变为==“成功”==，在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；

+ **`reject`函数**：将`Promise`对象的状态从“未完成”变为“失败”

### 3.2 then方法

+ 第一个回调函数是`Promise`对象的状态变为`resolved`时调用，
+ 第二个回调函数是`Promise`对象的状态变为`rejected`时调用。这两个函数都是==可选的==，

### 3.3 实例

+ promise是新建后就立即执行

  ```javascript
          let promise = new Promise(function (resolve, reject) {
              console.log('Today is a good day');
              //执行一下成功后的回调函数
              resolve();
          })
  
          promise.then(function () {
              console.log('resolved.');
          });
  
          console.log('Hi!');
  
          // Today is a good day
          // Hi!
          // resolved.
  
  ```

+ 将另一个异步操作作为参数传入resolve函数

+ 调用`resollve`或`reject`并不会终结promise的执行

  ```javascript
  new Promise((resolve, reject) => {
    resolve(1);
    console.log(2);
  }).then(r => {
    console.log(r);
  });
  // 2
  // 1
  ```




**❤例：异步加载图片**

```javascript
        function loadImageAsync(url) {
            return new Promise(function (resolve, reject) {
                const image = new Image();

                image.onload = function () {
                    resolve(image);
                };

                image.onerror = function () {
                    reject(new Error('Could not load image at ' + url));
                };

                image.src = url;
            });
        }

        const url = 'http://175.178.193.182:8080/public/img/upload_fb238f55a08fb26bc5d022bea08b79bb.jpg';
        loadImageAsync(url).then((res) => {
            console.log(res);
            //返回图片结构
        })

```

![image-20220520001524575](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202205200015677.png)

+ 

## 4. 原型对象的方法

### 4.1 promise.prototype.then()

`then`方法返回一个新的promise实例

**❤例：实现链式的then()**

（前一个回调执行完，将参数返回给下一个then执行）

```javascript
getJSON("/post/1.json").then(
  post => getJSON(post.commentURL)
).then(
  comments => console.log("resolved: ", comments),
  err => console.log("rejected: ", err)
);


getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL);
}).then(function (comments) {
  console.log("resolved: ", comments);
}, function (err){
  console.log("rejected: ", err);
});
```

### 4.2 promise.prototype.catch()

`Promise.prototype.catch()`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

+ 异步操作对象状态变为`resolved`，调用then(),
+ 异步操作对象状态变为`rejected`，调用catch(),

```javascript
getJSON('/post/1.json').then(function(post) {
  return getJSON(post.commentURL);
}).then(function(comments) {
  // some code
}).catch(function(error) {
  // 处理前面三个Promise产生的错误
});

一个由getJSON()产生，两个由then()产生。它们之中任何一个抛出的错误，都会被最后一个catch()捕获。
```

**❤注意**：

1. 使用catch而不使用reject的回调函数是因为，==catch可以捕获前面then方法执行中的错误==，更接近同步的写法（`try/catch`）。

2.  Promise 内部的错误不会影响到 Promise 外部的代码，即：==就算有错误还是会继续执行下去==

   ```javascript
   const someAsyncThing = function() {
     return new Promise(function(resolve, reject) {
       // 下面一行会报错，因为x没有声明
       resolve(x + 2);
     });
   };
   
   someAsyncThing()
   .catch(function(error) {
     console.log('oh no', error);
   })
   .then(function() {
     console.log('carry on');
   });
   // oh no [ReferenceError: x is not defined]
   // carry on
   
   上面代码运行完catch()方法指定的回调函数，会接着运行后面那个then()方法指定的回调函数。如果没有报错，则会跳过catch()方法。
   ```

### 4.3 promise.prototype.finally()

+ `finally()`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。

+ **本质上是then方法的实例**

```javascript
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});

//可以用来执行后就关闭服务器
```

## 5. promise的方法

### 5.1 promise.all()

+ 将多个promise实例包装成一个新的Promise实例

  ```javascript
  const p = Promise.all([p1, p2, p3]);
  ```

  1. 只有`p1`、`p2`、`p3`的状态**都变成`fulfilled`，`p`的状态才会变成`fulfilled`**，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。

  2. 只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。



❤例：获取书籍信息和作者信息

```javascript
const databasePromise = connectDatabase();

const booksPromise = databasePromise
  .then(findAllBooks);

const userPromise = databasePromise
  .then(getCurrentUser);

Promise.all([
  booksPromise,
  userPromise
])
.then(([books, user]) => pickTopRecommendations(books, user));
```

### 5.2 promise.race()

+ `Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

  ```javascript
   const p = Promise.race([p1, p2, p3]);
  ```

  1. 只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。
  2. 率先改变的 Promise 实例的返回值会传递给`p`的回调函数。

### 5.3 其他方法

| 名称                  | 特点                                                         |
| --------------------- | ------------------------------------------------------------ |
| `promise.allSettle()` | 用来确定一组异步操作是否都结束了（不管成功或失败）           |
| `promise.any()`       | 一个变成`fulfilled`状态，包装实例就会变成`fulfilled`状态；如果所有参数实例都变成`rejected`状态，包装实例就会变成`rejected`状态。（与`promise.race()`类比） |
| `promise.resolve`     | 将现有对象转为 Promise 对象,状态为resolved                   |
| `promise.rejected`    | 返回一个新的 Promise 实例，该实例的状态为`rejected`。        |
| ❤**`promise.try()`**  | 让同步函数同步执行，异步函数异步执行，并且让它们具有统一的 API |

## 6. 应用

+ **加载图片**:一旦加载完成，promise的状态就发送变化

  （可以用于实现瀑布流）

  ```javascript
  const preloadImage = function (path) {
    return new Promise(function (resolve, reject) {
      const image = new Image();
      image.onload  = resolve;
      image.onerror = reject;
      image.src = path;
    });
  };

+ **Generator函数与Promise的结合**

  ```javascript
  function getFoo () {
    return new Promise(function (resolve, reject){
      resolve('foo');
    });
  }
  
  const g = function* () {
    try {
      const foo = yield getFoo();
      console.log(foo);
    } catch (e) {
      console.log(e);
    }
  };
  
  function run (generator) {
    const it = generator();
  
    function go(result) {
      if (result.done) return result.value;
  
      return result.value.then(function (value) {
        return go(it.next(value));
      }, function (error) {
        return go(it.throw(error));
      });
    }
  
    go(it.next());
  }
  
  run(g);
  ```

  
