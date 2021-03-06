# #[剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

## 示例:

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

## 限制：

```
0 <= 节点个数 <= 5000
```

## 解题思路：

### 解 1：双指针

申请两个指针，第一个指针叫 pre，最初是指向 null 的。
第二个指针 cur 指向 head，然后遍历 链表。
每次迭代，都将 cur 的 next 指向 pre，然后 pre 和 cur 前进一位。
都迭代完成(cur 变成 null 了)，pre 就是最后一个节点了。

（还需一个指针tmp保存cur.next)

~~~java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        ListNode tmp = null;

        while(cur != null) {
            tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp;
        }

        return pre;
    }
}
~~~

- 时间复杂度：O(n)
- 空间复杂度：O(1)，不需要额外空间

### 解 2：

递归到链表末尾，回溯的时候改变链接。

~~~java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;

        ListNode cur = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return cur;
    }
}
~~~

- 时间复杂度：O(n)
- 空间复杂度：O(n)，n层递归