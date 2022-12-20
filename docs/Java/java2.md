### 基本if-else语句

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int a = sc.nextInt();

        if (a > 5) {
            System.out.printf("%d is big!\n", a);
            System.out.printf("%d + 1 = %d\n", a, a + 1);
        } else {
            System.out.printf("%d is small!\n", a);
            System.out.printf("%d - 1 = %d\n", a, a - 1);
        }
    }
}
```

---

### 条件表达式

+ 与`&&`
+ 或`||`
+ 非`!`

---

### switch语句

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int day = sc.nextInt();
        String name;

        switch(day) {
            case 1:
                name = "Monday";
                break;
            case 2:
                name = "Tuesday";
                break;
            case 3:
                name = "Wednesday";
                break;
            case 4:
                name = "Thursday";
                break;
            case 5:
                name = "Friday";
                break;
            case 6:
                name = "Saturday";
                break;
            case 7:
                name = "Sunday";
                break;
            default:
                name = "not valid";
        }

        System.out.println(name);
    }
}
```

---
