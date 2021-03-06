# #[剑指 Offer 56 - II. 数组中数字出现的次数 II](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

在一个数组 `nums` 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。

## 示例 1：

```
输入：nums = [3,4,3,3]
输出：4
```

## 示例 2：

```
输入：nums = [9,1,7,9,7,9,7]
输出：1
```

## 限制：

- `1 <= nums.length <= 10000`
- `1 <= nums[i] < 2^31`



## 解题思路：

### 解 1：HashMap

第一时间想到遍历统计即可，代码很简单：

~~~java
class Solution {
    public int singleNumber(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i : nums) {
            map.put(i, map.getOrDefault(i, 0) + 1);
        }

        for (int i : map.keySet()) {
            if(map.get(i) == 1) return i;
        }
        return 0;
    }
}
~~~

### 解 2：状态机

但是看了题解，发现自己太年轻。。。。看得似懂非懂，贴上链接，日后仔细琢磨：

https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/solution/zhuang-tai-ji-jie-jue-ci-wen-ti-xiang-jie-shu-zi-d/