# Proxy

文档：[ES6 Proxy 阮一峰](https://es6.ruanyifeng.com/#docs/proxy) 

## 1.概述

+ Proxy 用于==修改==某些操作的==默认行为==，等同于在语言层面做出修改
+ 在目标对象之前架设一层==“拦截”==，外界对该对象的访问，都必须先通过这层拦截

```javascript
let obj = new Proxy({}, {
            get: function (target, propKey, receiver) {
                console.log(`getting ${propKey}!`);
                return Reflect.get(target, propKey, receiver);
            },
            set: function (target, propKey, value, receiver) {
                console.log(`setting ${propKey}!`);
                return Reflect.set(target, propKey, value, receiver);
            }
        });

        obj.count = 1;
		//  setting count!
		++obj.count
		//  getting count!
		//  setting count!
        console.log(obj.count);
```

## 2.生成

+ 通过new的方式生成实例

  ```javascript
  let proxy = new Proxy(target, handler);
  ```

+ `target`参数表示所要拦截的==目标对象==，`handler`参数也是一个对象，用来==定制拦截行为==。

### 2.1 例子

```javascript
       let proxy = new Proxy({}, {
            get: function (target, propKey) {
                return 20;
            }
        })

        // 当调用对象的时候，会执行get里面的操作
        console.log(proxy.age);

		//第一个参数是需要代理的目标对象，第二个是一个配置对象，通过提供的处理函数来拦截对应操作
```

+ get方法用来拦截访问请求，`target`和`propKey`分别为==目标对象==与==要访问的属性==

### 2.2 注意

+ 使得`Proxy`起作用，必须针对==Proxy实例==进行操作，而不是针对目标对象（上例是空对象）进行操作。

+ 如果`handler`没有设置任何拦截，那就等同于直接通向原对象。

  ```javascript
  let target = {};
  let handler = {};
  let proxy = new Proxy(target, handler);
  proxy.a = 'b';
  console.log(target.a) // "b"
  ```

+ 作为其他对象的原型对象

  ```javascript
          let proxy = new Proxy({}, {
              get: function (target, propKey) {
                  return 35;
              }
          });
  
          let obj = {};
          // 创建一个proxy的实例对象
          obj = Object.create(proxy);
          obj.age = 18;
          //  age代表对象本来的属性
          console.log(obj.age, obj.time);
          //18 35
  ```

  + 上面代码中，`proxy`对象是`obj`对象的原型，`obj`对象本身并没有`time`属性，所以根据原型链，会在`proxy`对象上读取该属性，导致被拦截。

### 2.3 拦截操作

![image-20220516234751754](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202205162347875.png)

## 3. Proxy实例方法

### 3.1 get()

+ `get`方法用于拦截某个属性的读取操作，可以接受三个参数：==目标对象==、==属性名==和 ==proxy 实例本身(可选)==

#### 3.1.1 基本使用

```javascript

		let student = {
            name: '小马',
            like: 'felix',
            age: '19',
        }

        let proxy = new Proxy(student, {
            get: function (target, propKey) {
                if (propKey in target) {
                    // 输出属性的内容
                    console.log(target[propKey]);
                } else {
                    //如果没有该属性，弹出一个自定义内容的错误
                    throw new ReferenceError("Prop name \"" + propKey + "\" does not exist.");
                }
            }
        })

        proxy.name;
        proxy.none;

		//不调用拦截函数
		console.log(student.name)//'小马'
		console.log(student.none)// 'undefined'
```

![image-20220517145955731](https://raw.githubusercontent.com/cwscrsj/typoraImges/main/img/202205171459780.png)

+ 如果访问目标对象不存在的属性，会==抛出一个错误==。如果没有这个拦截函数，访问不存在的属性，只会返回`undefined`。

  

#### 3.1.2 继承

1. **将拦截操作定义在prototype对象上面**

   ```javascript
    let proto = new Proxy({}, {
               get(target, propertyKey, receiver) {
                   console.log('GET ' + propertyKey);
                   return target[propertyKey];
               }
           });
   
           let obj = Object.create(proto);
           // 让obj对象本身有一个属性
           obj.name = '小马';
           
           obj.none // "GET none"
           console.log(obj.name);//'小马'
   ```

   + 上面代码中，拦截操作定义在`Prototype`对象上面，所以如果读取==obj对象继承的属性时==，拦截会生效。

2. **使用get拦截，实现数组读取负数的索引**

   ```javascript
           function createArray(...elements) {
               let arr = [];
               // 定义一个负责拦截的函数
               let handler = {
                   get(target, propKey, receiver) {
                       console.log(typeof propKey);//表明传递过来的索引号是string类型
                       // 将索引号转变为可以操作的数字
                       let index = Number(propKey);
                       if (index < 0) {
                           propKey = String(target.length + index);
                       }
   
                       return Reflect.get(target, propKey, receiver);
                   }
               }
   
               arr.push(...elements);
               return new Proxy(arr, handler);
           }
   
           let arr = createArray('a', 'b', 'c');
           console.log(arr[-2]); // a
   
   ```

#### 3.1.3 receiver(指向对象本身)

```javascript
        const proxy = new Proxy({}, {
            get: function (target, key, receiver) {
                return receiver;
            }
        });

        const d = Object.create(proxy);
        // 给d设定一个自带的属性
        d.a = 12;
        console.log(d.a === d,d.c===d);//false true
```

+ `d`对象本身没有`c`属性，所以读取`d.c`的时候，会去`d`的原型`proxy`对象找。这时，`receiver`就指向`d`，代表==原始的读操作所在的那个对象==。

#### 3.1.4 注意

如果一个属性==不可配置（configurable）==且==不可写（writable）==，则 Proxy 不能修改该属性，否则通过 Proxy 对象访问该属性会报错。

### 3.2 set()

+ `set`方法用来拦截某个属性的==赋值操作==，可以接受四个参数，依次为目标对象、属性名、属性值和 Proxy 实例本身(可选)

+ 保证输入的参数符合条件,利用`set`方法，还可以数据绑定，即每当对象发生变化时，会自动更新 DOM。

  ```javascript
          let validator = {
              set: function (obj, prop, value) {
                  if (prop === 'age') {
                      if (!Number.isInteger(value)) {
                          throw new TypeError('The age is not an integer');
                      }
                      if (value > 200) {
                          throw new RangeError('The age seems invalid');
                      }
                  }
  
                  // 对于满足条件的 age 属性以及其他属性，直接保存
                  obj[prop] = value;
                  return true;
              }
          };
  
          let person = new Proxy({}, validator);
  
          person.age = 100;
  
          console.log(person.age);//100
          person.age = 'young' // 报错
          person.age = 300 // 报错
  
  ```

+ 防止内部属性被外部读写（==内部属性名的第一个字符使用下划线开头==）

```javascript

        const handler = {
            get(target, key) {
                invariant(key, 'get');
                return target[key];
            },
            set(target, key, value) {
                invariant(key, 'set');
                target[key] = value;
                return true;
            }
        };
        function invariant(key, action) {
            if (key[0] === '_') {
                throw new Error(`Invalid attempt to ${action} private "${key}" property`);
            }
        }
        const target = {};
        const proxy = new Proxy(target, handler);
        proxy._prop
        // Error: Invalid attempt to get private "_prop" property
        proxy._prop = 'c'
		// Error: Invalid attempt to set private "_prop" property   

```

### 3.3 apply() 

+ `apply`方法拦截函数的调用、==call apply操作==

+ `apply`方法可以接受三个参数，分别是==目标对象==、==目标对象==的==上下文对象==（`this`）和目标对象的参数数组。



```javascript
        let target = function () {
            console.log('猜猜我是谁');
        }
        let handler = {
            apply: function () {
                console.log('我是笨蛋小马');
            }
        }

        let proxy = new Proxy(target, handler);
        proxy();//我是笨蛋小马
```

变量`p`是 Proxy 的实例，当它作为函数调用时（`p()`），就会被`apply`方法拦截，返回一个字符串。

### 3.4 has()

+ `has()`方法用来==拦截HasProperty操作==，即判断对象是否具有某个属性时，这个方法会生效。典型的操作就是`in`运算符。
+ `has()`方法可以接受两个参数，分别是==目标对象==、==需查询的属性名。==



```javascript
let handler = {
  has (target, key) {
    if (key[0] === '_') {
      return false;
    }
    return key in target;
  }
};
let target = { _prop: 'foo', prop: 'foo' };
let proxy = new Proxy(target, handler);
'_prop' in proxy // false
```

如果原对象的属性名的第一个字符是下划线，`proxy.has()`就会返回`false`，从而不会被`in`运算符发现。

### 3.5 其他方法

| 方法                       | 作用                                                         |
| -------------------------- | ------------------------------------------------------------ |
| construct()                | `construct()`方法用于拦截`new`命令                           |
| deleteProperty()           | 用于拦截`delete`操作                                         |
| defineProperty()           | 拦截了`Object.defineProperty()`操作                          |
| getOwnPropertyDescriptor() | 拦截`Object.getOwnPropertyDescriptor()`，返回一个属性描述对象或者`undefined`。 |
| 。。。                     | 。。。                                                       |

## 4. proxy.revocable()

+ `Proxy.revocable()`方法返回一个可取消的 Proxy 实例。

```javascript
        let target = {};
        let handler = {};

        let { proxy, revoke } = Proxy.revocable(target, handler);

        proxy.foo = 123;
        proxy.foo // 123

        revoke();
        proxy.foo // TypeError: Revoked
```

## 5. this指向问题

+ 目标对象内部的this会指向Proxy代理

  ```javascript
          let target = {
          };
          let handler = {
              get: function (target, propKey) {
                  // console.log(this);
                  return this;
              }
          }
  
          let proxy = new Proxy(target, handler);
          console.log(proxy.abc);//显示的是proxy代理
  ```

+ 有些原生对象的内部属性，只有通过正确的`this`才能拿到，所以 Proxy 也无法代理这些原生对象的属性。

  ```javascript
  const target = new Date();
  const handler = {};
  const proxy = new Proxy(target, handler);
  
  proxy.getDate();
  // TypeError: this is not a Date object.
  
  //getDate()方法只能在Date对象实例上面拿到，
  ```

  + 解决方案：让this绑定原始对象

  ```javascript
  const target = new Date('2015-01-01');
  const handler = {
    get(target, prop) {
      if (prop === 'getDate') {
          //通过bind更改this指向
        return target.getDate.bind(target);
      }
      return Reflect.get(target, prop);
    }
  };
  const proxy = new Proxy(target, handler);
  
  proxy.getDate() // 1
  ```

  
