## arguments的使用

### 1.本质

arguments 实际上 它是当前函数的一个内置对象。所有函数都内置了一个 arguments 对象，arguments 对象中存储了传递的 所有实参。

### 2.特点

<font color='red'>**arguments展示形式是一个伪数组**</font>，可以进行遍历

+ 具有 length 属性
+ 按索引方式储存数据
+ 不具有数组的push，pop

### 3. 代码实现

案例：冒泡排序

```javascript
  function sort() {
           for(var i=0;i<arguments.length-1;i++) {
               for(var j=0;j<arguments.length-i-1;j++) {
                    if(arguments[j]>arguments[j+1]) {
                        var c=arguments[j];
                        arguments[j]=arguments[j+1];
                        arguments[j+1]=c;
                    }
               }
           }
           return arguments;
       }

       console.log(sort(5,1,11,8,9,4,6,7));

```

