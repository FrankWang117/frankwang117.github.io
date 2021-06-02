---
layout: post
title: LeetCode Offer 07. 重建二叉树
categories: [LeetCode]
description: 重建二叉树
keywords: LeetCode, Offer, JavaScript, Binary tree
---

# Offer 07. 重建二叉树

## 题目[📓](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof)

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

例如，给出

```markdown
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：
```

```js
   3
  / \
 9   20
     / \
    15  7
```

限制：

> 0 <= 节点个数 <= 5000

## 题解

重建二叉树，首先是要了解二叉树的遍历特点：

- 前序遍历： [根节点] -> [左子树] -> [右子树]
- 中序遍历：[左子树] -> [根节点] -> [右子树]
- 后序遍历：[左子树] -> [右子树] -> [根节点]

**注意**根结点的位置区别；

知道了三种遍历的区别，那就可以针对前序遍历与中序遍历的不同寻找切入点：

从前序遍历可以得知，第一个元素为根节点。而从中序遍历得知，根节点的位置左侧为左子树，右侧为右子树。而通过中序遍历获取到的左右子树的个数，又可以在根节点的前序遍历中获取左右子树的前序遍历：

## 解答

```typescript
/**
 * Definition for a binary tree node.
 * class TreeNode {
 *     val: number
 *     left: TreeNode | null
 *     right: TreeNode | null
 *     constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.left = (left===undefined ? null : left)
 *         this.right = (right===undefined ? null : right)
 *     }
 * }
 */

function buildTree(preorder: number[], inorder: number[]): TreeNode | null {
  	if(!Array.isArray(perorder) ||perorder.length === 0) {
      return null
    }
    const root = new TreeNode(preorder[0]);
    const rootInInorderIndex = inorder.indexOf(preorder[0]);
    const leftInorder = inorder.slice(0,rootInInorderIndex);
    const rightInorder = inorder.slice(rootInInorderIndex+1);
    root.left = buildTree(preorder.slice(1,leftInorder.length + 1),leftInorder)
    root.right = buildTree(perorder.slice(leftInorder.length+1),rightInorder)
    return root
}
```

## 解析

见代码.

## 想法

熟能生巧。唯有多练习。
