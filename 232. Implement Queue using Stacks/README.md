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

### 1. Intuition

用一個 stack (`in_stack`) 存放放進來的元素，另一個 stack (`out_stack`) 負責將 `in_stack` 的順序全部倒過來，以完成 FIFO 的效果。

為了要將順序倒過來，可以寫一個 `transfer()` 的 function 方便在 pop/peek 時使用。

```python
class MyQueue(object):

    def __init__(self):
        self.in_stack = []
        self.out_stack = []


    def transfer(self):
        while self.in_stack:
            self.out_stack.append(self.in_stack.pop())


    def push(self, x):
        """
        :type x: int
        :rtype: None
        """
        self.in_stack.append(x)


    def pop(self):
        """
        :rtype: int
        """
        if not self.out_stack:
            self.transfer()
        return self.out_stack.pop()
        

    def peek(self):
        """
        :rtype: int
        """
        if not self.out_stack:
            self.transfer()
        return self.out_stack[-1]
        

    def empty(self):
        """
        :rtype: bool
        """
        return not self.in_stack and not self.out_stack
        

# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```
* Time complexity:
    * **Push:** $O(1)$.
    * **Pop/Peek:** 
        * $O(n)$ in the worst case (when `out_stack` is empty, and all elements need to be moved from `in_stack` to `out_stack`). 
        * Amortized complexity is $O(1)$ per operation since each element is moved at most once.
* Space complexity: $O(n)$
    * Storage required for the two stacks.