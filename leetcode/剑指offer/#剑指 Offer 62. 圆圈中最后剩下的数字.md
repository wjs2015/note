# #[剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

0,1,···,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

## 示例 1：

```
输入: n = 5, m = 3
输出: 3
```

## 示例 2：

```
输入: n = 10, m = 17
输出: 2
```

## 限制：

- `1 <= n <= 10^5`
- `1 <= m <= 10^6`

## 解题思路：

### 解 1：递归

自顶向下：定义f(int n, int m)的返回值为最后一次删除元素的**下标**，则 f(n, m) = (m+f(n-1, m))%n，层层递归到最后一层，即n==1时，返回值为序列长度为1时应删除的元素下标（即0），往上返回时，每一层均可根据上一层的返回值及上式求得当前层序列长度应返回的下标值，返回至最顶层即为最后结果。

~~~java
class Solution {
    public int lastRemaining(int n, int m) {
        return f(n, m);
    }
    private int f(int n, int m) {
        if (n == 1) {
            return 0;
        }
        int x = f(n-1, m);
        return (m+x) % n;
    }
}
~~~

- 时间复杂度：O(n)，求解n次
- 空间复杂度：O(n)，递归调用n次

### 解 2：迭代

直接按照上述思路以自底向上的顺序迭代即可。

~~~java
class Solution {
    public int lastRemaining(int n, int m) {
        int f = 0;
        for(int i = 2; i <= n; i++) {
            f = (f + m) % i;
        }
        return f;
    }
}
~~~

- 时间复杂度：O(n)
- 空间复杂度：O(1)，使用常数级别的额外空间（变量）

