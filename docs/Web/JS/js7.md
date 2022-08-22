## 概述

数组是一种特殊的对象。
类似于C++中的数组，但是数组中的元素类型可以不同。

+   数组中的元素可以是变量、数组、对象、函数。

例如：

```js
let a = [1, 2, "a", "fyq"];

let b = [
    1,  // 变量
    "fyq",  // 变量
    ['a', 'b', 3],  // 数组
    function () {  // 函数
        console.log("Hello World");
    },
    { name: "fyq", age: 18 }  // 对象
];
```

---

## 访问数组中的元素

通过下标。

例如：

```js
a[0] = 1; // 访问数组a[]的第0个元素
console.log(a[0]);
```

---

## 数组的常用属性和函数

+   属性`length`：返回数组长度。注意`length`是属性，不是函数，因此调用的时候不要加()
+   函数`push()`：向数组末尾添加元素
+   函数`pop()`：删除数组末尾的元素
+   函数`splice(a, b)`：删除从a开始的b个元素
+   函数`sort()`：将整个数组从小到大排序
+   自定义比较函数：`array.sort(cmp)`，函数`cmp`输入两个需要比较的元素，返回一个实数，负数表示第一个参数小于第二个参数，0表示相等，正数表示大于。