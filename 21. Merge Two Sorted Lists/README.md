# 21. Merge Two Sorted Lists

`Linked List`, `Recursion`

## Problem

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.


### Example

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

```
Input: list1 = [], list2 = [0]
Output: [0]
```

### Constraints
* The number of nodes in both lists is in the range `[0, 50]`.
* `-100 <= Node.val <= 100`
* Both `list1` and `list2` are sorted in **non-decreasing** order.

---

## Idea

### 1. Iterative Comparison

將 `list1` 和 `list2` 當前的 value 逐一比較大小，再決定新的 linkedlist 的 next 要接上 `list1` 或 `list2`。

* **Note:**
    * `current.next` 決定接上 `list1` 或 `list2` 後，需要將 `list1` 或 `list2` 更新為剩下的部分 (把頭斷掉：`list1 = list1.next`)
    * 因為 while loop 只要在其中一個 list 為空就會停止，所以最後要記得將不為空的 list 接上。
    * 因為要輸出的結果為 **the head of the merged linked list**，意思就是輸出最後的 list，故要將一開始的 `LinkedList` 指向 `next` (因為初始的第一個 value=0)。

```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def mergeTwoLists(self, list1, list2):
        """
        :type list1: Optional[ListNode]
        :type list2: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        LinkedList = ListNode()
        current = LinkedList
        while list1 and list2:
            if list1.val < list2.val:
                current.next = list1
                list1 = list1.next
            else:
                current.next = list2
                list2 = list2.next
            current = current.next
        
        current.next = list1 or list2
        return LinkedList.next
```
* Time complexity: $O(n+m)$
    * `n` is length of `list1` and `m` is length of `list2`
* Space complexity: $O(1)$

### 2. Recursion Comparison

邏輯相同，只是把 while loop 改為遞迴的形式。

* **Note:**
    * 最一開始的 if/else 主要是要判斷有沒有 list 為空，若有就直接 return 另一個 list。
    * 接下來就是根據大小的判斷決定目前的 list 要怎麼往下接。

```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def mergeTwoLists(self, list1, list2):
        """
        :type list1: Optional[ListNode]
        :type list2: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        if not list1 or not list2:
            return list1 if list1 else list2

        if list1.val < list2.val:
            list1.next = self.mergeTwoLists(list1.next, list2)
            return list1
        else:
            list2.next = self.mergeTwoLists(list1, list2.next)
            return list2
```
* Time complexity: $O(n+m)$
* Space complexity: $O(n+m)$ ?
    * The depth of call stack takes $O(n+m)$ ?