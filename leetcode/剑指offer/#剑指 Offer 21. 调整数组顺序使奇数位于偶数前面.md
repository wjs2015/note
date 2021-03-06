# #[剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

## 示例：

```
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```

## 提示：

1. `1 <= nums.length <= 50000`
2. `1 <= nums[i] <= 10000`

## 解题思路：

### 解 1：首尾双指针

定义头指针 left，尾指针 right .
left 一直往右移，直到它指向的值为偶数
right 一直往左移， 直到它指向的值为奇数
交换 nums[left] 和 nums[right] .
重复上述操作，直到 left==right .

~~~java
class Solution {
    public int[] exchange(int[] nums) {
        int l = 0, r = nums.length-1;

        while(l < r) {
            if((nums[l] & 1) != 0){
                l++;
                continue;
            }
            if((nums[r] & 1) != 1){
                r--;
                continue;
            }

            int temp = nums[l];
            nums[l] = nums[r];
            nums[r] = temp;
            l++;
            r--;
        }

        return nums;
    }
}
~~~

- 时间复杂度：O(N)
- 空间复杂度：O(1)

### 解 2：快慢指针

定义快慢双指针 fast 和 lowlow ，fast 在前， low 在后 .
fast 的作用是向前搜索奇数位置，low 的作用是指向下一个奇数应当存放的位置
fast 向前移动，当它搜索到奇数时，将它和 nums[low] 交换，此时 low 向前移动一个位置 .
重复上述操作，直到 fast 指向数组末尾 .

~~~java
class Solution {
    public int[] exchange(int[] nums) {
        int low = 0, fast = 0;

        while(fast < nums.length) {
            if((nums[fast] & 1) == 1) {
                int temp = nums[fast];
                nums[fast] = nums[low];
                nums[low] = temp;
                low++;
            }
            fast++;
        }
        return nums;
    }
}
~~~

- 时间复杂度：O(N)
- 空间复杂度：O(1)

注：判断奇数和偶数，只需和1做位与即可判断，结果为1为奇数，结果为0为偶数。