# #[剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

## 示例1:

```
输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

## 提示：

- `1 <= arr.length <= 10^5`
- `-100 <= arr[i] <= 100`

## 解题思路：

### 解 1：动态规划

- 状态定义： 设动态规划列表 dp，dp[i] 代表以元素 nums[i] 为结尾的连续子数组最大和。
  - 为何定义最大和 dp[i] 中必须包含元素 nums[i] ：保证 dp[i] 递推到 dp[i+1] 的正确性；如果不包含 nums[i] ，递推时则不满足题目的连续子数组 要求。

- 转移方程： 若 dp[i−1]≤0 ，说明 dp[i−1] 对 dp[i] 产生负贡献，即 dp[i−1]+nums[i] 还不如 nums[i] 本身大。
  - 当 dp[i−1]>0 时：执行 dp[i]=dp[i−1]+nums[i] ；
  - 当 dp[i−1]≤0 时：执行 dp[i]=nums[i] ；
- 初始状态： dp[0]=nums[0]，即以 nums[0] 结尾的连续子数组最大和为 nums[0] 。
- 返回值： 返回 dp 列表中的最大值，代表全局最大值。

~~~java
class Solution {
    public int maxSubArray(int[] nums) {
        int dp = nums[0];
        int res = nums[0];
        for(int i=1; i<nums.length; i++) {
            if (dp <= 0) dp = nums[i];
            else dp = dp + nums[i];
            res = Math.max(dp, res);
        }
        return res;
    }
}
~~~

时间复杂度 O(N) ： 线性遍历数组 nums 即可获得结果，使用 O(N) 时间。
空间复杂度 O(1) ： 使用常数大小的额外空间。

### 解 2：分治思想

~~~java
/*
 * 方法二 分治法 O(nlogn)
 *
 * 分治法模板：
 * 1. 定义基本情况
 * 2. 将问题分解为子问题并递归解决子问题
 * 3. 合并子问题的解以获得原始问题的解
 *
 * 将nums由中点mid分为三种情况：
 * 1. 最大子串在左边
 * 2. 最大子串在右边
 * 3. 最大子串跨中点，左右都有
 *
 * 当子串在左边或右边时，继续分中点递归分解到一个数为止，
 * 对于递归后横跨的子串，再分治为左侧和右侧求最大子串，
 * 可使用贪心算法求最大子串值，再合并为原始的最大子串值
 * */


class Solution {
    public int maxSubArray(int[] nums) {
        if(nums.length == 0) return 0;
        return helper(nums, 0, nums.length-1);
    }

    private int helper(int[] nums, int left, int right) {
        if (left == right) {
            return nums[left];
        }

        // 求中点值
        int mid = left + (right - left) / 2;

        // 中点左边的最大值
        int leftSum = helper(nums, left, mid);
        // 中点右边的最大值
        int rightSum = helper(nums, mid + 1, right);
        // 横跨中点的最大值
        int croSum = crossSum(nums, left, right, mid);

        if (leftSum>rightSum) return Math.max(leftSum, croSum);
        else return Math.max(rightSum, croSum);

    }

    private int crossSum(int[] nums, int left, int right, int mid) {
        if (left == right) {
            return nums[left];
        }

        // 贪心法求左边的最大值
        int leftSubsum = Integer.MIN_VALUE;
        int curSum = 0;
        for (int i = mid; i > left - 1; i--) {
            curSum += nums[i];
            leftSubsum = Math.max(leftSubsum, curSum);
        }

        // 贪心法求右边的最大值
        int rightSubsum = Integer.MIN_VALUE;
        curSum = 0;
        for (int i = mid + 1; i < right + 1; i++) {
            curSum += nums[i];
            rightSubsum = Math.max(rightSubsum, curSum);
        }

        return leftSubsum + rightSubsum;
    }
}
~~~

- 时间复杂度：O(nlogn)
- 空间复杂度：O(logn)