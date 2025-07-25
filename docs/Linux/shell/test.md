## 逻辑运算符&&和||

+   `&&` 表示与，`||` 表示或
+   二者具有短路原则：
    +   `expr1 && expr2`：当`expr1`为假时，直接忽略`expr2`
    +   `expr1 || expr2`：当`expr1`为真时，直接忽略`expr2`
+   表达式的`exit code`为`0`，表示真；为非零，表示假。（与`C/C++`中的定义相反）

---

### test命令

在命令行中输入`man test`，可以查看`test`命令的用法。

`test`命令用于判断文件类型，以及对变量做比较。

`test`命令用`exit code`返回结果，而不是使用`stdout`。`0`表示真，非`0`表示假。

**举例**

```shell
test 2 -lt 3  # 为真，返回值为0
echo $?  # 输出上个命令的返回值，输出0
```

```shell
acs@9e0ebfcd82d7:~$ ls  # 列出当前目录下的所有文件
homework  output.txt  test.sh  tmp
acs@9e0ebfcd82d7:~$ test -e test.sh && echo "exist" || echo "Not exist"
exist  # test.sh 文件存在
acs@9e0ebfcd82d7:~$ test -e test2.sh && echo "exist" || echo "Not exist"
Not exist  # testh2.sh 文件不存在
```

### 文件类型判断

**命令格式：**

``` shell
test -e filename  # 判断文件是否存在
```

|测试参数|代表意义|
|---|---|
|-e|文件是否存在|
|-f|是否为文件|
|-d|是否为目录|


### 文件权限判断

**命令格式：**

```shell
test -r filename  # 判断文件是否可读
```

|测试参数|代表意义|
|---|---|
|-r|文件是否可读|
|-w|文件是否可写|
|-x|文件是否可执行|
|-s|是否为非空文件|


### 整数间的比较

**命令格式：**

```shell
test $a -eq $b  # a是否等于b
```

|测试参数|代表意义|
|---|---|
|-eq|a是否等于b|
|-ne|a是否不等于b|
|-gt|a是否大于b|
|-lt|a是否小于b|
|-ge|a是否大于等于b|
|-le|a是否小于等于b|

### 字符串比较

|测试参数|	代表意义|
|---|---|
|test -z STRING	|判断STRING是否为空，如果为空，则返回true|
|test -n STRING	|判断STRING是否非空，如果非空，则返回true（-n可以省略）|
|test str1 == str2	|判断str1是否等于str2|
|test str1 != str2	|判断str1是否不等于str2|

### 多重条件判定

**命令格式：**

```shell
test -r filename -a -x filename
```

|测试参数|	代表意义|
|---|---|
|-a|两条件是否同时成立|
|-o|两条件是否至少一个成立|
|!|取反。如 test ! -x file，当file不可执行时，返回true|

---

## 判断符号[]

`[]`与`test`用法几乎一模一样，更常用于`if`语句中。另外`[[]]`是`[]`的加强版，支持的特性更多。

**例如：**

```shell
[ 2 -lt 3 ]  # 为真，返回值为0
echo $?  # 输出上个命令的返回值，输出0
```

```shell
acs@9e0ebfcd82d7:~$ ls  # 列出当前目录下的所有文件
homework  output.txt  test.sh  tmp
acs@9e0ebfcd82d7:~$ [ -e test.sh ] && echo "exist" || echo "Not exist"
exist  # test.sh 文件存在
acs@9e0ebfcd82d7:~$ [ -e test2.sh ] && echo "exist" || echo "Not exist"
Not exist  # testh2.sh 文件不存在
```

### 注意点

+   `[]`内的每一项都要用空格隔开
+   中括号内的变量，最好用双引号括起来
+   中括号内的常数，最好用单或双引号括起来

### 举例

```shell
name="acwing fyq"
[ $name == "acwing fyq" ]  # 错误，等价于 [ acwing fyq == "acwing fyq" ]，参数太多
[ "$name" == "acwing fyq" ]  # 正确
```

