---
tags:
    - Theory
---

# Relation Model

## Structures

- Relation: Table
- Tuple: Row
- Attribute: Column
- Relation schema: 关系模式
- Relation instance: 关系实例

!!! definition "Relation Schema"
    若 $A_1, A_2, \ldots, A_n$ 为一系列属性，则可称 $R=(A_1, A_2, \ldots, A_n)$ 为一个关系模式（Relation Schema）

定义在关系模式 $R$ 上的关系实例 $r$ 可被记为 $r(R)$.

### Attributes

- 某个属性可接受的值组成的集合称为该属性的域（domain）

- 关系中某个属性的值需要是原子的（atomic）

- 


### Database Schema

- 数据库模式（Database Schema）：数据库的逻辑设计
- 数据库实例（Database Instance）：数据库中的数据在给定时刻下的快照（snapshot）

### Key

键（Key）是属性的子集

- 超键（Superkey）：键 $K$ 被称为关系模式 $R$ 的超键，如果 $K$ 的值能唯一确定关系模式 $R$ 中的任意一个元组.
- 候选键（Candidate Key）：$K$ 被称为关系模式 $R$ 的候选键，如果 $K$ 是 $R$ 的超键，且 $K$ 的任何真子集都不是超键.
- 主键（Primary Key）：设计数据库时，从候选键中选取一个作为主键，主键应选取值唯一且从不（极少）变化的属性.
- 外键（Foreign Key）：关系模式 $R_1$ 中的属性 $A_1$ 被称为参照关系模式 $R_2$ 的外键，如果 $A_1$ 的域是 $R_2$ 的主键的域的子集.
    - $R_1$ 被称为外键的参照关系（Referencing Relation）
    - $R_2$ 被称为外键的被参照关系（Referenced Relation）

## Query Language

- 过程化语言（Procedural Language）
- 非过程化语言（Nonprocedural Language）

### Relational Algebra

关系代数（Relational Algebra）是一种过程化语言，包含以下基本算子：

- 选择（Selection）：$\sigma$

    选择算子 $\sigma_p(r)$ 可从关系 $r$ 中选出满足谓词 $p$ 的元组

- 投影（Projection）：$\Pi$

    投影算子 $\Pi_{A_1, A_2, \ldots, A_k}(r)$ 可从输入关系 $r$ 中提取出指定的属性集 $A_1, A_2, \ldots, A_k$，并完成去重操作（相当于降维）

- 笛卡尔积（Cartesian Product）：$\times$

    笛卡尔积算子 $R_1 \times R_2$ 将 $R_1$ 与 $R_2$ 中的元组拼接成新的元组，任意 $R_1$ 中的元组都将与 $R_2$ 中的所有元组进行组合

- 并（Union）：$\cup$

    并算子 $R_1 \cup R_2$ 输出 $R_1$ 和 $R_2$ 中的元组的并集

- 差（Difference）：$\setminus$

    差算子 $R_1 \setminus R_2$ 输出 $R_1$ 中不在 $R_2$ 中的元组

- 重命名（Rename）：$\rho$

    $\rho_i(r)$ 可将关系 $r$ 重命名为 $i$

以下算子可以通过上述基本算子组合而成：

- 连接（Join）：$\bowtie$

    连接算子 $r \bowtie_\theta s$ 将 $r$ 和 $s$ 按照连接条件 $\theta$ 进行连接，连接结果为一个新的关系

    连接算子可使用笛卡尔积算子和选择算子来表示：$r \bowtie_\theta s = \sigma_{\theta}(r \times s)$

- 赋值（Assignment）：$\leftarrow$

    将中间结果赋给一个变量，如 $x \leftarrow r \bowtie_\theta s$

- 交（Intersection）：$\cap$

    交算子 $R_1 \cap R_2$ 输出 $R_1$ 和 $R_2$ 中的元组的交集

    要求：
    
    1. $R_1$ 和 $R_2$ 必须有相同的属性集
    2. $R_1$ 和 $R_2$ 中的任意属性是兼容的

!!! tip
    数学上等价的关系代数表达式有可能有很大的性能差异.
