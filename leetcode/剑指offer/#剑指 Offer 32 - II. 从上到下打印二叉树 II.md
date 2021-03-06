# #[剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

例如:
给定二叉树: `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其层次遍历结果：

```
[
  [3],
  [9,20],
  [15,7]
]
```

##  提示：

1. `节点总数 <= 1000`

## 解题思路：

### 解 1：递归

为每一层设置一个变量 level 以在递归的时候维护相应的层。

~~~java
class Solution {

    private List<List<Integer>> out = new ArrayList<List<Integer>>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        if (root == null) return out;
        out.add(new ArrayList<Integer>());
        dfs (root, 0);
        return out;
    }

    private void dfs(TreeNode root, int level) {
        if (root == null) return;
        if (out.size() - 1 < level) {
            ArrayList<Integer> temp = new ArrayList<Integer>();
            temp.add(root.val);
            out.add(temp);
        }
        else {
            out.get(level).add(root.val);
        }
        dfs(root.left, level + 1);
        dfs(root.right, level + 1);
    }
}
~~~

- 时间复杂度：O(n)遍历每一个节点
- 空间复杂度：最好情况下O(h)，h为树的高度；最坏情况下，退化为链表，空间复杂度O(n)

### 解 2：迭代

迭代开始之前记录队列的size()，即当前层的节点数。

~~~java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        List<List<Integer>> res = new ArrayList<>();
        if(root != null) queue.add(root);
        while(!queue.isEmpty()) {
            List<Integer> tmp = new ArrayList<>();
            for(int i = queue.size(); i > 0; i--) {
                TreeNode node = queue.poll();
                tmp.add(node.val);
                if(node.left != null) queue.add(node.left);
                if(node.right != null) queue.add(node.right);
            }
            res.add(tmp);
        }
        return res;
    }
}
~~~

时间复杂度 O(N) ： N 为二叉树的节点数量，即 BFS 需循环 N 次。
空间复杂度 O(N) ： 最差情况下，即当树为平衡二叉树时，最多有 N/2 个树节点同时在 queue 中，使用 O(N) 大小的额外空间。

