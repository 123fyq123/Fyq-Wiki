### 源文件文件规则

+ 一个源文件中只能有一个`public`类。
+ 一个源文件可以有多个非`public`类。
+ 源文件的名称应该和`public`类的类名保持一致。
+ 每个源文件中，先写`package`语句，再写`import`语句，最后定义类。

---

### 类的定义

+ `public`：可以被所有对象访问
+ `private`：只有本类内部可以访问
+ `protected`：同一个包或者子类可以访问
+ 不添加修饰符：同一个包可以访问
+ 静态(static)成员变量与普通成员变量区别
  + 所有`static`成员变量在类中只有一份，被所有对象共享
  + 普通成员变量在所有对象中都有独立的一份
  + 静态函数只能调用静态变量，普通函数可以调用普通变量和静态变量

```java
class Point {
    private int x;
    private int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public void setX(int x) {
        this.x = x;
    }

    public void setY(int y) {
        this.y = y;
    }

    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }

    public String toString() {
        return String.format("(%d, %d)", x, y);
    }
}
```

---

### 类的继承

**每个类只能继承一个类**

```java
class ColorPoint extends Point {
    private String color;

    public ColorPoint(int x, int y, String color) {
        super(x, y);
        this.color = color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public String toString() {
        return String.format("(%d, %d, %s)", super.getX(), super.getY(), this.color);
    }
}
```

---

### 类的多态

```java
public class Main {
    public static void main(String[] args) {
        Point point = new Point(3, 4);
        Point colorPoint = new ColorPoint(1, 2, "red");

        // 多态，同一个类的实例，调用相同的函数，运行结果不同
        System.out.println(point.toString());
        System.out.println(colorPoint.toString());
    }
}
```

---

### 接口

#### 接口的定义

**接口中不添加修饰符时，默认为public。**

```java
interface Role {
    public void greet();
    public void move();
    public int getSpeed();
}
```

#### 接口的继承

**每个接口可以继承多个接口**

```java
interface Hero extends Role {
    public void attack();
}
```

#### 接口的实现

```java
class Zeus implements Hero {
    private final String name = "Zeus";
    public void attack() {
        System.out.println(name + ": Attack!");
    }

    public void greet() {
        System.out.println(name + ": Hi!");
    }

    public void move() {
        System.out.println(name + ": Move!");
    }

    public int getSpeed() {
        return 10;
    }
}
```

#### 接口的多态

```java
class Athena implements Hero {
    private final String name = "Athena";
    public void attack() {
        System.out.println(name + ": Attack!!!");
    }

    public void greet() {
        System.out.println(name + ": Hi!!!");
    }

    public void move() {
        System.out.println(name + ": Move!!!");
    }

    public int getSpeed() {
        return 10;
    }
}

public class Main {
    public static void main(String[] args) {
        Hero[] heros = {new Zeus(), new Athena()};
        for (Hero hero: heros) {
            hero.greet();
        }
    }
}
```

---
