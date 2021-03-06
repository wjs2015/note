# #[剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

##  示例 1：

```
输入：head = [1,3,2]
输出：[2,3,1]
```

##  限制：

```
0 <= 链表长度 <= 10000
```

## 解题思路：

### 解 1：

使用辅助栈，先进后出。遍历链表，一次将节点压入栈中，完成之后获取栈的大小，创建相应容量的数组，一次从栈中取出元素放入数组中。

~~~java
class Solution {
    public int[] reversePrint(ListNode head) {
        Stack<ListNode> stack = new Stack<>();
        while (head != null) {
            stack.push(head);
            head = head.next;
        }
        int size = stack.size();
        int[] out = new int[size];
        for (int i = 0; i < size; i++) {
            out[i] = stack.pop().val;
        }
        return out;
    }
}
~~~

- 时间复杂度：O(n)
- 空间复杂度：O(n)

### 解 2：

递归法，先递归到链表末尾，然后再回溯，回溯过程中将节点加入临时列表，最后将列表转换为数组。

~~~java
class Solution {

    ArrayList<Integer> res;

    public int[] reversePrint(ListNode head) {
       res = new ArrayList<>();
       helper(head);
       int size = res.size();
       int[] out = new int[size];
       for (int i = 0; i < size; i++) {
           out[i] = res.get(i);
       }
       return out;
    }

    private void helper(ListNode head){
        if (head == null) return;
        helper(head.next);
        res.add(head.val);
    }
}
~~~

- 时间复杂度：O(n)
- 空间复杂度：O(n)，递归占用栈空间

