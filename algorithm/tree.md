
给定一个二叉树，返回其节点值自底向上的层次遍历。（即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）
例如：给定二叉树 [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
返回其自底向上的层次遍历为：
[
  [15,7],
  [9,20],
  [3]
]

```python?linenums
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def levelOrderBottom(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        res = []
        if root:
            res.append([root.val])
            stack = [root]
            swap_stack = []
            while len(stack)>0:
                tmp = []
                for node in stack:
                    if node.left:
                       swap_stack.append(node.left)
                       tmp.append(node.left.val) 
                    if node.right:
                        swap_stack.append(node.right)
                        tmp.append(node.right.val)
                if tmp:
                    res.insert(0,tmp)
                stack = swap_stack
                swap_stack=[]
        return res
        
```
