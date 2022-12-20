### while循环

```java
public class Main {
    public static void main(String[] args) {
        int i = 0;
        while (i < 10) {
            System.out.println(i);
            i ++ ;
        }
    }
}
```

---

### Do While循环

**`do while`语句限制性循环体后检查条件。不管条件的值如何，我们都要至少执行一次循环。**

```java
public class Main {
    public static void main(String[] args) {
        int x = 1;
        while (x < 1) {
            System.out.println("x!");
        }

        int y = 1;
        do {
            System.out.println("y!");
        } while (y < 1);
    }
}
```

---

### for循环

```java
public class Main {
    public static void main(String[] args) {
        for (int i = 0; i < 10; i ++ ) {  // 循环体中只有一条语句时，可以不加大括号
            System.out.println(i);
        }
    }
}
```

---
