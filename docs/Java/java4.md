### 数组定义

```java
public class Main {
    public static void main(String[] args) {
        int[] a = new int[10], b;
        float[] f = new float[33];
        double[] d = new double[123];
        char[] c = new char[21];
    }
}
```

---

### 数组初始化

```java
public class Main {
    public static void main(String[] args) {
        int[] a = {0, 1, 2};        // 含有3个元素的数组，元素分别是0, 1, 2
        int[] b = new int[3];       // 含有3个元素的数组，元素的值均为0
        char[] d = {'a', 'b', 'c'}; // 字符数组的初始化
    }
}
```

---

### 常用API

+ `length`：返回数组长度
+ `Arrays.sort()`：数组排序
+ `Arrays.fill(int a[], int val)`：数组填充
+ `Arrays.toString()`：将数组转化为字符串
+ 使用`Arrays`需要`import java.util.Arrays`

---

