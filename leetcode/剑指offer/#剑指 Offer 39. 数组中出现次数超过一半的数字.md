# #[剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

## 示例 1:

```
输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
输出: 2
```

## 限制：

```
1 <= 数组长度 <= 50000
```

## 解题思路：

### 解 1：HashMap统计

~~~java
class Solution {
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i : nums) {
            int val = map.getOrDefault(i, 0);
            if(val >= nums.length/2) return i;
            map.put(i, val+1);
        }
        return -1;
    }
}
~~~

- 时间复杂度：O(N)
- 空间复杂度：O(N)

### 解 2：数组排序法，将数组 nums 排序，数组中点的元素一定为众数

~~~java
class Solution {
    public int majorityElement(int[] nums) {
       Arrays.sort(nums);
       return nums[nums.length/2];
    }
}
~~~

- 时间复杂度：O(NlogN)
- 空间复杂度：O(1)

### 解 3：摩尔投票法

设输入数组 nums 的众数为 x ，数组长度为 n 。

推论一： 若记 众数 的票数为 +1 ，非众数 的票数为 −1 ，则一定有所有数字的 票数和 > 0 。

推论二： 若数组的前 a 个数字的 票数和 =0 ，则 数组剩余 (n−a) 个数字的 票数和一定仍 >0 ，即后 (n−a) 个数字的 众数仍为 x 。

根据以上推论，假设数组首个元素 n1为众数，遍历并统计票数。当发生票数和 =0 时，剩余数组的众数一定不变 ，这是由于：

当 n1=x ： 抵消的所有数字，有一半是众数 x 。
当 n1 != x ： 抵消的所有数字，少于或等于一半是众数 x 。
利用此特性，每轮假设发生 票数和 =0 都可以缩小剩余数组区间 。当遍历完成时，最后一轮假设的数字即为众数。

算法流程:

- 初始化： 票数统计 votes = 0 ， 众数 x；
- 循环： 遍历数组 nums 中的每个数字 num ；
- - 当 票数 votes 等于 0 ，则假设当前数字 num 是众数；
  - 当 num = x 时，票数 votes 自增 1 ；当 num != x 时，票数 votes 自减 1 ；

- 返回值： 返回 x 即可；

~~~java
class Solution {
    public int majorityElement(int[] nums) {
        int x = 0, votes = 0;

        for(int i : nums) {
            if(votes == 0) x = i;
            votes += i==x ? 1:-1; 
        }
        return x;
    }
}
~~~

- 时间复杂度：O(N)
- 空间复杂度：O(1)