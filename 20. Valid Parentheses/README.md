# 20. Valid Parentheses

`String`, `Stack`

## Problem

Given a string s containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.


### Example

```
Input: s = "(]"
Output: false
```

```
Input: s = "([])"
Output: true
```

### Constraints
* `1 <= s.length <= 10^4`
* `s` consists of parentheses only `'()[]{}'`

---

## Idea

### 1. Brute-force (myself)

其實就是按照邏輯去逐一判斷服不符合。

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        stack = []
        for i in s:
            if i in '([{':
                stack.append(i)
            else:
                if stack==[]:
                    return False
                else:
                    if ((stack[-1]=='(' and i==')') or 
                        (stack[-1]=='[' and i==']') or 
                        (stack[-1]=='{' and i=='}')):
                        stack.pop()
                    else:
                        return False
        if stack == []:
            return True
        else:
            return False
```
* Time complexity: $O(n)$
* Space complexity: $O(n)$

### 2. Brute-force (simple)

其實邏輯一模一樣，都是按照順序去檢查，但寫的更簡潔。

* **Note:**
    * `not stack` 是在檢查 list 是否為空
    * `stack.pop()` 除了丟掉最後一個外，也可以直接把最後一個 element 取出來

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        stack = []
        mapping = {'(': ')', '[': ']', '{': '}'}
        for i in s:
            if i in mapping.keys():
                stack.append(i)
            else:
                if not stack or mapping[stack.pop()] != i:
                    return False
        return not stack
```
* Time complexity: $O(n)$
* Space complexity: $O(n)$