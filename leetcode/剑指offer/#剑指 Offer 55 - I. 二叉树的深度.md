# #[剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

例如：

给定二叉树 [3,9,20,null,null,15,7]，

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 。

## 提示：

1. 节点总数 <= 10000

## 解题思路：深度优先和广度优先

### 解 1：深度优先递归写法：

终止条件：遇到空结返回0点

依次计算根结点左子树和右子树的深度，对于根结点，取两棵子树中深度较大的一颗，并在其基础上加1，得到根结点的深度。

~~~java
class Solution {
    public int maxDepth(TreeNode root) {
        if ( root == null) return 0;
        return Math.max(maxDepth(root.left),maxDepth(root.right)) + 1;
    }
}
~~~

时间复杂度 O(N) ： N 为树的节点数量，计算树的深度需要遍历所有节点。
空间复杂度 O(N) ： 最差情况下（当树退化为链表时），递归深度可达到 N。

### 解 2：广度优先：

遍历每层，计数器加1

初始化两个列表：queue和temp，queue用于存储当前层结点，用来遍历，temp用于临时存储下一层节点，遍历完queue之后（即遍历完一层）将temp赋给queue（即到了下一层），计数器加1。

~~~java
class Solution {
    public int maxDepth(TreeNode root) {
        LinkedList<TreeNode> queue = new LinkedList<>();
        if (root == null) return 0;
        queue.add(root);
        LinkedList<TreeNode> temp;
        int out = 0;
        while (!queue.isEmpty()) {
            temp = new LinkedList<>();
            for (TreeNode i : queue) {
                if (i.left != null) temp.add(i.left);
                if (i.right != null) temp.add(i.right);
            }
            queue = temp;
            out++;
        }
        return out;
    }
}
~~~

时间复杂度 O(N) ： N 为树的节点数量，计算树的深度需要遍历所有节点。
空间复杂度 O(N)： 最差情况下（当树平衡时），队列 queue 同时存储 N/2 个节点。
