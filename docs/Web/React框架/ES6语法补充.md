## 使用bind()函数绑定this取值

在JavaScript中，函数里的`this`指向的是执行时的调用者，而非定义时所在的对象。

**例如：**

```jsx
const person = {
  name: "fyq",
  talk: function() {
    console.log(this);
  }
}

person.talk();

const talk = person.talk;
talk();
```

**运行结果：**

```jsx
{name: 'fyq', talk: ƒ}
Window
```

`bind()`函数，可以绑定`this`的取值。**例如：**

```jsx
const talk = person.talk.bind(person);
```

--- 

## 箭头函数的简写方式

```jsx
const f = (x) => {
  return x * x;
};
```

可以简写为：

```jsx
const f = x => x * x;
```

---

## 箭头函数不重新绑定this的取值

**例如：**

```jsx
const person = {
  talk: function() {
    setTimeout(function() {
      console.log(this);
    }, 1000);
  }
};

person.talk();  // 输出Window
```

```jsx
const person = {
  talk: function() {
    setTimeout(() => {
      console.log(this);
    }, 1000);
  }
};

person.talk();  // 输出 {talk: f}
```

---

## 对象的解构

**例如：**

```jsx
const person = {
  name: "fyq",
  age: 18,
  height: 180,
};

const {name : nm, age} = person;  // nm是name的别名
```

---

## 数组和对象的展开

### 数组展开

```jsx
let a = [1, 2, 3];
let b = [...a];  // b是a的复制
let c = [...a, 4, 5, 6];
```

### 对象展开
```jsx
const a = {name: "fyq"};
const b = {age: 18};
const c = {...a, ...b, height: 180};
```

---

## Named 与 Default exports

+   `Named Export`：可以`export`多个，`import`的时候需要加大括号，名称需要匹配
+   `Default Export`：最多`export`一个，`import`的时候不需要加大括号，可以直接定义别名