# #[剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

输入一个正整数 `target` ，输出所有和为 `target` 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

## 示例 1：

```
输入：target = 9
输出：[[2,3,4],[4,5]]
```

## 示例 2：

```
输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
```

## 限制：

- `1 <= target <= 10^5`

## 解题思路：

### 解 1：数学优化

如果我们知道起点 x 和终点 y ，那么 x 累加到 y 的和由求和公式可以知道是 (x+y)∗(y−x+1)/2 ，那么问题就转化为了是否存在一个正整数 y (y>x) ，满足等式 (x+y)∗(y−x+1)=target

转化一下变成 y^2+y-x^2+x-2*target=0

这是一个关于 y 的一元二次方程，其中 a=1,b=1,c=-x^2+x-2*target， 直接套用求根公式即可 O(1) 解得 y ，判断是否整数解需要满足两个条件：

- 判别式 b^2-4ac 开根需要为整数
- 最后的求根公式的分子需要为偶数，因为分母为 2

~~~java
class Solution {
    public int[][] findContinuousSequence(int target) {
        int start = 0;
        int limit = (target-1)/2;
        List<int[]> res = new ArrayList<int[]>();

        for(int i = 1; i <= limit; i++) {
            long delta = 1-4*(i-(long)i*i-2*target);
            if (delta < 0) continue;
            int delta_sqrt = (int)Math.sqrt(delta + 0.5);//为了不损失精度，且用于以下判断，转为int后下式还相等，则说明b^2-4ac 开根为整数

            if((long)delta_sqrt*delta_sqrt == delta && (delta_sqrt-1)%2 == 0) {
                int x = (-1 + delta_sqrt)/2;

                if(x > i){
                    int[] temp = new int[x-i+1];
                    for(int j = i; j <= x; j++) {
                        temp[j-i] = j;
                    }
                    res.add(temp);
                }
            }
        }

        return res.toArray(new int[res.size()][]);
    }
}
~~~

- 时间复杂度：O(target)
- 空间复杂度：O(1)



### 解 2：双指针

我们用两个指针 l 和 r 表示当前枚举到的以 l 为起点到 r 的区间，sum 表示 [l, r] 的区间和，由求和公式可 O(1) 求得为 sum= (l+r)∗(r−l+1)/2，起始 l=1,r=2。

一共有三种情况：

如果sum<target 则说明指针 r 还可以向右拓展使得 sum 增大，此时指针 r 向右移动，即 r+=1
如果 sum>target 则说明以 l 为起点不存在一个 r 使得 sum=target，此时要枚举下一个起点，指针 l 向右移动，即l+=1
如果 sum==target 则说明我们找到了以 ll 为起点得合法解 [l, r] ，我们需要将 [l, r] 的序列放进答案数组，且我们知道以 l 为起点的合法解最多只有一个，所以需要枚举下一个起点，指针 l 向右移动，即 l+=1
终止条件即为 l>=r 的时候，这种情况的发生指针 r 移动到了⌊target/2⌋+1 的位置，导致 l<r 的时候区间和始终大于target。

此方法其实是对暴力法的优化，因为暴力法是没有考虑区间与区间的信息可以复用，只是单纯的枚举起点，然后从起点开始累加，而该方法就是考虑到了如果已知 [l, r] 的区间和等于 target ，那么枚举下一个起点的时候，区间 [l+1,r] 的和必然小于 target ，我们就不需要再从 l+1 再开始重复枚举，而是从 r+1 开始枚举，充分的利用了已知的信息来优化时间复杂度。

~~~java
class Solution {
    public int[][] findContinuousSequence(int target) {
        int l=1, r=2;
        List<int[]> res = new ArrayList<>();

        while(l< r){
            int sum = (l+r)*(r-l+1)/2;
            if (sum < target) r++;
            else if (sum > target) l++;
            else{
                int[] temp = new int[r-l+1];
                for(int j = l; j <= r; j++) {
                    temp[j-l] = j;
                }
                res.add(temp);
                l++;
            }
        }

        return res.toArray(new int[res.size()][]);
    }
}
~~~

- 时间复杂度：O(target)
- 空间复杂度：O(1)