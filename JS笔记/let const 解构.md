# let const 解构 

## 1. let

### 1.1 用法

类似于var，但只在let命令所在的代码块有用

### 1.2 特点

+ 不能重定义

  ```javascript
  // 报错
  function func() {
    let a = 10;
    let a = 1;
  }
  ```

+ 暂时性死区

  > 只要块级作用域内存在`let`命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。

  + 即若在某一处let声明一个变量，则在这一处代码上方若使用该变量会报错，该部分称为暂时性死区

    ```javascript
    a = 4;
            //报错： Cannot access 'a' before initialization
    
            let a = 3;
    ```

+ 不存在变量提升

### 1.3 在for循环中的let

``` javascript
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
```

+ 每进行一次循环，相当于创建一个局部作用域，创建一个新变量i，由于js引擎会记住上一轮循环i的值，所以i的值可以跟随循环递增，同时因为当前的i只在本轮循环有用，所以当调用该函数时会沿着作用域链查找到i的值，从而保证i不是10。

## 2. 块级作用域

+ ES6 允许块级作用域的任意嵌套。
+ 允许在块级作用域之中声明函数（类似于了let，在其他块级作用域不可调用）
+ **==对象不构成单独的作用域==**

## 3. const

### 3.1 用法

与let类似，但声明一个只读的常量

### 3.2 特点

+ ==声明时需要赋值==

+ 在块级作用域在有效（与let类似）

+ 本质上就是保存指向的内存地址，

  （个人理解：保存的是栈中的那个数据，简单数据类型因为保存在上面的都是具体数值，由于对象，数组在栈保存的都是指向堆空间的地址，所以改变对象中的内容时，地址不会改变，故不会报错）

## 4. 解构

+ 从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

### 4.1 数组解构

+ 基本形式

```javascript
let [a, b, c] = [1, 2, 4];
console.log(a, b, c);//1 2 4
```

+ 若解构不成功，即数组元素不匹配，变量值为undefined
+ 允许不完全解构，

```javascript
let [x, y] = [1, 2, 3];
x // 1
y // 2
```

### 4.2 对象解构赋值

+ 数组的元素是按次序排列的，变量的取值由它的位置决定；而==对象的属性没有次序，变量必须与属性同名==，才能取到正确的值。 

```javascript
  let obj = {
            name: 'Jack',
            age: '13',
            saysomething: () => {
                console.log('saysomething');
            }

        }
        let { name, saysomething, age } = obj;
        saysomething();//saysomething
        console.log(name);//Jack
```

+ 对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。

  ```javascript
  let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
  baz // "aaa"
  foo // error: foo is not defined
  ```

### 4.3 默认值

+ 可以在解构的时候指定初始值

+ 对于数组来说，一般在最后一个赋初始值，

  （数组元素是按照顺序解构的）

  ```javascript
  let [x, y] = [1, 2, 3];
  x // 1
  y // 2
  ```

+ 对于对象 默认值生效的条件是，==对象的属性值严格等于undefined。==

  ```javascript
  var {x = 3} = {x: undefined};
  x // 3
  ```

### 4.4 其他解构

+ 字符串也可以解构

  ```javascript
  const [a, b, c, d, e] = 'hello';
  a // "h"
  b // "e"
  c // "l"
  d // "l"
  e // "o"
  ```

+ 数组属于特殊对象，也可以运用对象的解构方法

+ 函数参数的解构

  ```javascript
  function add([x, y]){
    return x + y;
  }
  
  add([1, 2]); // 3
  ```

### 4.5 用途

+ 交换变量的值
+ 从函数返回多个值

