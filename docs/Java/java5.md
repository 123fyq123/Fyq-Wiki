### String初始化

```java
String a = "Hello World";
String b = "My name is ";
String x = b;  // 存储到了相同地址
String c = b + "yxc";  // String可以通过加号拼接
String d = "My age is " + 18;  // int会被隐式转化成字符串"18"
String str = String.format("My age is %d", 18);  // 格式化字符串，类似于C++中的sprintf
String money_str = "123.45";
double money = Double.parseDouble(money_str);  // String转double
```

**只读变量，不能修改，例如:**

```java
String a = "Hello ";
a += "World";  // 会构造一个新的字符串
```

---

### 常用API

+ `length()`：返回字符串长度
+ `split(String regex)`：分割字符串
+ `equals()`：判断两字符串是否相等，注意不能使用`==`
+ `substring(int beginIndex, int endIndex)`：返回`[beinInext, endIndex)`中的子串
+ `trim()`：去除首尾的空白字符

---

### 输入与输出

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str1 = sc.next();  // 输入字符串，遇到空格、回车等空白字符时停止输入
        String str2 = sc.nextLine();  // 输入一整行字符串，遇到空格不会停止输入，遇到回车才会停止

        System.out.println(str1);  // 可以直接输出
        System.out.printf("%s\n", str2);  // 也可以格式化输出，用 %s 表示字符串
    }
}
```

---

### StringBuilder和StringBuffer

**由于`String`不能被修改，如果打算修改，可以使用`StringBuider`或`StringBuffer`**

`StringBuffer`线程安全，速度较慢

`StringBuilder`线程不安全，速度较快

```java
StringBuilder sb = new StringBuilder("Hello ");  // 初始化
sb.append("World");  // 拼接字符串
System.out.println(sb);

for (int i = 0; i < sb.length(); i ++ ) {
    sb.setCharAt(i, (char)(sb.charAt(i) + 1));  // 读取和写入字符
}

System.out.println(sb);
```

---

