# Iterator和for...of循环

## 1.Iterator概念

Iterator是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成==遍历操作==

## 2. 默认Iterator接口

+ Iterator 接口的目的，就是为所有数据结构，提供了一种统一的访问机制，即`for...of`循环当使用`for...of`循环遍历某种数据结构时，该循环会自动去寻找 Iterator 接口。

+ 一个数据结构只要具有`Symbol.iterator`属性，就可以认为是“可遍历的”

  

❤例：让对象可被`for...of`调用

（在`Symbol.iterator`属性上部署遍历器生成方法）

```javascript
 class RangeIterator {
            constructor(start, stop) {
                this.value = start;
                this.stop = stop;
            }

            [Symbol.iterator]() {
                return this;
            }

            next() {
                var value = this.value;
                if (value < this.stop) {
                    this.value++;
                    return { done: false, value: value };
                }
                return { done: true, value: undefined };
            }
        }

        function range(start, stop) {
            return new RangeIterator(start, stop);
        }

        for (var value of range(0, 3)) {
            console.log(value); // 0, 1, 2
        }
```

## 3.调用Iterator接口的场合

1. 解构赋值

2. 扩展运算符

3. `yield *`

   yidld后面跟的是一个可遍历的结构，会调用该结构的遍历器接口

   ```javascript
           let generator = function* () {
               yield 1;
               yield* [2, 3, 4];
               yield 5;
           };
   
           var iterator = generator();
   
           console.log(iterator.next()); // { value: 1, done: false }
           console.log(iterator.next()); // { value: 2, done: false }
           console.log(iterator.next()); // { value: 3, done: false }
           console.log(iterator.next()); // { value: 4, done: false }
           console.log(iterator.next()); // { value: 5, done: false }
           console.log(iterator.next()); // { value: undefined, done: true }
   ```

   

## 4. for...of

### 4.1 数组

`for...of`循环本质上就是调用`Iterator`接口产生的遍历器

```javascript
        const arr = ['red', 'green', 'blue'];

        for (let v of arr) {
            console.log(v); // red green blue
        }

        const obj = {};
        // 让空对象在遍历时调用arr的Iterator接口
        obj[Symbol.iterator] = arr[Symbol.iterator].bind(arr);

        for (let v of obj) {
            console.log(v); // red green blue
        }

        //空对象obj部署了数组arr的Symbol.iterator属性，结果obj的for...of循环，产生了与arr完全一样的结果。

```

+ 与forin的区别

  + `for...in`循环读取键名

  + `for...of`循环读取键值，`for...of`循环调用遍历器接口，数组的遍历器接口==只返回具有数字索引==的属性

    ```javascript
    let arr = [3, 5, 7];
    arr.foo = 'hello';
    
    for (let i in arr) {
      console.log(i); // "0", "1", "2", "foo"
    }
    
    for (let i of arr) {
      console.log(i); //  "3", "5", "7"
    }
    ```

    

### 4.2 set 和 map

+ Set 结构遍历时，**返回的是一个值**，而 Map 结构遍历时，**返回的是一个数组**，该数组的两个成员分别为当前 Map 成员的键名和键值。

```javascript
let map = new Map().set('a', 1).set('b', 2);
for (let pair of map) {
  console.log(pair);
}
// ['a', 1]
// ['b', 2]

for (let [key, value] of map) {
  console.log(key + ' : ' + value);
}
// a : 1
// b : 2
```

