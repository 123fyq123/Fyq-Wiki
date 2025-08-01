`expr`命令用于求表达式的值，格式为：

```shell
expr 表达式
```

---

## 说明

+   用空格隔开每一项
+   用反斜杠放在shell特定的字符前面（发现表达式运行错误时，可以试试转义）
+   对包含空格和其他特殊字符的字符串要用引号括起来
+   `expr`会在`stdout`中输出结果。如果为逻辑关系表达式，则结果为真，`stdout`为`1`，否则为`0`。
+   `expr`的`exit code`：如果为逻辑关系表达式，则结果为真，`exit code`为`0`，否则为`1`。

---

## 字符串表达式

+   `length STRING`
    +   返回`STRING`的长度
+   `index STRING CHARSET`
    +   `CHARSET`中任意单个字符在`STRING`中最前面的字符位置，下标从`1`开始。如果在`STRING`中完全不存在
    +   `CHARSET`中的字符，则返回`0`。
+   `substr STRING POSITION LENGTH`
    +   返回`STRING`字符串中从`POSITION`开始，长度最大为`LENGTH`的子串。如果`POSITION`或`LENGTH`为负数，`0`或非数值，则返回空字符串。

### 举例

```shell
str="Hello World!"

echo `expr length "$str"`  # ``不是单引号，表示执行该命令，输出12
echo `expr index "$str" aWd`  # 输出7，下标从1开始
echo `expr substr "$str" 2 3`  # 输出 ell
```

---

## 整数表达式

`expr`支持普通的算术操作，算术表达式优先级低于字符串表达式，高于逻辑关系表达式。

+   `+ -`
    +   加减运算。两端参数会转换为整数，如果转换失败则报错。
+   `* / %`
    +   乘，除，取模运算。两端参数会转换为整数，如果转换失败则报错。
+   `()`
    +   可以改变优先级，但需要用反斜杠转义

### 举例

```shell
a=3
b=4

echo `expr $a + $b`  # 输出7
echo `expr $a - $b`  # 输出-1
echo `expr $a \* $b`  # 输出12，*需要转义
echo `expr $a / $b`  # 输出0，整除
echo `expr $a % $b` # 输出3
echo `expr \( $a + 1 \) \* \( $b + 1 \)`  # 输出20，值为(a + 1) * (b + 1)
```

---

## 逻辑关系表达式

+   `|`
    +   如果第一个参数非空且非0，则返回第一个参数的值，否则返回第二个参数的值，但要求第二个参数的值也是非空或非0，否则返回0。如果第一个参数是非空或非0时，不会计算第二个参数。
+   `&`
    +   如果两个参数都非空且非0，则返回第一个参数，否则返回0。如果第一个参为0或为空，则不会计算第二个参数。
+   `< <= = == != >= >`
    +   比较两端的参数，如果为true，则返回1，否则返回0。”==”是”=”的同义词。”expr”首先尝试将两端参数转换为整数，并做算术比较，如果转换失败，则按字符集排序规则做字符比较。
+   `() `
    +   可以改变优先级，但需要用反斜杠转义

### 举例

```shell
a=3
b=4

echo `expr $a \> $b`  # 输出0，>需要转义
echo `expr $a '<' $b`  # 输出1，也可以将特殊字符用引号引起来
echo `expr $a '>=' $b`  # 输出0
echo `expr $a \<\= $b`  # 输出1

c=0
d=5

echo `expr $c \& $d`  # 输出0
echo `expr $a \& $b`  # 输出3
echo `expr $c \| $d`  # 输出5
echo `expr $a \| $b`  # 输出3
```