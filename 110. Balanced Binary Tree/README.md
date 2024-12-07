# 110. Balanced Binary Tree

`Tree`, `DFS`, `Binary Tree`

## Problem

Given a binary tree, determine if it is height-balanced.

* **height-balanced 的定義：**
    * 二元樹的左子樹是平衡的
    * 二元樹的右子樹是平衡的
    * 二元樹的左右子樹高的差不大於 1


### Example

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```

```
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false
```

```
Input: root = []
Output: true
```

### Constraints
* The number of nodes in the tree is in the range `[0, 5000]`.
* `-10^4 <= Node.val <= 10^4`


---

## Idea

### 1. Brute-force

1. 先定義一個 function (cal_height) 可以用來計算 subtree 的深度
2. 以遞迴的方式去進行，去比較左右兩邊的 subtree 是否都符合 height-balanced

* **Note:**
    * 是每個左右 subtree 間都要滿足 height-balanced，並非只有 root 左右的兩棵樹要滿足
    * 此方法幾乎就是把所有 node 都走過一次，計算出所有 subtree 的深度

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def isBalanced(self, root):
        """
        :type root: Optional[TreeNode]
        :rtype: bool
        """
        if root is None:
            return True

        r_height = self.cal_height(root.right)
        l_height = self.cal_height(root.left)

        if(abs(r_height-l_height) > 1):
            return False

        return self.isBalanced(root.right) and self.isBalanced(root.left)

    def cal_height(self, root):
        if root is None:
            return 0
        r = self.cal_height(root.right)
        l = self.cal_height(root.left)
        return max(r, l)+1
```
* Time complexity: $O(n^2)$
    * Running balance check (which takes $O(n)$ itself) for each node takes $O(n^2)$ in total.

* Space complexity: $O(n)$
    * Deriving the height of a single node takes $O(n)$.
        * Think about two extreme cases, perfect and skewed.



### 2. Optimized Recursion

分析上面的暴力解後，可以發現有許多高度重複被計算。為了避免重複，我們在推導高度時即時進行 height-balanced 的檢查。

The process is summarized as follows,

1. Go down as deep as we can.
2. Return the current height.
3. Run balance check based on heights.
    * Balanced is viewed as a function of heights.
4. Recursively do steps 2-3 till end.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def isBalanced(self, root):
        """
        :type root: Optional[TreeNode]
        :rtype: bool
        """
        def _dfs(root):
            if root is None:
                return 0
            r = _dfs(root.right)
            l = _dfs(root.left)
            if abs(r-l) > 1:
                balanced[0] = False
            return max(r, l)+1

        balanced = [True]
        _dfs(root)
        return balanced[0]
```
* Time complexity: $O(n)$
    * Visiting each node in the binary tree takes $O(n)$.
* Space complexity: $O(h)$, where $h$ is the height of the binary tree.
    * The worst case happens when the binary tree is extremely unbalanced, which has a call stack of depth $n$. Hence, it takes $O(n)$ space complexity.
