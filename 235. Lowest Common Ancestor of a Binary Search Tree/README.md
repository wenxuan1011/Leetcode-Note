# 235. Lowest Common Ancestor of a Binary Search Tree

`Tree`, `BFS`, `DFS`, `Binary Tree`

## Problem

Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”


### Example

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
Output: 6
Explanation: The LCA of nodes 2 and 8 is 6.
```

```
Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
Output: 2
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.

```

### Constraints
* The number of nodes in the tree is in the range `[2, 10^5]`.
* `-10^9 <= Node.val <= 10^9`
* All `Node.val` are **unique**.
* `p != q`
* `p` and `q` will exist in the BST.

---

## Idea

### 1. Recursive - DFS

很直覺的去分成要往左走或往右走，若不能再往下走了就回傳 `root`。

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if root is None:
            return None
            
        if(p.val>root.val and q.val>root.val):
            return self.lowestCommonAncestor(root.right, p, q)
        elif(p.val<root.val and q.val<root.val):
            return self.lowestCommonAncestor(root.left, p, q)
        else:
            return root
```
* Time complexity: $O(h)$, where $h$ is the height of the BST.
    * Halving the binary search tree during searching takes $O(log\space n)$
 in total.
    * The worst case happens when the BST is skewed, which takes $O(n)$ in total.
* Space complexity: $O(h)$
    * The worst case occurs when the BST is skewed, in which the depth of call stack takes $O(h)≈O(n)$.


### 2. Iterative - DFS (?)

改成用 while loop 去做

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if root is None:
            return None
        qu = deque([root])
        while q:
            visited = qu.popleft()
            if(p.val>visited.val and q.val>visited.val):
                qu.append(visited.right)
            elif(p.val<visited.val and q.val<visited.val):
                qu.append(visited.left)
            else:
                return visited
```
* Time complexity: $O(h)$
* Space complexity: $O(1)$
