# 232. Implement Queue using Stacks

`Stack`, `Design`, `Queue`

## Problem

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

### Example

```
Input:
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
Output:
[null, null, null, 1, 1, false]

Explanation:
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```


### Constraints
* `1 <= x <= 9`
* At most `100` calls will be made to `push`, `pop`, `peek`, and `empty`.
* All the calls to `pop` and `peek` are valid.
---

## Idea

### 1. Linked List Traversal (Brute-force)

記錄下所有已經拜訪過的 node，如果現在拜訪的 node.next 已存在在拜訪過的 node 中，則有 cycle。

* **Note:**
    * `visited` 用 set 或 list 都可以，但判斷元素是否在其中 (in) 的搜索方法，**set 會比 list 快很多**。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        visited = set()
        while head is not None:
            if head in visited:
                return True
            visited.add(head)
            head = head.next
        return False
```
* Time complexity: $O(n)$
    * Traverse the linked list only once.
    * Python set lookup takes $O(1)$ on average.
* Space complexity: $O(n)$
    * Use a `set` to record the visited nodes.


### 2. Cycle Detection via Slow-Fast Pointers

用兩個 pointer，一個走得快一個走得慢，如果有 cycle 存在，快的有一天會追上慢的。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        if head is None:
            return False
        fast = head
        slow = head
        while fast.next and fast.next.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                return True
        return False
```
* Time complexity: $O(n)$
* Space complexity: $O(1)$
