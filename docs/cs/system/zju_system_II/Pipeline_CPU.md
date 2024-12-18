# Pipeline CPU

## Data Hazard

### Forwarding

### Double Data Hazard

```assembly
add x1, x2, x3
add x1, x1, x4
sub x5, x1, x6
```

运行到 `sub` 指令时，同时存在 `EX` 和 `MEM` 阶段的数据冲突，此时我们只需要来源于 `EX` 阶段的 forwarding result.

### Load-Use Hazard

- ID.MemRead = 1 (load instruction)
- ID.Rd = IF.Rs1 or IF.Rs2

在 IF/ID 阶段处理冲突的原因是为了避免对 forwarding unit 产生影响.

### Stalls and Performance

Stall 会导致性能下降.

解决方案：

- Write-Read Hazard: Forwarding
- Load-Use Hazard: (Code Rearrangement) 编译器可以调换指令顺序，在 Load 指令后插入一条无关指令.

## Control Hazard

branch 指令执行到 `WB` 阶段时可以确定跳转地址，若要跳转，则需要把当前执行到 `ID`，`EX`，`MEM` 阶段的指令 flush 掉. 该过程中会产生三个 bubble.

为减少产生的 bubble 数，我们需要尽早检测分支跳转的行为，可通过在 `ID` 阶段增加加法器和比较器来实现.

Delay Slot（分支延迟槽）

Branch prediction（分支预测）

Branch History Table
