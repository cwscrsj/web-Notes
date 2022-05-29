# Symbol

## 1. 概述

+ 如果使用一个他人提供的对象，又想为这个对象添加新方法可以使用symbol

+ 一种新的原始数据类型`Symbol`，表示独一无二的值。它属于 JavaScript 语言的数据类型之一

  （对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。==凡是属性名属于 Symbol 类型，就都是独一无二的==）

+ `symbol`不能使用new命令，因为symbol是一个原始类型的值，不是对象（理解为类似于字符串的数据类型）

  ```javascript
  let a=Symbol()
  ```

+ `Symbol`函数可以接受==一个字符串作为参数==，表示对 Symbol 实例的**描述**，Symbol值不能运算

```javascript
let s1 = Symbol('foo');
let s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)

//s1 s2都是symbol函数的返回值
```

+ Symbol可以显示转换为字符串

  ```javascript
  let sym = Symbol('My symbol');
          let symStr = String(sym) // 'Symbol(My symbol)'
          console.log(symStr + 'aaa'); // 'Symbol
  ```

  补充：显示类型转换：手动调用Boolean(value)、Number(value)、String(value)完成的类型转换

+ symbol可以转换为布尔值，但==不能转为数值==

### 总结

+ symbol是一种新的数据类型，通过symbol()函数创建，返回一个独一无二的值
+ symbol函数里的值是描述性文本，以便于区分，因为控制台都是symbol（）
+ 返回的值赋给一个变量，那该变量就是独一无二的值。
+ 如果symbol参数值为对象，那会转换程字符串生成值
+ symbol值不能运算
+ symbol可以转换为字符串，布尔值

## 2. Symbol.prototype.description

+ 读取symbol类型描述需要将Symbol转为字符串

  ```javascript
  const sym = Symbol('foo');
  
  String(sym) // "Symbol(foo)"
  sym.toString() // "Symbol(foo)"
  
  //使用symbol的实例属性，description
  const sym = Symbol('foo');
  
  sym.description // "foo"
  ```

## 3. symbol值作为属性

+ 通过`[ ]`或 `Object.defineProperty`

```javascript
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```

+ symbol值可以作为属性值，表示独一无二

  ```javascript
  let a={
  	BUG:symbol('haha');
  }
  ```

+ 总结：以symbol值作为某一对象属性，可以保证当该对象有多个属性或方法时，该symbol属性不会被覆盖（他是独一无二的）

## 4. 属性名的遍历

+ Symbol作为属性名，不会出现在for in，for of里，但也不是私有属性

+ `Object.getOwnPropertySymbols()`方法，获取指定对象的所有 Symbol 属性名。
  + 返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。

```javascript
 const objectSymbols = 	Object.getOwnPropertySymbols(obj);
        console.log(objectSymbols);
```

+ Symbol 值作为键名，不会被常规方法遍历得到。可以利用这个特性，为对象定义一些非私有的、但又希望只用于内部的方法。

## 5. Symbol.for(),symbol.keyFor()

+ `Symbol.for()`可以做到同一个symbol值，以其中的参数查找，==同一个参数为同一个值==，如果查找不到，就返回一个新值

+ `symbol.Keyfor() `返回一个已登记的Symbol类型的key

  ```javascript
  let s1 = Symbol.for("foo");
  Symbol.keyFor(s1) // "foo"
  
  let s2 = Symbol("foo");
  Symbol.keyFor(s2) // undefined
  //s2还未登记，两个以s2为参数的symbol值不相等
  ```

  

## 6. 内部的Symbol值

### 1. Symbol.hasInstance

+ 当内部引用`A instanceof Array`时，实际上在内部引用`Array[Symbol.hasInstance](A)`
  + 补充：类（class）通过 static关键字定义静态方法。不能在类的实例上调用静态方法，而应该通过类本身调用。

```javascript
class Even {
            static [Symbol.hasInstance](item) {
                return item % 2 == 0
            }
        }
        console.log(2 instanceof Even);
```

### 2. Symbol.isConcatSpreadable

+ 对象的`Symbol.isConcatSpreadable`属性等于一个==布尔值==，表示该对象用于`Array.prototype.concat()`时，是否可以展开。

### 3. Symbol.species

+ 对象的`Symbol.species`属性，指向一个构造函数。创建==衍生对象==（可以理解为继承的实例对象）时，会使用该属性。

​	![image-20220415195440427](https://cws201800115.oss-cn-shenzhen.aliyuncs.com/typoraimges/image-20220415195440427.png)

> 总之，`Symbol.species`的作用在于，实例对象在运行过程中，需要再次调用自身的构造函数时，会调用该属性指定的构造函数。它主要的用途是，有些类库是在基类的基础上修改的，那么子类使用继承的方法时，作者可能希望返回基类的实例，而不是子类的实例。

### 4. symbol.match

+ 对象的`Symbol.match`属性，指向一个函数。当执行`str.match(myObject)`时，如果该属性存在，会调用它，返回该方法的返回值。

### 5. 其他内置对象

+ `Symbol.replace`：

+ `Symbol.search`:
+ 。。。