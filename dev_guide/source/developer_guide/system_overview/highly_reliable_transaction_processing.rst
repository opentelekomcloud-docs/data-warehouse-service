:original_name: dws_04_0011.html

.. _dws_04_0011:

Highly Reliable Transaction Processing
======================================

GaussDB(DWS) manages cluster transactions, the basis of HA and failovers. This ensures speedy fault recovery, guarantees the Atomicity, Consistency, Isolation, Durability (ACID) properties for transactions and after a recovery, and enables concurrent control.

**Fault Rectification**

GaussDB(DWS) provides an HA mechanism to reduce the service interruption time when a cluster is faulty. It protects key user programs to continuously provide external services, minimizing the impact of hardware, software, and human faults on services and ensuring service continuity.

-  Hardware HA: Disk RAID, switch stacking, NIC bond, and uninterruptible power supply (UPS)
-  Software HA: HA mechanism used for instances in the GaussDB(DWS) cluster, such as CNs, GTMs, and DNs)

**Transaction Management**

-  Transaction blocks are supported. You can run **start transaction** to make the startup of a transaction block explicit.
-  Single-statement transactions are supported. If you do not explicitly start a transaction, a single statement is processed as a transaction.
-  Distributed transaction management and global transaction information management are supported. This includes gxid, snapshot, timestamp management, distributed transaction status management, and gxid overflow processing.
-  Distributed transactions have ACID properties.
-  Deadlocks are prevented in the distributed system. A transaction will be unlocked immediately after a deadlock (if any).
