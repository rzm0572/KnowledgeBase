---
tags:
    - theory
---

# Database design using E-R model

!!! tip "cheatsheet"

    === "page 1"

        ![pEyC7Lj.png](https://s21.ax1x.com/2025/03/31/pEyC7Lj.png)
    
    === "page 2"

        ![pEyCqwn.png](https://s21.ax1x.com/2025/03/31/pEyCqwn.png)

## Design Progress

### Design Phases

- Initial phase: 完整地刻画数据库用户的数据需求
- Second phase (conceptual-design phase): 选择数据模型（Data Model）
    - 将需求转化为数据库的概念模式（Conceptual Schema）
    - 概念设计阶段会构建实体 - 联系图（Entity-Relationship Diagram，E-R Diagram），提供对模型的图形化表述
    - 完整的概念模式中还需指明功能需求，定义在数据库上可以进行的操作
- Final phase：将抽象的数据模型转化为数据库实现
    - 逻辑设计：将概念模式映射到数据库系统的实现数据模型上，通常是由实体 - 联系模型映射到关系数据模型
    - 物理设计：指明数据库的物理特征，如文件组织格式和索引结构的选择

### Design Alternatives

!!! tip "Avoid pitfalls"
    - 冗余（Redundancy）：在设计数据库时，应避免创建冗余数据，即在多个表中存储相同的数据.
    - 不完整（Incompleteness）：在设计数据库时，应避免创建不完整的数据，尽量避免表中出现空值.

## E-R Model

实体 - 联系模型（Entity-Relationship Model，E-R Model）包括三个基本概念：实体集（Entity Set）、联系集（Relationship Set）和属性（Attributes）.

### Entity Set

!!! definition "Entity"

    实体（Entity）是现实世界中可区别于所有其他对象的一个对象，可通过属性集来描述，每个属性有一个值（Value）.

!!! definition "Entity Set"

    实体集（Entity Set）是具有相同属性的一个实体集合.

    实体集的图示：

    ![pEypFTP.png](https://s21.ax1x.com/2025/03/31/pEypFTP.png)

- 实体集的外延（Extentsion）指实际属于该实体集的所有实体的集合.
- 实体集的属性的一个子集构成实体集的主键（Primary Key），主键是唯一标识实体集中每个实体的属性集.


### Relationship Set

!!! definition "Relationship"

    联系（Relationship）指多个实体之间的相互关联（Association）.

!!! definition "Relationship Set"

    联系集（Relationship Set）是相同类型联系的集合，可以具有描述性属性（Descriptive Attributes）.

    联系集的图示：

    === "不带属性"

        ![pEypAFf.png](https://s21.ax1x.com/2025/03/31/pEypAFf.png)

    === "带属性"

        ![pEypEY8.png](https://s21.ax1x.com/2025/03/31/pEypEY8.png)

具体地，假设 $E_1, E_2, \ldots, E_n$ 为实体集，则定义在 $E_1, E_2, \ldots, E_n$ 上的联系集 $R$ 为

$$
    \left\{ (e_1, e_2, \ldots, e_n) \mid e_i \in E_i, i = 1, 2, \ldots, n \right\}
$$

的一个子集，而 $(e_1, e_2, \ldots, e_n)$ 是一个联系.

在上面的例子中，我们称实体集 $E_1, E_2, \ldots, E_n$ 参与（Participate）联系集 $R$.

!!! definition "role"
    实体在联系中扮演的功能被称为实体的角色（Role），在参与的实体集互异的联系集中，角色通常无需指定，而在自环（Recursive）联系集中，我们需要指定角色.

    ![pEypnyj.png](https://s21.ax1x.com/2025/03/31/pEypnyj.png)

!!! definition "degree of relationship set"
    联系集的度（Degree）是指参与的实体集的个数，在上面的例子中， $R$ 的度为 $n$.

    多元联系集的图示：

    ![pEypw01.png](https://s21.ax1x.com/2025/03/31/pEypw01.png)

    多元联系集可以通过引入一个新实体集，并建立新实体集与参与实体集之间的二元关系来代替：

    ![pEgedC4.png](https://s21.ax1x.com/2025/04/08/pEgedC4.png)

### Attributes

属性可取指的集合被称为属性的域（Domain）或值集（Value Set）.

- 简单（Simple）属性和复合（Composite）属性
    - 复合属性可以被划分为更简单的属性
- 单值（Single-Valued）属性和多值（Multi-Valued）属性
    - 多值属性指一个属性可以对应一组值
    - 使用花括号将属性名括起来表示多值属性
- 派生（Derived）属性
    - 可以由其他属性、实体、关系中派生出来的属性
    - 无需存储

!!! example
    ![pEyp6pD.png](https://s21.ax1x.com/2025/03/31/pEyp6pD.png)

    - `name` 是复合属性
    - `phone_number` 是多值属性
    - `age` 是派生属性，可由 `birth_date` 计算得出

!!! note "Redundant Attributes"
    冗余属性指在多个实体集中存储相同的数据，会导致数据冗余，降低数据一致性. 我们应该通过在实体集之间建立联系集的方法来消除冗余属性.

### Constraints

#### Mapping Cardinality Constraints

!!! definition "Mapping Cardinality"
    映射基数（Mapping Cardinality）表示一个实体通过一个联系集能关联的实体的个数.

对于建立在实体集 $A$ 与 $B$ 上的二元联系集 $R$ 来说，映射基数有四种情况：

=== "一对一"

    $A$ 中的一个实体至多与 $B$ 中的一个实体关联，反之亦然.

    ![pEypf0I.png](https://s21.ax1x.com/2025/03/31/pEypf0I.png)

=== "一对多"

    $A$ 中的一个实体可以与 $B$ 中的多个实体关联，$B$ 中的一个实体至多与 $A$ 中的一个实体关联.

    ![pEyph7t.png](https://s21.ax1x.com/2025/03/31/pEyph7t.png)

=== "多对一"

    $B$ 中的一个实体可以与 $A$ 中的多个实体关联，$A$ 中的一个实体至多与 $B$ 中的一个实体关联.

=== "多对多"

    $A$ 中的一个实体可以与 $B$ 中的多个实体关联，反之亦然.

#### Participation Constraints

- 全部参与（Totally participating）：实体集中的所有实体都必须出现在联系集中的某个联系中

    - 在 E-R 图中使用双线表示

- 部分参与（Partially participating）

!!! note "Notations"
    我们可以在连线上用形如 $a..b$ 的记号来表示映射基数约束和参与约束：

    - $0..*$ 表示无约束
    - $0..1$ 表示这一侧的实体集中的实体最多参与到一个联系中
    - $1..*$ 表示全部参与约束

    ![pEyCoQg.png](https://s21.ax1x.com/2025/03/31/pEyCoQg.png)

#### Primary Key

主键定义了在实体集中唯一确定实体、在联系集中唯一确定联系的方法.

##### Primary key of an entity set

##### Primary key of a relationship set

假设 $R$ 是定义在 $E_1, E_2, \ldots, E_n$ 上的联系集，则属性集合

$$
    \text{primary_key}(E_1) \cup \text{primary_key}(E_2) \cup \cdots \cup \text{primary_key}(E_n)
$$

构成了联系集的一个超键（Superkey）.

主键应选取超键的最小子集，以便在关系数据库中实现快速查找. 对二元关系来说，

- 一对一关系：选择任意一个参与联系集的实体集的主键即可
- 一对多关系 / 多对一关系：选择“多”侧的实体集的主键
- 多对多关系：选择两个参与联系集的实体集的主键的并集

##### Weak Entity sets

没有足够的属性以形成主键的实体集被称为弱实体集（Weak Entity Set），有主键的实体集被称为强实体集（Strong Entity Set）.

弱实体集采取与另一个被称作标识实体集（Identifying Entity Set）或属主实体集（Owner Entity Set）的实体集相关联的方式来形成主键.

- 称弱实体集存在依赖（Existence dependent）于标识实体集
- 称标识实体集拥有（Own）它所标识的弱实体集
- 弱实体集与标识实体集之间的关系被称为标识性联系（Identifying Relationship）
    - 标识性联系是多对一的（弱实体集 - 标识实体集）

- 标识实体集中的某个实体对应弱实体集中的若干个实体，这些实体之间通过分辨符（Discriminator）进行区分. 分辨符在 E-R 图中用虚线表示.
- 弱实体集的主键由标识实体集中的主键和弱实体集中的分辨符组成.

![pEy9IbR.png](https://s21.ax1x.com/2025/03/31/pEy9IbR.png)

!!! tip "弱实体集的判定条件"
    如果一个实体集中的实体完全依赖于与另一个实体集之间的关系而存在，则可以使用弱实体集来表示该实体集.

!!! note "引入弱实体集的原因"
    TODO


### Inheritance

#### Specialization

实体集可能包括某些具有相同属性的子集. 在实体集内进行分组并派生出新的实体集的过程被称为特化（Specialization）.

特化过程中，子集会继承原实体集的属性，并增加原实体集中没有的属性，这被称为属性继承（Attribute inheritance）.

![pEyCZrj.png](https://s21.ax1x.com/2025/03/31/pEyCZrj.png)

上图中，空心箭头表示 ISA (is a) 关系，用于描述特化.

=== "Overlapping（重叠特化）"

    若原实体集中的某个实体可以同时属于两个或更多的特化实体集，则称这样的特化为重叠特化.

    上图中从 `person` 中派生出 `employee` 与 `student` 的过程为重叠特化.

=== "Disjoint（不相交特化）"

    若原实体集中的某个实体只能属于一个特化实体集，则称这样的特化为不相交特化.

    上图中从 `employee` 中派生出 `instructor` 与 `sectorary` 的过程为不相交特化.

#### Generalization

概化（Generalization）是特化的逆过程，通过寻找多个实体集的共同属性，将多个实体集合并为一个实体集.

!!! note "Generalization and Specialization"
    - 特化是自顶向下的过程，从高层实体集（超类）开始分组，得到低层实体集（子类）
    - 概化是自底向上的过程，从低层实体集（子类）开始合并，得到高层实体集（超类）

!!! note "Constraints on Generalization"
    TODO

### Aggregation

![pEyCISS.png](https://s21.ax1x.com/2025/03/31/pEyCISS.png)

聚类（Aggregation）将联系视为抽象的高层实体集，建立联系集与实体集之间的联系.

聚类的关系模型实现：将联系集的主键、实体集的主键和描述性属性作为聚类关系的属性.

## Reduction to Relational Schema

实体集和联系集都可以被统一地表示为数据库中的关系模式（Relational Schema）.

### Entity Sets

#### Strong Entity Sets

将强实体集转化为关系模式只需要将<strong>强实体集的属性</strong>作为关系模式的属性即可，主键即为强实体集的主键.

对于复杂属性：

- 复合属性：分解为多个简单属性
- 多值属性：创建新的关系模式，包括强实体集的主键和多值属性

!!! example
    对于 `instructor` 实体集的多值属性 `phone_number`，我们可以建立关系模式 `instructor_phone_number`：

    $$
        instructor\_phone\_number(\underline{ID}, \underline{phone\_number}).
    $$


#### Weak Entity Sets

将弱实体集转化为关系模式需要将<strong>弱实体集的属性</strong>和<strong>标识实体集的主键</strong>作为关系模式的属性，主键由<strong>标识实体集的主键</strong>与<strong>弱实体集的分辨符</strong>组成.

除此之外，还需要建立从弱实体集对应的关系模式到标识实体集对应的关系模式的外键约束，采用级联删除模式.

### Relationship Sets

假设 $R$ 为建立在 $E_1, E_2, \ldots, E_n$ 上的联系集，具有描述性属性 $b_1, b_2, \ldots, b_m$，则其对应的关系模式的属性集合为

$$
    \text{primary_key}(E_1) \cup \text{primary_key}(E_2) \cup \cdots \cup \text{primary_key}(E_n) \cup \{b_1, b_2, \ldots, b_m\}.
$$

二元联系集对应的关系模式的主键选取在 [Primary Key](./#primary-key) 一节中已说明，在此不再赘述.

对于多元联系集，关系模式的主键的选取遵循以下规则：

- 对于没有箭头的 $n$ 元联系集，其主键由 $n$ 个实体集的主键的并集构成.
- 对于有箭头的 $n$ 元联系集，不在箭头侧的实体集的主键属性作为关系模式的主键.

联系集转化为关系模式时还需要建立外键约束：$R$ 中来自 $E_i$ 主键的属性需要参照关系模式 $E_i$ 的主键.

#### Redundancy of Schemas

弱实体集与强实体集之间的标示性联系，满足以下特性：

- 多对一
- 无描述性属性
- 弱联系集的主键包含强实体集的主键

对于这种情况，我们不需要为联系集单独创建一个关系模式，因为这样的关系模式的所有信息都可以从弱实体集的关系模式中获得，故是冗余的.

#### Combination of Schemas

对于满足以下要求的实体集 $A$ 和 $B$，以及建立在 $A$ 与 $B$ 上的联系集 $R$：

- $A$ 相对 $B$ 具有多对一 / 一对一关系
- $A$ 中的所有实体都参与到 $R$ 中

则我们可以将联系集 $R$ 和实体集 $A$ 合并成一个关系模式，其属性集为 $A$ 的属性集合和 $R$ 的属性集合的并集，主键为 $A$ 的主键.
