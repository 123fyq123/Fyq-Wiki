### 编写函数

**一个典型的函数定义包括以下部分：修饰符、返回类型、函数名字、由0个或多个形参组成的列表以及函数体。**

**以下是求阶乘的函数:**

````java
public class Main {
    private static int fact(int val) {
        int res = 1;
        for (int i = 1; i <= val; i ++ )
            res *= i;
        return res;
    }
}
````

+	函数名字是`fact`，它作用于一个整型参数，返回一个整型值。`return`语句负责结束`fact`并返回`res`的值。

+ 修饰符包括`private`、`static`等，它们属于类相关的概念，会在下一章解释。

---

### 调用函数

```java
public class Main {
    private static int fact(int val) {
        int res = 1;
        for (int i = 1; i <= val; i ++ )
            res *= i;
        return res;
    }

    public static void main(String[] args) {
        int res = fact(5);
        System.out.printf("5! is %d\n", res);
    }
}
```

---

### 参数传递

#### 值传递

**将实参的初始值拷贝给形参。此时，对形参的改动不会影响实参的初始值。**

```java
public class Main {
    private static void f(int x) {
        x = 5;
    }

    public static void main(String[] args) {
        int x = 10;
        f(x);
        System.out.println(x);
    }
}
```

#### 引用传递

**除`String`以外的数据类型的对象，例如数组、`StringBuilder`等采用引用传递。**

**将实参的引用（地址）传给形参，通过引用找到变量的真正地址，然后对地址中的值修改。所以此时对形参的修改会影响实参的初始值。**

```java
import java.util.Arrays;

public class Main {
    private static void f1(int[] a) {
        for (int i = 0, j = a.length - 1; i < j; i ++, j -- ) {
            int t = a[i];
            a[i] = a[j];
            a[j] = t;
        }
    }

    private static void f2(StringBuilder sb) {
        sb.append("Hello World");
    }

    public static void main(String[] args) {
        int[] a = {1, 2, 3, 4, 5};
        f1(a);
        System.out.println(Arrays.toString(a));

        StringBuilder sb = new StringBuilder("");
        f2(sb);
        System.out.println(sb);
    }
}
```

---

### 函数重载

**函数重载是指：在同一个类中存在多个函数，函数名称相同但参数列表不同。**

**编译器会根据实参的类型选择最匹配的函数来执行。**

```java
import java.util.Scanner;

public class Main {
    private static int max(int a, int b) {
        System.out.println("int max");
        if (a > b) return a;
        return b;
    }

    private static double max(double a, double b) {
        System.out.println("double max");
        if (a > b) return a;
        return b;
    }

    public static void main(String[] args) {
        System.out.println(max(3, 4));
        System.out.println(max(3.0, 4.0));
    }
}
```

---

### 函数递归

**在一个函数内部，也可以调用函数本身。**

```java
import java.util.Scanner;

public class Main {
    private static int fib(int n) {  // 求斐波那切数列第n项
        if (n <= 2) return 1;
        return fib(n - 1) + fib(n - 2);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        System.out.println(fib(n));
    }
}
```

---
