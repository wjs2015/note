# #[LCP 06. 拿硬币](https://leetcode-cn.com/problems/na-ying-bi/)

桌上有 n 堆力扣币，每堆的数量保存在数组 coins 中。我们每次可以选择任意一堆，拿走其中的一枚或者两枚，求拿完所有力扣币的最少次数。

## 示例 1：

输入：[4,2,1]

输出：4

解释：第一堆力扣币最少需要拿 2 次，第二堆最少需要拿 1 次，第三堆最少需要拿 1 次，总共 4 次即可拿完。

## 示例 2：

输入：[2,3,10]

输出：8

## 限制：

1 <= n <= 4
1 <= coins[i] <= 10

## 解题思路：

### 解 1：模2以判断是奇数还是偶数，若是偶数，直接除以2，若是奇数，除以2加1：

```java
class Solution {
  public int minCount(int[] coins) {
​    int count = 0;
​    for (int i = 0; i < coins.length; i++) {
​      if (coins[i] % 2 == 0) count += coins[i]/2;
​      else count += coins[i]/2 + 1;
​    }
​    return count;
  }
}
```

### 解 2：递归解法：

```java
class Solution {
  int count=0;
  public int minCount(int[] coins) {
​    int len = coins.length;
​    for(int i=0;i<len;i++){
​      helper(coins[i]);
​    }
​    return count;
  }

  private void helper(int coin) {
​    if(coin==0){
​      return;
​    }
​    if(coin>=2){
​      count++;
​      helper(coin-2);
​    }else if(coin==1){
​      count++;
​      helper(coin-1);
​    }
  }
}
```

此方法占用内存比解法1要少，为什么呢？

