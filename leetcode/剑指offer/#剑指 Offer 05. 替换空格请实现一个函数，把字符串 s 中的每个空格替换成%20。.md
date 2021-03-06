# #[剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)请实现一个函数，把字符串 `s` 中的每个空格替换成"%20"。

## 示例 1：

```
输入：s = "We are happy."
输出："We%20are%20happy."
```

## 限制：

```
0 <= s 的长度 <= 10000
```

## 解题思路：

### 解 1：StringBuilder

新建一个StringBuilder，遍历原字符串，遇到空格则往StringBuilder中添加 “%20”，否则添加原字符。

~~~java
class Solution {
    public String replaceSpace(String s) {
        StringBuilder b = new StringBuilder();
        for (int i=0; i < s.length(); i++) {
            if (s.charAt(i) == ' ') {
                b.append("%20");
            }
            else {
                b.append(s.charAt(i));
            }
        }

        return b.toString();
    }
}
~~~

- 时间复杂度：O(n)

- 空间复杂度：O(n)

### 解 2：

创建额外数组实现，新建一个长度为原字符串长度3倍的字符数组，然后遍历原字符串，依次把字符添加到字符数组中，遇到空格则添加 “%20”。

~~~java
class Solution {
    public String replaceSpace(String s) {
        int length = s.length();
        char[] array = new char[length * 3];
        int size = 0;
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                array[size++] = '%';
                array[size++] = '2';
                array[size++] = '0';
            } else {
                array[size++] = c;
            }
        }
        String newStr = new String(array, 0, size);
        return newStr;
    }
}
~~~

- 时间复杂度：O(n)

- 空间复杂度：O(n)

### 解 3：

由于Java中字符串是不可变的，所以无法原地修改，在C++中字符串是可变的，因此可以原地修改。

具体方式为：

- 初始化：空格数量 count ，字符串 s 的长度 len ；
- 统计空格数量：遍历 s ，遇空格则 count++ ；
- 修改 s 长度：添加完 "%20" 后的字符串长度应为 len + 2 * count ；
- **倒序**遍历修改：i 指向**原**字符串尾部元素， j 指向**新**字符串尾部元素；当 i = j 时跳出（代表左方已没有空格，无需继续遍历）；
- 当 s[i] 不为空格时：执行 s[j] = s[i] ；
- 当 s[i] 为空格时：将字符串闭区间 [j-2, j] 的元素修改为 "%20" ；由于修改了 3 个元素，因此需要 j -= 2 ；
- 返回值：已修改的字符串 s ；

