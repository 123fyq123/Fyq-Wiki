`echo`用于输出字符串。命令格式：

```shell
echo STRING
```

---

## 显示普通字符串

```shell
echo "Hello Terminal"
echo Hello Terminal  # 引号可以省略
```

---

## 显示转义字符

```shell
echo "\"Hello Terminal\""  # 注意只能使用双引号，如果使用单引号，则不转义
echo \"Hello Terminal\"  # 也可以省略双引号
```

---

## 显示变量

``` shell
name=fyq
echo "My name is $name"  # 输出 My name is fyq
```

---

## 显示换行

``` shell
echo -e "Hi\n"  # -e 开启转义
echo "我为acwing举大旗！！！"
```

**输出结果：**

```shell
Hi

我为acwing举大旗！！！
```

---

## 显示不换行

```shell
echo -e "Hi \c" # -e 开启转义 \c 不换行
echo "我为acwing举大旗"
```

**输出结果：**

```shell
Hi 我为acwing举大旗
```

---

## 显示结果定向至文件

```shell
echo "Hello World" > output.txt  # 将内容以覆盖的方式输出到output.txt中
```

---

## 原样输出字符串，不进行转义或取变量(用单引号)

```shell
name=acwing
echo '$name\"'
```

**输出结果：**

```shell
$name\"
```

---

## 显示命令的执行结果


```shell
echo `date`
```

**输出结果：**

```shell
Wed Sep 1 11:45:33 CST 2021
```




