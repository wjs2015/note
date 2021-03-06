# #[剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

写一个函数，输入 `n` ，求斐波那契（Fibonacci）数列的第 `n` 项（即 `F(N)`）。斐波那契数列的定义如下：

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
```

斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

## 示例 1：

```
输入：n = 2
输出：1
```

## 示例 2：

```
输入：n = 5
输出：5
```

## 提示：

- `0 <= n <= 100`

## 解题思路：

### 解 1：递归

简单递归会导致大量重复计算，例如求f(n-1)和f(n-2)时都会重复计算f(n-3)。。。，因此简单的递归会超时，对此进行优化，使用一个HashMap存储已经计算过的值，有则直接使用，无则计算。

~~~java
class Solution {
    int constant = 1000000007;

    public int fib(int n) {
        return fib(n, new HashMap());
    }

    public int fib(int n, Map<Integer, Integer> map) {
        if (n < 2)
            return n;
        if (map.containsKey(n))
            return map.get(n);
        int first = fib(n - 1, map) % constant;
        map.put(n - 1, first);
        int second = fib(n - 2, map) % constant;
        map.put(n - 2, second);
        int res = (first + second) % constant;
        map.put(n, res);
        return res;
    }
}
~~~

- 时间复杂度：O(n^2?)
- 空间复杂度：O(n^2?)

### 解 2：迭代（动态规划）

动态规划解析：

- 状态定义： 设 dpdp 为一维数组，其中 dp[i] 的值代表 斐波那契数列第 i 个数字 。
- 转移方程： dp[i+1]=dp[i]+dp[i−1] ，即对应数列定义 f(n+1)=f(n)+f(n−1) ；
- 初始状态： dp[0]=0, dp[1]=1 ，即初始化前两个数字；
- 返回值： dp[n] ，即斐波那契数列的第 n 个数字。



空间复杂度优化：

- 若新建长度为 n 的 dp 列表，则空间复杂度为 O(N) 。
- 由于 dp 列表第 i 项只与第 i−1 和第 i−2 项有关，因此只需要初始化三个整形变量 sum, a, b ，利用辅助变量 sum 使 a, b 两数字交替前进即可 （具体实现见代码） 。节省了 dp 列表空间，因此空间复杂度降至 O(1) 。

循环求余法：

- 大数越界： 随着 n 增大, f(n) 会超过 Int32 甚至 Int64 的取值范围，导致最终的返回值错误。

- 求余运算规则： 设正整数 x, y, p，求余符号为⊙ ，则有 (x+y)⊙p=(x⊙p+y⊙p)⊙p 。
  解析： 根据以上规则，可推出 f(n)⊙p=[f(n−1)⊙p+f(n−2)⊙p]⊙p ，从而可以在循环过程中每次计算sum=(a+b)⊙1000000007 ，此操作与最终返回前取余等价。

~~~java
class Solution {
    public int fib(int n) {
        int f0 = 0;
        int f1 = 1;
        int sum;
        if (n == 0) return 0;
        if (n == 1) return 1;

        for(int i = 2; i <= n; i++) {
            sum = (f0 +f1)%((int)1e9+7);
            f0 = f1;
            f1 = sum;
        }
        return f1;
    }
}
~~~

- 时间复杂度 O(N) ： 计算 f(n) 需循环 n 次，每轮循环内计算操作使用 O(1) 。
- 空间复杂度 O(1) ： 几个标志变量使用常数大小的额外空间。

