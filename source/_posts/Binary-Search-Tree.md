---
title: Binary Search Tree
date: 2019-06-18 15:15:23
tags: 
    - BST
    - Tree
categories:
    - [Algorithm, Tree]
---

## pre

```python
class TreeNode:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

## level first order/breadth first order print tree

```python
def print_tree(root):
    # use a FIFO queue to store trees to be printed
    stack = []
    if root is None:
        print(None, end='\t')
        return 
    while stack or root:
        if root is None:
            print('null', end='\t')
        else:
            # Print the value the current tree and put its children to queue
            print(root.val, end='\t')
            stack.append(root.left)
            stack.append(root.right)
        # Each time get a TreeNode from the front of the queue 
        root = stack.pop(0)
    print('\n')
```

## inorder traversal of BST
```python
def traverse(root: TreeNode):
    """ using stack """

    final = []

    while stack or root:
        while root:
            stack.append((root.val, root.right))
            root = root.left

        val, right = stack.pop()
        final.append(val)
        root = right
    return final
```

```python
def another_inorder_traverse(root: TreeNode):
    """In-Place method, Morris traversal
    Notice this method can not format a new flatten TreeNode in inorder-traversal.
    Although it can produce values in inorder-traversal.
    """
    cur = root
    final = []
    while cur:
        if not cur.left:
            final.add(cur.val)
            cur = cur.right
        else:
            pre = cur.left
            while pre.right:
                pre = pre.right
            pre.right = cur
            temp = cur
            cur = cur.left
            temp.left = None
    return final
```

```python
def inorder_recursive(root: TreeNode):
    """Recursive method"""
    if root is None:
        return []
    return inorder_recursive(root.left) + [root.val] + inorder_recursive(root.right)
```

## preorder traversal of BST
```python
def preorder(root: TreeNode):
    """Morris"""
    if root is None:
        return
    cur = root
    while cur:
        if not cur.left:
            cur = cur.right
        else:
            pre = cur.left
            while pre and pre.right:
                pre = pre.right
            pre.right = cur.right
            tmp = cur
            cur = cur.left
            tmp.right = cur
            tmp.left = None
```

```python
def preorder_stack(root: TreeNode):
    """With stack"""
    if root is None:
        return
    stack = []
    while stack or root:
        while root and root.left:
            tmp = root
            root = root.left
            if tmp.right:
                stack.append(tmp.right)
            tmp.right = root
            tmp.left = None

        if root.right:
            stack.append(root.right)
            root.right = None
        if not stack:
            break
        else:
            right = stack.pop()
            root.right = right
            root = root.right
```

```python
pre = None
def preorder_recursive(root: TreeNode):
    """Smart recursive"""
    if root is None:
        return
    preorder_recursive(root.right)
    preorder_recursive(root.left)
    root.right = pre
    root.left = None
    pre = root
```

## postorder traversal
```python
def postorder_traversal(root: TreeNode) -> [int]:
    """with stack"""
    if root is None:
        return []
    stack = [(root, False)]
    final = []
    while stack:
        node, visited = stack.pop()
        if node:
            if visited:
                final.append(node.val)
            else:
                stack.append((node, True))
                stack.append((node.right, False))
                stack.append((node.left, False))
    return final

def reverse_pre(root: TreeNode) -> [int]:
    """Modified preorder traversal, then reverse the result"""
    if not root:
        return []
    stack = []
    final = []
    cur = root
    while stack or cur:
        while cur:
            final.append(cur.val)
            stack.append(cur)
            cur = cur.right
        cur = stack.pop().left
    final.reverse()
    return final   
        
```

## Traversal Summary

> Only preorder traversal can happen in place, namely forming a new `LinkedList` without using extra space. ???

## unique BST
```python
def gen_tree(start, end) -> [TreeNode]:
    
    if start == end:
        return [TreeNode(start)]

    if start > end:
        return [None]


    final = []
    for i in range(end-start+1):
    
        for left in gen_tree(start, start+i-1):

            for right in gen_tree(start+i+1, end):
                this = TreeNode(start+i)
                this.left = left
                this.right = right
                final.append(this)
    
    return final
```

## permutation
```python
def permutation(n):

    res = [0 for i in range(n+1)]
    res[0] = 1
    for i in range(1, n+1):
        for j in range(i):
            remain = i-1-j
            res[i] += res[j] * res[remain]

    return res[n]
```

