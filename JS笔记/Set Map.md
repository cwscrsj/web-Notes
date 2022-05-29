# Set 

一种新的类似于数组的数据结构

## 1. 基本特点

+ ==类似于数组，成员的值都是唯一的，没有重复值==

  **（与数组的最大区别）**

  ```javascript
   let a = [1, 4, 3, 1, 2, 3, 4, 5, 2, 1, 4, 3];
          let g = new Set(a);
          console.log(g);//1 4 3 2 5
  ```

  

+ Set本身是一个构造函数，通过new创建

+ set可以通过传递一个数组作为参数初始化

+ 向set增加值时，==不会发生类型转换==，如'5' 和 5

## 2.用法

+ `add()`用于增添元素（无法增添重复值）

+ 去除数组或字符串重复元素,返回新数组(根据其特性)

  ```javascript
          let a = [1, 4, 3, 1, 2, 3, 4, 5, 2, 1, 4, 3];
          let g = [...new Set(a)];//。。。为扩展运算符
          console.log(g);//1 4 3 2 5
  
  ```

## 3. 实例的属性及方法

+ Set的属性
  + `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
  + `Set.prototype.size`：返回`Set`实例的==成员总数==。
+ Set的方法
  + `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。
  + `Set.prototype.delete(value)`：删除某个值，==返回一个布尔值==，表示删除是否成功。
  + `Set.prototype.has(value)`：==返回一个布尔值==，表示该值是否为`Set`的成员。
  + `Set.prototype.clear()`：==清除所有成员==，没有返回值。

+ `Array.from`可以将set结构转为数组

  ```javascript
  const items = new Set([1, 2, 3, 4, 5]);
  const array = Array.from(items);
  ```

## 4. 遍历

###  1.keys(),values()

+ `keys()`返回键名，`values()`返回键值由于Set结构没有键名，这两的方法行为一致

+ 所以直接使用`for of g`就可以了

  ```javascript
  let a = [1, 4, 3, 1, 2, 3, 4, 5, 2, 1, 4, 3];
          let g = new Set(a);
          for (let item of g.keys()) {
              console.log(item);
          }
  ```

### 2. entries()

+ 同时返回==键名==和==键值==以数组的形式。

  ```javascript
  let a = [1, 4, 3, 1, 2, 3, 4, 5, 2, 1, 4, 3];
          let g = new Set(a);
          for (let item of g.entries()) {
              console.log(item);
              console.log(item instanceof Array);//true
          }
  ```

### 3. forEach()

+ 用于对某个成员指执行某种操作，没有返回值

  （就单纯操作一下）

  ```javascript
          const arr = [2, 5, 1, 4];
          arr.forEach((key, value) => {
              console.log(key, value);// 索引号，数组值
          });
  
          const arrSet = new Set(arr);
          arrSet.forEach((key, value) => {
              console.log(key, value);
          })
  ```

  

### 4. 遍历的应用

+ 用map和filter可以用于更改或筛选set

```javascript
let set = new Set([1, 2, 3]);
set = new Set([...set].map(x => x * 2));
// 返回Set结构：{2, 4, 6}

let set = new Set([1, 2, 3, 4, 5]);
set = new Set([...set].filter(x => (x % 2) == 0));
// 返回Set结构：{2, 4}
```

+ 利用Set找并集，交集

  ```javascript
  let a = new Set([1, 2, 3]);
  let b = new Set([4, 3, 2]);
  
  // 并集
  let union = new Set([...a, ...b]);
  // Set {1, 2, 3, 4}
  
  // 交集
  let intersect = new Set([...a].filter(x => b.has(x)));
  // set {2, 3}
  
  // （a 相对于 b 的）差集
  let difference = new Set([...a].filter(x => !b.has(x)));
  // Set {1}
  ```

## 5. WeakSet

+ WeakSet的成员只能是对象

### 5.1 创建

+ 与set类似，用new创建

  ```javascript
  let arr1=[];
  let arr2=[];
  
  let g=new WeakSet(arr1,arr2);
  ```

### 5.2 方法

