# [623. Add One Row to Tree](https://leetcode.com/problems/add-one-row-to-tree/)


## 题目

Given the root of a binary tree, then value `v` and depth `d`, you need to add a row of nodes with value `v` at the given depth `d`. The root node is at depth 1.

The adding rule is: given a positive integer depth `d`, for each NOT null tree nodes `N` in depth `d-1`, create two tree nodes with value `v` as `N's` left subtree root and right subtree root. And `N's` **original left subtree** should be the left subtree of the new left subtree root, its **original right subtree** should be the right subtree of the new right subtree root. If depth `d` is 1 that means there is no depth d-1 at all, then create a tree node with value **v** as the new root of the whole original tree, and the original tree is the new root's left subtree.

**Example 1:**

```
Input: 
A binary tree as following:
       4
     /   \
    2     6
   / \   / 
  3   1 5   

v = 1d = 2Output: 
       4
      / \
     1   1
    /     \
   2       6
  / \     / 
 3   1   5   
```

**Example 2:**

```
Input: 
A binary tree as following:
      4
     /   
    2    
   / \   
  3   1    

v = 1d = 3Output: 
      4
     /   
    2
   / \    
  1   1
 /     \  
3       1
```

**Note:**

1. The given d is in range [1, maximum depth of the given tree + 1].
2. The given binary tree has at least one tree node.

## 题目大意

给定一个二叉树，根节点为第1层，深度为 1。在其第 d 层追加一行值为 v 的节点。添加规则：给定一个深度值 d （正整数），针对深度为 d-1 层的每一非空节点 N，为 N 创建两个值为 v 的左子树和右子树。将 N 原先的左子树，连接为新节点 v 的左子树；将 N 原先的右子树，连接为新节点 v 的右子树。如果 d 的值为 1，深度 d - 1 不存在，则创建一个新的根节点 v，原先的整棵树将作为 v 的左子树。

## 解题思路

- 这一题虽然是 Medium，实际非常简单。给二叉树添加一行，用 DFS 或者 BFS，遍历过程中记录行数，到达目标行一行，增加节点即可。不过需要注意 2 个特殊情况，特殊情况一，`d==1`，此时需要添加的行即为根节点。特殊情况二，`d>height(root)`，即要添加的行数比树还要高，这时只需要在最下层的叶子节点添加一层。时间复杂度 O(n)，空间复杂度 O(n)。

## 代码

```go
package leetcode

import (
	"github.com/halfrost/leetcode-go/structures"
)

// TreeNode define
type TreeNode = structures.TreeNode

/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func addOneRow(root *TreeNode, v int, d int) *TreeNode {
	if d == 1 {
		tmp := &TreeNode{Val: v, Left: root, Right: nil}
		return tmp
	}
	level := 1
	addTreeRow(root, v, d, &level)
	return root
}

func addTreeRow(root *TreeNode, v, d int, currLevel *int) {
	if *currLevel == d-1 {
		root.Left = &TreeNode{Val: v, Left: root.Left, Right: nil}
		root.Right = &TreeNode{Val: v, Left: nil, Right: root.Right}
		return
	}
	*currLevel++
	if root.Left != nil {
		addTreeRow(root.Left, v, d, currLevel)
	}
	if root.Right != nil {
		addTreeRow(root.Right, v, d, currLevel)
	}
	*currLevel--
}
```