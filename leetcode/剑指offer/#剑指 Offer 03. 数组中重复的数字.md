# #[剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

找出数组中重复的数字。
在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

## 示例 1：

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

## 限制：

```
2 <= n <= 100000
```

## 解题思路：

### 解 1：额外数组

因所有数字都在 0～n-1 的范围内，所以可以使用大小为n的数组记录每个元素出现的次数，一旦某一元素出现次数大于1，则可返回。

~~~java
class Solution {
    public int findRepeatNumber(int[] nums) {
        int[] temp = new int[nums.length];
        for(int i = 0; i < temp.length; i++) {
            temp[i] = 0;
        }

        for(int i : nums) {
            temp[i]++;
            if (temp[i] > 1) return i;
        }
        return -1;
    }
}
~~~

- 时间复杂度：O(N)
- 空间复杂度：O(N)

### 解 2：使用集合

添加不成功即重复。

~~~java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        int repeat = -1;
        for (int num : nums) {
            if (!set.add(num)) {
                repeat = num;
                break;
            }
        }
        return repeat;
    }
}
~~~

- 时间复杂度：O(N)
- 空间复杂度：O(N)

### 解 3：原地交换

如果没有重复数字，那么正常排序后，数字i应该在下标为i的位置，所以思路是重头扫描数组，遇到下标为i的数字如果不是i的话（假设为m)，那么我们就拿之与下标m的数字交换。在交换过程中，如果有重复的数字发生，那么终止返回ture（一个萝卜一个坑）。交换过程可理解为给萝卜找坑的过程。

~~~java
class Solution {
    public int findRepeatNumber(int[] nums) {
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] == i) continue;

            if(nums[i] == nums[nums[i]]) return nums[i];

            int temp = nums[nums[i]];
            nums[nums[i]] = nums[i];
            nums[i] = temp;
        }
        return -1;
    }
}
~~~

- 时间复杂度：O(N)
- 空间复杂度：O(1)

