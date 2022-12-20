### List

+ 接口：`java.util.List<>`
+ 实现：
    + `java.util.ArrayList<>`：变长数组
    + `java.util.LinkedList<>`：双链表
+ 函数:
    + `add()`：在末尾添加元素
    + `get(i)`：获取第i个元素
    + `clear()`：清空
    + `set(i, val)`：将第i个元素设置为val
    + `isEmpty()`：是否为空
    + `size()`：返回长度
+ 定义：

```java
ArrayList<String> sites = new ArrayList<String>();
sites.add("Google");
sites.add("Runoob");
System.out.println(sites);

LinkedList<String> sites = new LinkedList<String>();
sites.add("Google");
sites.add("Runoob");
System.out.println(sites);
```

---

### 栈

+ 类：`java.util.Stack<>`
+ 函数：
    + `push()`：入栈
    + `pop()`：出栈
    + `empty()`：是否为空
    + `clear()`：清空
    + `size()`：长度
    + `peek()`：返回栈顶元素

---

### 队列

+ 接口：`java.util.Queue<>`
+ 实现：
    + `java.util.LinkedList<>`：双链表
    + `java.util.PriorityQueue<>`：优先队列
        + 默认是小根堆，大根堆写法：`new PriorityQueue<>(Collections.reverseOrder())`
+ 函数：
    + `add()`：在队尾添加元素
    + `remove()`：删除并返回队头
    + `isEmpty()`：是否为空
    + `size()`：返回长度
    + `clear()`：清空
    + `peek()`：返回队头

---

### Set

+ 接口：`java.util.Set<K>`
+ 实现：
    + `java.util.HashSet<K>`：哈希表
    + `java.util.TreeSet<K>`：平衡树
+ 函数
    + `add()`：添加元素
    + `contains()`：是否包含某个元素
    + `remove()`：删除元素
    + `size()`：返回元素数
    + `isEmpty()`：是否为空
    + `clear()`：清空

---

### Map

+ 接口：`java.util.Map<K, V>`
+ 实现：
    + `java.util.HashMap<K, V>`：哈希表
    + `java.util.TreeMap<K, V>`：平衡树
+ 函数：
    + `put(key, value)`：添加关键字和其对应的值
    + `get(key)`：返回关键字对应的值
    + `containsKey(key)`：是否包含关键字
    + `remove(key)`：删除关键字
    + `size()`：返回元素数
    + `isEmpty()`：是否为空
    + `clear()`：清空

---

