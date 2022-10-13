## 创建项目

### 创建项目box-app

```shell
create-react-app box-app
cd box-app
npm start
```

### 安装bootstrap库

```shell
npm i bootstrap
```

### bootstrap的引入方式


`import 'bootstrap/dist/css/bootstrap.css'`


---

## 创建按钮

当子节点数量大于1时，可以用`<div>`或`<React.Fragment>`将其括起来。

---

## 内嵌表达式

JSX中使用`{}`嵌入表达式。

---

## 设置属性

+   `class -> className`
+   CSS属性：`background-color -> backgroundColor`，其它属性类似

---

## 渲染列表

+   使用`map`函数
+   每个元素需要具有唯一的`key`属性，用来帮助`React`快速找到被修改的`DOM`元素。

## Conditional Rendering

---

利用逻辑表达式的短路原则。

+   与表达式中 `expr1 && expr2`，当`expr1`为假时返回`expr1`的值，否则返回`expr2`的值
+   或表达式中 `expr1 || expr2`，当`expr1`为真时返回`expr1`的值，否则返回`expr2`的值

---

## 绑定事件

+   注意妥善处理好绑定事件函数的`this`

---

## 修改state

+   需要使用`this.setState()`函数
+   每次调用`this.setState()`函数后，会重新调用`this.render()`函数，用来修改虚拟DOM树。React只会修改不同步的实际DOM树节点。




