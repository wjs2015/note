# #[剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

## 示例 1:

```
输入: [0,1,3]
输出: 2
```

## 示例 2:

```
输入: [0,1,2,3,4,5,6,7,9]
输出: 8
```

## 限制：

```
1 <= 数组长度 <= 10000
```

## 解题思路：

### 解 1：遍历数组，若元素值与下标值不同则返回下标志。

~~~java
class Solution {
    public int missingNumber(int[] nums) {
        for(int p = 0; p < nums.length; p++) {
            if(nums[p]!=p) return p;
        }

        return nums[nums.length-1] + 1;//说明缺失的是最后一个数字

    }
}
~~~

- 时间复杂度：O(n)
- 空间复杂度：O(1)

### 解 2：二分查找

排序数组中的搜索问题，想到 二分法 解决。
根据题意，数组可以按照以下规则划分为两部分。

- 左子数组： nums[i] == i ；
- 右子数组： nums[i] ！= i ；

缺失的数字等于 **“右子数组的首位元素”** 对应的索引；因此考虑使用二分法查找 “右子数组的首位元素” 。

算法解析：

- 初始化： 左边界 i = 0 ，右边界 j = len(nums) - 1 ；代表闭区间 [i, j] 。
- 循环二分： 当i≤j 时循环 （即当闭区间 [i, j] 为空时跳出） ； 计算中点 m = (i + j) // 2 ，其中 "//" 为向下取整除法； 若 nums[m] == m ，则 “右子数组的首位元素” 一定在闭区间 [m + 1, j] 中，因此执行 i = m + 1；
  若 nums[m] !=m ，则 “左子数组的末位元素” 一定在闭区间 [i, m - 1] 中，因此执行 j = m - 1；
- 返回值： 跳出时，变量 i 和 j 分别指向 “右子数组的首位元素” 和 “左子数组的末位元素” 。因此返回 i 即可。

~~~java
class Solution {
    public int missingNumber(int[] nums) {
        int i = 0, j = nums.length-1;

        while(i <= j) {
            int mid = (i+j)/2;

            if(nums[mid] == mid) i = mid +1;
            else j = mid - 1;
        }
        return i;
    }
}
~~~

- 时间复杂度 O(logn)： 二分法为对数级别复杂度。
- 空间复杂度 O(1)： 几个变量使用常数大小的额外空间。