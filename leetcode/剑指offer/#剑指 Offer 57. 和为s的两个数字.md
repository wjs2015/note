# #[剑指 Offer 57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

输入一个递增排序的数组和一个数字s，在数组中查找两个数，使得它们的和正好是s。如果有多对数字的和等于s，则输出任意一对即可。

## 示例 1：

```
输入：nums = [2,7,11,15], target = 9
输出：[2,7] 或者 [7,2]
```

## 示例 2：

```
输入：nums = [10,26,30,31,47,60], target = 40
输出：[10,30] 或者 [30,10]
```

##  限制：

- `1 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^6`

## 解题思路 ：

### 解 ：双指针

- 初始化： 双指针 i , j 分别指向数组 nums 的左右两端 （俗称对撞双指针）。
  - 循环搜索： 当双指针相遇时跳出；
    - 计算和 s=nums[i]+nums[j] ；
    - 若 s > target ，则指针 j 向左移动，即执行 j = j - 1 ；
    - 若 s < target ，则指针 i 向右移动，即执行 i = i + 1 ；
    - 若 s = target ，立即返回数组 [nums[i], nums[j]] ；

- 返回空数组，代表无和为 target 的数字组合。

~~~java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int l = 0, r = nums.length - 1;

        while(l < r) {
            if(nums[l] + nums[r] < target) l++;
            else if(nums[l] + nums[r] > target) r--;
            else {
                int[] out = new int[2];
                out[0] = nums[l];
                out[1] = nums[r];
                return out;
            }
        }

        return new int[0];
    }
}
~~~

- 时间复杂度：O(N)
- 空间复杂度：O(1)

正确性证明：https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/solution/mian-shi-ti-57-he-wei-s-de-liang-ge-shu-zi-shuang-/