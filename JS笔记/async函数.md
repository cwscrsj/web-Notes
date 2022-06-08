# async函数

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
            遇到await执行里面的promise对象
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

### 1. 返回promise对象

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

