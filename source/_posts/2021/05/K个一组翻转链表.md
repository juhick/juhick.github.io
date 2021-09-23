---
title: K个一组翻转链表
date: 2021-05-12 21:32:54
tags:
 - leetcode
 - 链表
categories:
 - leetcode
math:
---

# K个一组翻转链表

[原题链接](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

## 题目描述

给你一个链表，每 *k* 个节点一组进行翻转，请你返回翻转后的链表。

*k* 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 *k* 的整数倍，那么请将最后剩余的节点保持原有顺序。

<!-- more -->

**进阶：**

- 你可以设计一个只使用常数额外空间的算法来解决此问题吗？
- **你不能只是单纯的改变节点内部的值**，而是需要实际进行节点交换。

 **示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

```
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

**示例 3：**

```
输入：head = [1,2,3,4,5], k = 1
输出：[1,2,3,4,5]
```

**示例 4：**

```
输入：head = [1], k = 1
输出：[1]
```

**提示：**

- 列表中节点的数量在范围 `sz` 内
- `1 <= sz <= 5000`
- `0 <= Node.val <= 1000`
- `1 <= k <= sz`

## 思路分析

因为每次需要翻转K个数据，所以可以首先实现一个翻转指定区间`[a, b)`内的数据，只需要将标准的翻转函数的判断条件`cur == null`修改为`cur == b`即可实现，如下所示：

```java
public ListNode reverse(ListNode a, ListNode b){
        ListNode pre = null, cur = a, nxt;
        while(cur != b){
            nxt = cur.next;
            cur.next = pre;
            pre = cur;
            cur = nxt;
        }
        return pre;
}
```

然后每次获取到一组的右端端点即可完成K个一组翻转，不足时则不翻转，函数实现如下：

```java
public ListNode reverseKGroup(ListNode head, int k) {
        if(head == null) return null;
        ListNode a, b;
        a = b = head;
        for(int i = 0; i < k; i++){
            if(b == null) return head;
            b = b.next;
        }
        ListNode newHead = reverse(a, b);
        a.next = reverseKGroup(b, k);
        return newHead;
    }
```

## 代码实现

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {

    public ListNode reverse(ListNode a, ListNode b){
        ListNode pre = null, cur = a, nxt;
        while(cur != b){
            nxt = cur.next;
            cur.next = pre;
            pre = cur;
            cur = nxt;
        }
        return pre;
    }

    public ListNode reverseKGroup(ListNode head, int k) {
        if(head == null) return null;
        ListNode a, b;
        a = b = head;
        for(int i = 0; i < k; i++){
            if(b == null) return head;
            b = b.next;
        }
        ListNode newHead = reverse(a, b);
        a.next = reverseKGroup(b, k);
        return newHead;
    }
}
```