![image-20220415221239582](https://cws201800115.oss-cn-shenzhen.aliyuncs.com/typoraimges/image-20220415221239582.png)

### 5.3 特点

+ 没有size属性，没有办法遍历他的成员

# Map

## 1. 基本概念

+ 类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键

## 2. 用法

```javascript
        const o = {};
        const m = new Map();
        m.set(o, 'content');
        console.log(m);
        console.log(m.get(o));//content

// key:o对象  value：‘content'

```

+ 通过set方法增加键值
+ 通过get获取他的键，从而得到他的值，也可以用delete，has

+ 可以给map参数里设置数组，第一个值代表键，第二个代表值

## 3. 特点

+ 对同一个键多次赋值，后面的值会覆盖前面的

+ Map 的键实际上是跟内存地址绑定的，只要==内存地址不一样，就视为两个键。==

  ```javascript
  const map = new Map();
  
  const k1 = ['a'];
  const k2 = ['a'];
  
  map
  .set(k1, 111)
  .set(k2, 222);
  
  map.get(k1) // 111
  map.get(k2) // 222
  ```

+ 如果键是一个简单数据类型，则完全相等时视为同一个键

## 4. 实例属性和操作方法

+ size：返回成员总是
+ `Map.prototype.set(key, value)`：设置键名返回整个map结构
+ `Map.prototype.get(key)`:获取键，返回值
+ `Map.prototype.has(key)`：返回布尔值，表示是否在Map当中

+ delete/clear

## 5. 遍历方法

+ 可用扩展运算符转换为数组

  ```javascript
  const map = new Map([
    [1, 'one'],
    [2, 'two'],
    [3, 'three'],
  ]);
  
  [...map.keys()]
  // [1, 2, 3]
  
  [...map.values()]
  //one,two,three
  ```

+ 结合map方法，filter方法，实现map的遍历和过滤

  ```javascript
  const map0 = new Map()
    .set(1, 'a')
    .set(2, 'b')
    .set(3, 'c');
  
  const map1 = new Map(
    [...map0].filter(([k, v]) => k < 3)
  );
  // 产生 Map 结构 {1 => 'a', 2 => 'b'}
  
  const map2 = new Map(
    [...map0].map(([k, v]) => [k * 2, '_' + v])
      );
  // 产生 Map 结构 {2 => '_a', 4 => '_b', 6 => '_c'}
  ```

  

+ ==遍历时在控制台输出的是多个两个参数的数组==

+ 用` for of ` 获取值

  + `Map.prototype.keys()`：返回键名的遍历器。
  + `Map.prototype.values()`：返回键值的遍历器。
  + `Map.prototype.entries()`：返回所有成员的遍历器。
  + `Map.prototype.forEach()`：遍历 Map 的所有成员。

```javascript
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);//返回的是数组
}
// "F" "no"
// "T" "yes"
```

## 6. 应用

+ map可以转换成对象

  （用for of来做）

  ```javascript
   const myMap = new Map()
              .set('yes', true)
              .set('no', false);
  
          let obj = {};
          for (let [x, y] of myMap) {
              obj[x] = y;
          }
  
          console.log(obj);
  ```

## 7.WeakMap

==WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名。==

### 1. 创建

+ 与Map类似，通过new产生

### 2. 特点

+ 没有遍历操作（即没有`keys()`、`values()`和`entries()`方法），也没有`size`属性。
+ 只接受对象作为键名，不接受其他类型的值作为键名。

### 3. 用途

+ `WeakMap`的专用场合就是，它的键所对应的对象，可能会在将来消失。`WeakMap`结构有助于防止内存泄漏。

+ 具体在文档



# 数组中的map方法

+ map() 方法==返回一个新数组==，数组中的元素为原始数组元素调用函数处理后的值。

  map() 方法按照原始数组元素顺序依次处理元素。

# weak的应用场景

基本上，如果你要往对象上添加数据，又不想干扰垃圾回收机制，就可以使用 `WeakMap`。一个典型应用场景是，在网页的 DOM 元素上添加数据，就可以使用`WeakMap`结构。当该 DOM 元素被清除，其所对应的`WeakMap`记录就会自动被移除。

[csdn weakmap详解](https://blog.csdn.net/qq_32925031/article/details/111032188?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-111032188-blog-102106146.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-111032188-blog-102106146.pc_relevant_default&utm_relevant_index=1)