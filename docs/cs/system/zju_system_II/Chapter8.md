---
tags:
  - Theory
---

# OS structure

## System call

使用 sys call table 来实现系统调用.

sys call 随架构的不同而不同.

`strace` 工具可以用来跟踪系统调用.

Openfile Table

Parameter Passing:
- 寄存器传递
- 栈传递

Process management
File management
Device management
Information maintenance
Communication
Protection

File descriptor:

- 0: standard input
- 1: standard output
- 2: standard error

## System service

System programs provide a conventient environmemnt for program development and execution.

## Linking

动态链接的 ELF 文件中有 `.interp` 数据段

静态链接比动态链接可移植性更好.

DYN: elf_entry --> loader_elf_entry
STA: elf_entry --> self_elf_entry
