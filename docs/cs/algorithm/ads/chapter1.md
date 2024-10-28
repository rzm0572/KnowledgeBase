---
tags: 
  - Theory
---

# AVL Trees, Splay Trees, and Amortized Analysis

## AVL Trees

### Why

Target: Speed up operations like search, insertion and deletion

Baseline: Binary Search Trees (BSTs)



### What

Before we start, let's define some concepts:

!!! info "Definition :: Height Balanced"

    1. An empty binary tree is height balanced.
    2. If $T$ is a non-empty binary tree with $T_L$ and $T_R$ as its left and right subtrees, then $T$ is height balanced iff
        1. $T_L$ and $T_R$ are both height balanced binary trees.
        2. The height of $T_L$ and $T_R$ differ by at most 1.

    **Note**: The height of an empty tree is -1.

!!! info "Definition :: Balance Factor"

    Balance Factor: The difference between the heights of the left and right subtrees of a node, i.e. 
    
    $$
        BF(node)=|h_L-h_R|
    $$
    
    Balance Factor quantitatively measure how balanced a binary tree is.

!!! info "Definition :: AVL Tree"

    A self-balancing binary search tree (BST) that maintains Balance Factor of any node not more than 1, i.e. 
    
    $$
        \forall v \in T , |BF(v)|\leq 1
    $$

### How

Tree rotation: A technique on a binary tree that changes the structure without interfering with the order of the elements.

- Effect of rotation: 
- Time complexity: $O(1)$
- Types of rotations:
    1. RR rotation: The trouble maker node is in the right subtree's right subtree of the trouble finder node. We should perform a left rotation on the trouble finder node's right child.
    2. LL rotation: The trouble maker node is in the left subtree's left subtree of the trouble finder node. We should perform a right rotation on the trouble finder node's left child.
    3. LR rotation: The trouble maker node is in the left subtree's right subtree of the trouble finder node. We should first perform a left rotation on the trouble finder node's left child's right child, and then a right rotation on the trouble finder node's left child.
    4. RL rotation: The trouble maker node is in the right subtree's left subtree of the trouble finder node. We should first perform a right rotation on the trouble finder node's right child's left child, and then a left rotation on the trouble finder node's right child.

The height $h$ of an AVL tree is $O(\log n)$, where $n$ is the number of nodes in the tree.

!!! note "Proof"

    Let $n_h$ be the minimum number of nodes in a height balanced tree of height $h$. 

    Assuming that there's an AVL tree $T$ of height $h$, we know that the left and right subtrees of the root of $T$ are both AVL trees, and their heights differ by at most 1. Moreover, at least one of their heights is $h-1$.

    To construct an AVL tree of height $h$ with the least nodes, the height of the left and right subtrees could be respectively $h-1$ and $h-2$. So we have:

    $$
        n_h = n_{h-1} + n_{h-2} + 1
    $$

    **Q.E.D.**

## Splay Trees

### What

Target: Any $M$ consecutive tree operations starting from an empty tree take at most $O(M\log n)$ time.

### How



Deletions:

- Splay $X$ to the root of the tree.
- Delete $X$
- Find the rightmost node in the left subtree of $X$ and splay it to the root

## Amortized Analysis | 摊还分析

To analyze the time complexity of a sequence of operations, we have three types of bounds:

- Worst-case bound
    - Assume that every operation in the sequence takes the worst-case time
    - May not be precisive enough to analyze the algorithm
- Amortized bound
    - Garantee that the total time to perform all operation is in amortized bound
    - Have nothing to do with the probability of operation in the input sequence
    - Provide a good estimate of the algorithm's performance
- Average-case bound
    - Statistical analysis of the average cost of each operation
    - May contain big errors in some cases

Generally, Worst-case bound > Amortized bound > Average-case bound.

Followings are three methods to do amortized analysis:

### Aggregate analysis | 聚合分析

Idea: Show that for all $n$, a sequence of $n$ operations takes worst-case time $T(n)$ in total. In the worst-case, the average cost, or aamortized cost, per operation is $\frac{T(n)}{n}$.

### Accounting method | 核算法

When an operation's amortized cost $\hat{c_i}$ exceeds its actual cost $c_i$, 

Assuming that we have $n$ steps, for all $k$ steps from start we need to maintain:

$$
    \sum\limits_{i=1}^k \hat{c_i} \geq \sum\limits_{i=1}^k c_i
$$

### Potential method | 势能法

$$
    \hat{c_i} - c_i = Credit_i = \Phi(D_i) - \Phi(D_{i-1})
$$


- zig:
    
    $$
    \begin{align*}
    \hat{c_i} &= 1 + R_2(X) - R_1(X) + R_2(P) - R_1(P) \\
              &= 1 + 2(R_2(X) - R_1(X))
    \end{align*}
    $$

- zig-zag:
- zig-zig:
