# #[剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

例如输入：

   4
  /  \
 2   7
 / \  / \
1  3 6  9
镜像输出：

   4
  /  \
 7   2
 / \  / \
9  6 3  1

## 示例 1：

输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]

## 限制：

0 <= 节点个数 <= 1000

## 解题思路：

### 解 1：深度优先搜索，递归地遍历二叉树（DFS）

- 终止条件：root为空时，返回null；
- 递归地将去交换左右子树；
- 交换完成之后返回root结点。

~~~java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if (root == null) return null;

        TreeNode rootLeft = mirrorTree(root.left);
        TreeNode rootRight = mirrorTree(root.right);

        root.left = rootRight;
        root.right = rootLeft;

        return root;
    }
}
~~~

- 时间复杂度 O(N)： 其中 N 为二叉树的节点数量，建立二叉树镜像需要遍历树的所有节点，占用 O(N)时间。
- 空间复杂度 O(N)： 最差情况下（当二叉树退化为链表），递归时系统需使用 O(N) 大小的栈空间。



### 解 2：辅助栈（或队列，栈为深度优先，队列为广度优先）

利用栈（或队列）遍历树的所有节点 node，并交换每个 node 的左 / 右子节点。

算法流程：

- 特例处理： 当 root 为空时，直接返回 null ；
- 初始化： 栈（或队列），本文用栈，并加入根节点 root 。
- 循环交换： 当栈 stack 为空时跳出；
  - 出栈： 记为 node ；
  - 添加子节点： 将 node 左和右子节点入栈；
  - 交换： 交换 node 的左 / 右子节点。

- 返回值： 返回根节点 root。

~~~java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if (root == null) return null;
        Stack<TreeNode> stack = new Stack<>();

        stack.add(root);

        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            if (node.left!=null) stack.add(node.left);
            if (node.right!=null) stack.add(node.right);

            TreeNode temp = node.left;
            node.left = node.right;
            node.right = temp;
        }
        return root;
    }
}
~~~

- 时间复杂度 O(N)： 其中 N 为二叉树的节点数量，建立二叉树镜像需要遍历树的所有节点，占用 O(N) 时间。
- 空间复杂度 O(N)： 最差情况下（当为满二叉树时），栈 stack 最多同时存储 N/2个节点，占用 O(N) 额外空间。