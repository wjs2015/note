# #[剑指 Offer 50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

## 示例:

```
s = "abaccdeff"
返回 "b"

s = "" 
返回 " "
```

## 限制：

```
0 <= s 的长度 <= 50000
```

## 解题思路：

### 解 1：使用HashMap统计

由于HashMap是无序的，因此第二次遍历的时候需遍历原字符串。

~~~java
class Solution {
    public char firstUniqChar(String s) {
        Map<Character, Integer> map = new HashMap<>();
        char[] ca = s.toCharArray();
        for(int i = 0; i < ca.length; i++) {
            map.put(ca[i], map.getOrDefault(ca[i], 0)+1);
        }
        for(int i = 0; i < ca.length; i++) {
            if(map.get(ca[i])==1) return ca[i];
        }
        return ' ';
    }
}
~~~

- 时间复杂度：O(n)
- 空间复杂度：O(n)

### 解 2：使用LinkedHashMap统计

由于LinkedHashMap是有序的，因此第二次遍历的时候只需遍历map即可。

~~~java
class Solution {
    public char firstUniqChar(String s) {
        Map<Character, Integer> map = new LinkedHashMap<>();
        char[] ca = s.toCharArray();
        for(int i = 0; i < ca.length; i++) {
            map.put(ca[i], map.getOrDefault(ca[i], 0)+1);
        }
        for(char i : map.keySet()) {
            if(map.get(i)==1) return i;
        }
        return ' ';
    }
}
~~~

- 时间复杂度：O(n)
- 空间复杂度：O(n)

### 解 3：使用数组统计

要求只出现一次的第一个字符，就需要去计数每一个字符出现了几次。一般都是用哈希表，但是字符往多了数按照扩展的ASCII码也就256个，就可以使用int[] count来代替哈希表：索引就是字符，值就是该字符出现的次数。

~~~java
class Solution {
    public char firstUniqChar(String s) {
        int[] count = new int[256];
        for(int i = 0; i < 256; i++) {
            count[i] = 0;
        }
        char[] ca = s.toCharArray();
        for(int i = 0; i < ca.length; i++) {
            count[ca[i]]++;
        }

        for(int i = 0; i < ca.length; i++) {
            if(count[ca[i]] == 1) return ca[i];
        }
        return ' ';
    }
}
~~~

- 时间复杂度：O(n)
- 空间复杂度：O(n)