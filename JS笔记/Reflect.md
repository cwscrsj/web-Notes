# Reflect 	

## 1. 概述

与Proxy对象一样，为了操作对象而提供的新API

## 2.目的

1. 从Reflect对象上拿到语言内部的方法

2. 修改某些`object`方法的返回结果

   (`defineProperty`为给对象添加属性，类似`obj.Property='111'`)

   ```javascript
   try {
               Object.defineProperty(target, property, attributes);
               // success
           } catch (e) {
               console.log(e);
               // failure
           }
   
           // 新写法
           if (Reflect.defineProperty(target, property, attributes)) {
               // success
           } else {
               
               // failure
           }
   ```

3. **让object都变成函数行为**

   ```javascript
   // 老写法
   'assign' in Object // true
   
   // 新写法
   Reflect.has(Object, 'assign') // true
   ```

4. **不管Proxy怎么修改默认行为，在Reflect上都能获取默认行为**

   (下方代码作用：==保证原生行为能够正常执行。添加的工作，就是将每一个操作输出一行日志。==)

   ```javascript
           let target = {};
           let proxy = new Proxy(target, {
               set: function (target, name, value, receiver) {
                   var success = Reflect.set(target, name, value, receiver);
                   if (success) {
                       console.log('property ' + name + ' on ' + target + ' set to ' + value);
                   }
                   return success;
               }
           });
           proxy.a = '15';
   
           //property a on [object Object] set to 15
   ```

   

## 3. 静态方法

+ | 方法                                           | 特点                                                         |
  | ---------------------------------------------- | ------------------------------------------------------------ |
  | Reflect.apply(target, thisArg, args)           | 等同于`Function.prototype.apply.call(func, thisArg, args)`，用于绑定`this`对象后执行给定函数。 |
  | Reflect.construct(target, args)                | `Reflect.construct`方法等同于`new target(...args)`           |
  | Reflect.get(target, name, receiver)            | 查找并返回`target`对象的`name`属性，如果没有该属性，则返回`undefined`。 |
  | Reflect.set(target, name, value, receiver)     | 设置`target`对象的`name`属性等于`value`。                    |
  | Reflect.defineProperty(target, name, desc)     | 用来为对象定义属性。                                         |
  | Reflect.deleteProperty(target, name)           | 等同于`delete obj[name]`，用于删除对象的属性。               |
  | Reflect.has(target, name)                      | `Reflect.has`方法对应`name in obj`里面的`in`运算符。==（判断有没有）== |
  | Reflect.ownKeys(target)                        | `用于返回对象的所有属性                                      |
  | Reflect.isExtensible(target)                   | 返回一个布尔值，表示当前对象是否可扩展。                     |
  | Reflect.preventExtensions(target)              | 用于让一个对象变为不可扩展。它返回一个布尔值，表示是否操作成功。 |
  | Reflect.getOwnPropertyDescriptor(target, name) | 用于得到指定属性的描述对象                                   |
  | Reflect.getPrototypeOf(target)                 | `Reflect.getPrototypeOf`方法用于读取对象的`__proto__`属性    |
  | Reflect.setPrototypeOf(target, prototype)      | 用于设置目标对象的原型（prototype）                          |

  

  