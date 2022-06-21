# Module

## 一、 概述

+ 模块（module）体系可以将一个大程序拆分成互相依赖的小文件，再用简单的方法拼装起来。

```javascript
// CommonJS模块
let { stat, exists, readfile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```

+ ES6 模块不是对象，而是通过`export`命令==显式指定输出的代码==，再==通过import命令输入==。

```javascript
// ES6模块
import { stat, exists, readFile } from 'fs';

//ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。当然，这也导致了没法引用 ES6 模块本身，因为它不是对象。
```

## 二、严格模式

ES6 的模块自动采用严格模式

![image-20220616225947862](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202206162259962.png)

## 三、export命令

### 1. 概述

模块功能主要由两个命令构成：`export`和`import`。

+ `export`命令用于规定==模块的对外接口==
+ `import`命令用于==输入其他模块提供的功能==。

### 2. 使用

+ 常用写法

  ```javascript
  // profile.js
  var firstName = 'Michael';
  var lastName = 'Jackson';
  var year = 1958;
  
  export { firstName, lastName, year };
  ```

+ `export`命令除了输出变量，还可以输出==函数==或==类==。

  ```javascript
  export function multiply(x, y) {
    return x * y;
  };
  ```

+ 可以用`as`重命名`export`输出的变量

  ```javascript
  function v1() { ... }
  function v2() { ... }
  
  export {
    v1 as streamV1,
    v2 as streamV2,
    v2 as streamLatestVersion
  };
                 
  //上面代码使用as关键字，重命名了函数v1和v2的对外接口。重命名后，v2可以用不同的名字输出两次。
  ```

  

+ `export`命令规定的是对外的接口

  ```javascript
  export var m = 1;
  
  // 写法二
  var m = 1;
  export {m};
  
  // 写法三
  var n = 1;
  export {n as m};
  
  // 报错
  function f() {}
  export f;
  
  // 正确
  export function f() {};
  
  // 正确
  function f() {}
  export {f};
  ```

### 3. export default命令

不用知道所要加载的变量名或函数名就加载模块

+ 使用`export default`时，对应的`import`语句==不需要使用大括号==；
+ `export default`命令==只能使用一次==
+ **<font color="red">`export default`命令的本质是将后面的值，赋给`default`变量</font>**

```javascript
// export-default.js
export default function foo() {
  console.log('foo');
}

// 或者写成

function foo() {
  console.log('foo');
}

export default foo;
```

+ 可以==任意取名字==

  ```javascript
  // modules.js
  function add(x, y) {
    return x * y;
  }
  export {add as default};
  // 等同于
  // export default add;
  
  // app.js
  import { default as foo } from 'modules';
  // 等同于
  // import foo from 'modules';
  ```

  


## 四、import 命令

### 1. 概述

使用`export`命令定义了模块的对外接口以后，其他 JS 文件就可以==通过import命令==加载这个模块。

### 2. 特点

+ `import`命令输入的变量都是**只读**的，但输入**对象可改写属性**

  **（ 建议凡是输入的变量，都当作完全只读，不要轻易改变它的属性。）**

+ `import`命令具有**提升效果**，会提升到整个模块的头部，首先执行。

+ `import`是静态执行，所以**不能使用表达式和变量**

+ import语句**只执行一次**

### 3. 使用

+ 引用

  ```javascript
  // main.js
  import { firstName, lastName, year } from './profile.js';
  
  function setName(element) {
    element.textContent = firstName + ' ' + lastName;
  }
  ```

+ 使用==as关键字==对输入变量重命名

  ```javascript
  import { lastName as surname } from './profile.js';
  ```

## 五、整体加载

+ 用星号（`*`）指定一个对象，所有输出值都加载在这个对象上面。

![image-20220618225451936](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202206182254037.png)

## 六、其他内容

[es6 阮一峰](https://es6.ruanyifeng.com/#docs/module#export-default-%E5%91%BD%E4%BB%A4)

| 内容                   | 作用                                                         |
| ---------------------- | ------------------------------------------------------------ |
| export和import复合写法 | 模块的接口改名和整体输出                                     |
| 模块的继承             |                                                              |
| **跨模块常量**         | 想设置跨模块的常量（即跨多个文件），或者说一个值要被多个模块共享 |

## 七、适用场景

### 1. 按需加载

`import()`可以在需要的时候，再加载某个模块。

```javascript
button.addEventListener('click', event => {
  import('./dialogBox.js')
  .then(dialogBox => {
    dialogBox.open();
  })
  .catch(error => {
    /* Error handling */
  })
});
```

### 2. 条件加载

根据不同情况加载不同模块

```javascript
if (condition) {
  import('moduleA').then(...);
} else {
  import('moduleB').then(...);
}
```

### 3. 动态的模块路径

```javascript
import(f())
.then(...);
```

上面代码中，根据函数`f`的返回结果，加载不同的模块。

## ❤ 注意点

+ 可以使用解构赋值获取输出接口

  ```javascript
  import('./myModule.js')
  .then(({export1, export2}) => {
    // ...·
  });
  ```

+ 可以使用`promise.all`加载多个模块

  ```javascript
  async function main() {
    const myModule = await import('./myModule.js');
    const {export1, export2} = await import('./myModule.js');
    const [module1, module2, module3] =
      await Promise.all([
        import('./module1.js'),
        import('./module2.js'),
        import('./module3.js'),
      ]);
  }
  main();
  ```

  
