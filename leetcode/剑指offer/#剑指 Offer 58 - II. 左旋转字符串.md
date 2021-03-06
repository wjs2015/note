# #剑指 Offer 58 - II. 左旋转字符串

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

## 示例 1：

输入: s = "abcdefg", k = 2
输出: "cdefgab"

## 示例 2：

输入: s = "lrloseumgh", k = 6
输出: "umghlrlose"

## 注：

- 1 <= k < s.length <= 10000

## 解题思路：

### 解 1：使用substring()切片之后直接拼接：

```java
class Solution {
  public String reverseLeftWords(String s, int n) {
​    return s.substring(n, s.length()) + s.substring(0, n);
  }
}
```

时间复杂度：O(N)；空间复杂度：O(N)

### 解 2：如果不能使用substring()，则可使用StringBuilder：

```java
class Solution {
  public String reverseLeftWords(String s, int n) {
​    StringBuilder res = new StringBuilder();
​    for (int i = n; i < s.length(); i++) {
​      res.append(s.charAt(i));
​    }
​    for (int i = 0; i < n; i++) {
​      res.append(s.charAt(i));
​    }
​    return res.toString();
  }
}
```

可以用取模的方式简化代码：

```java
class Solution {
  public String reverseLeftWords(String s, int n) {
​    StringBuilder res = new StringBuilder();
​    for (int i = n; i < n + s.length(); i++) {
​      res.append(s.charAt(i % s.length()));
​    }
​    return res.toString();
  }
}
```

注：使用取模的方式占用内存降低，但运行时间变长（都是遍历数组，取模的方式增加了计算量）

### 解 3：如果不能使用StringBuilder，则只能使用String，不断创建新的字符串（字符串不可变），这种方法效率低下：

```java
class Solution {
  public String reverseLeftWords(String s, int n) {
​    String res = "";
​    for (int i = n; i < s.length(); i++) {
​      res += s.charAt(i);
​    }
​    for (int i = 0; i < n; i++) {
​      res += s.charAt(i);
​    }
​    return res.toString();
  }
}
```

同样可以取模简化代码：

```java
class Solution {
  public String reverseLeftWords(String s, int n) {
​    String res = "";
​    for (int i = n; i < n + s.length(); i++) {
​      res += s.charAt(i % s.length());
​    }
​    return res.toString();
  }
}
```

上述所有解法的时间复杂度和空间复杂度均为O(n)