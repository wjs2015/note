# #[剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

给定一棵二叉搜索树，请找出其中第k大的节点。

## 示例 1:

```
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 4
```

## 示例 2：

```
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 4
```

## 限制：

1 ≤ k ≤ 二叉搜索树元素个数

### 解题思路：

中序遍历为升序，这里可以采用中序遍历的倒序（先右再左），每遍历一个节点，将k减1，若k为0，则当前节点就是所查找的节点，结束递归。

~~~java
class Solution {
    int k, out;
    public int kthLargest(TreeNode root, int k) {
        this.k = k;
        helper(root);
        return out;
    }
    private void helper(TreeNode root) {
        if (root == null) return; 
        helper(root.right);
        if(--k == 0) {
            out = root.val;
            return;
        }
        helper(root.left);
    }
}
~~~

- 时间复杂度 O(N) ： 当树退化为链表时（全部为右子节点），无论 k 的值大小，递归深度都为 N ，占用 O(N) 时间。
- 空间复杂度 O(N) ： 当树退化为链表时（全部为右子节点），系统使用 O(N) 大小的栈空间。