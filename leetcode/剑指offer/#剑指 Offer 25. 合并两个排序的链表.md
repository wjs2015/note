# #[剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

## 示例1：

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

## 限制：

```
0 <= 链表长度 <= 1000
```



## 解题思路：

双指针，各指向一个链表，依次比较两个指针指向节点的大小，将小的添加到新链表（改变链接而不是新建）且指针向后移动一步，大的原地不动，直到其中一个指针为空。对于头结点，可新建一个伪头结点，将其他节点链接到伪节点之后，最后返回伪节点下一个节点即可。

流程：

- 新建伪头结点 head。新建指针 cur 指向head
- 判断是否有空链表，有则直接返回另一个链表。
- 循环比较两指针指向的节点，cur.next指向其中小者，并将cur后移（指向cur.next)
- 循环到其中一个指针为空，将cur.next指向另一个链表，结束。

~~~java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {

        ListNode head = new ListNode(0);
        ListNode cur = head;

        if (l1 == null) return l2;
        if (l2 == null) return l1;

        while (l1 != null && l2 != null) {
            if(l1.val < l2.val) {
                cur.next = l1;
                l1 = l1.next;
            }
            else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }

        cur.next = l1 == null ? l2 : l1;

        return head.next;
    }
}
~~~

- 时间复杂度：O(m+n)，m和n分别为两条链表的长度。
- 空间复杂度：O(1)，只新建了两个节点引用head和cur，使用常数大小的额外空间。