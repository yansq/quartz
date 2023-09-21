# 2PL(two-phase locking)

2PL is a concurrency control method that guarantees serializability.

There are two major types of locks used in 2PL:

- Write-lock(exclusive lock)
- Read-lock(shared lock)

The compatibilities of the two kinds of locks are as follows:

| Lock type | Shared         | Exclusive      |
| --------- | -------------- | -------------- |
| Read(Shared) | Compatible     | Not Compatible |
| Write(Exclusive) | Not Compatible | Not Compatible |

By the 2PL protocol, locks are applied and removed in two phases:

1. Expanding phase: locks are acquired and no locks are released.
2. Shrinking phase: **locks are released and no locks are acquired.**

> [!tip] Pay attention to the second phase, when a transaction releases a lock, it can't acquire neither Shared Locks nor Exclusive Locks anymore.

This rule ensures the serializability property between conflict transactions. 




