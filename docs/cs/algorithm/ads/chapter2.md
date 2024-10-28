# Chapter 2 Red-Black Tree and B+ Tree

## Red-Black Tree

!!! info "Definition :: Red-Black Tree"
    一颗二叉搜索树若满足以下五条性质，则其被称为红黑树（Red-Black Tree）:

    1. 每个节点或者是红色，或者是黑色
    2. 根节点是黑色的
    3. 每个叶子节点（NIL）是黑色的
    4. 如果一个节点是红色的，则它的两个子节点都是黑色的
    5. 从任意节点到其每个叶子节点的所有路径上包含相同数目的黑色节点

!!! tip "NIL Node"
    在红黑树中，哨兵节点（NIL Node）是一种特殊的节点，不存储数据，只用于作为红黑树的叶子节点，其颜色为黑色。


!!! info "Definition :: black-height"
