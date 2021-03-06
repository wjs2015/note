# #[剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。 

## 示例 1：

```
输入: "the sky is blue"
输出: "blue is sky the"
```

## 示例 2：

```
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
```

## 示例 3：

```
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```

## 说明：

- 无空格字符构成一个单词。
- 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
- 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

## 解题思路：

### 解 ：双指针（快慢指针）

- 先去除首尾空格。

- 然后两个指针都指向字符串最后一个元素。

- 接着快指针向前移动寻找空格，找到空格后，即可取到字符串中最后一个单词。

- 然后快指针继续向前移动，寻找到非空格字符后，慢指针指向该位置。

- 重复上述步骤。

~~~java
class Solution {
    public String reverseWords(String s) {
        s = s.trim();
        int i = s.length() - 1, j = i;
        StringBuilder strb = new StringBuilder();

        while(i >= 0) {
            while(i >=0 && s.charAt(i) != ' ') i--;
            strb.append(s.substring(i+1, j+1));
            strb.append(' ');

            while(i >=0 && s.charAt(i) == ' ') i--;

            j = i;
        }
        return strb.toString().trim();

    }
}
~~~

- 时间复杂度 O(N) ： 其中 NN 为字符串ss 的长度，线性遍历字符串。
- 空间复杂度 O(N) ： 新建的  StringBuilder 中的字符串总长度 ≤N ，占用 O(N) 大小的额外空间。

