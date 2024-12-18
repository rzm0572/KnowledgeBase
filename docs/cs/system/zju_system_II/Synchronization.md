# Synchronization

Race Condition

对于无法并行的操作，如果两个或多个线程同时访问同一个共享资源，可能会导致数据不一致的问题。包含这种操作的区域称为 Critical Section.

对于单核 CPU，可以通过将 Interruption 关掉来解决，但对于多核 CPU，仍然会出现 Race Condition。

解决方法：加锁
