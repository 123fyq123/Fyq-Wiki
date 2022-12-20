### 变量定义

```java
int a = 5;
```

---

### 常量定义

```java
final int N = 50;
```

---

### 运算符 

类似C++

+ `+,-,*,/,%,++,--,+=,-=,*=,/=,%=`

---

### 输入

#### 方式一(效率低，规模小使用)

```java
import java.util.Scanner;

Scanner scan = new Scanner(System.in);
String str = scan.next(); // 读入下一个为字符串
int x = scan.nextInt(); // 读入下一个为整数
float y = scan.nextFloat(); // 读入下一个单精度浮点数
double z = scan.nextDouble(); // 读入下一个双精度浮点数
String line = scan.nextLine(); // 读入下一行
```

#### 方式二(效率高，需抛出异常)

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = br.readLine();
        System.out.println(str);
    }
}
```

---

### 输出

#### 方式一(效率低，规模小使用)

```java
System.out.println(123);  // 输出整数 + 换行
System.out.println("Hello World");  // 输出字符串 + 换行
System.out.print(123);  // 输出整数
System.out.printf("%04d %.2f\n", 4, 123.456D);  // 格式化输出，float与double都用%f输出
```

#### 方式二(效率高，需抛出异常)

```java
import java.io.BufferedWriter;
import java.io.OutputStreamWriter;

public class Main {
    public static void main(String[] args) throws Exception {
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        bw.write("Hello World\n");
        bw.flush();  // 需要手动刷新缓冲区
    }
}
```

---