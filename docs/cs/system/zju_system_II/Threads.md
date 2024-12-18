---
tags:
  - Theory
---

# Threads

## Advantages of Threads

!!! advice "Advantages of Threads"
    - Responsiveness | 响应性
    - Scalability | 可扩展性：可以更高效地利用系统资源

!!! not-advice "Drawbacks of Threads"
    - 线程间没有隔离

User threads and kernel threads:

- Many-to-One model
- One-to-One model
- Many-to-Many model
- Two-Level model

## Issues with Threads

### Fork and exec

### Signals

### Cancellation

- Asynchronous cancellation
- Deferred cancellation: Cancellation only occurs when thread reacheds cancellation point.

进程信息存储在 Leader 进程的 TCB 中.

Kernel Stack 一般 16 KB (4 Pages)
