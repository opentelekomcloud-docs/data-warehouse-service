:original_name: dws_04_0826.html

.. _dws_04_0826:

PGXC_THREAD_WAIT_STATUS
=======================

**PGXC_THREAD_WAIT_STATUS** displays all the call layer hierarchy relationship between threads of the SQL statements on all the nodes in a cluster, and the waiting status of the block for each thread, so that you can easily locate the causes of process response failures and similar phenomena.

The definitions of **PGXC_THREAD_WAIT_STATUS** view and **PG_THREAD_WAIT_STATUS** view are the same, because the essence of the **PGXC_THREAD_WAIT_STATUS** view is the query summary result of the **PG_THREAD_WAIT_STATUS** view on each node in the cluster.

.. table:: **Table 1** PGXC_THREAD_WAIT_STATUS columns

   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name        | Type    | Description                                                                                                                                                                                                                               |
   +=============+=========+===========================================================================================================================================================================================================================================+
   | node_name   | text    | Current node name                                                                                                                                                                                                                         |
   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_name     | text    | Database name                                                                                                                                                                                                                             |
   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | thread_name | text    | Thread name                                                                                                                                                                                                                               |
   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | query_id    | bigint  | Query ID. It is equivalent to **debug_query_id**.                                                                                                                                                                                         |
   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tid         | bigint  | Thread ID of the current thread                                                                                                                                                                                                           |
   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | lwtid       | integer | Lightweight thread ID of the current thread                                                                                                                                                                                               |
   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ptid        | integer | Parent thread of the streaming thread                                                                                                                                                                                                     |
   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | tlevel      | integer | Level of the streaming thread                                                                                                                                                                                                             |
   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | smpid       | integer | Concurrent thread ID                                                                                                                                                                                                                      |
   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | wait_status | text    | Waiting status of the current thread. For details about the waiting status, see :ref:`Table 2 <en-us_topic_0000001460562976__t2e88141821ac42158405ff5eea1d7285>`.                                                                         |
   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | wait_event  | text    | If **wait_status** is **acquire lock**, **acquire lwlock**, or **wait io**, this column describes the lock, lightweight lock, and I/O information, respectively. If **wait_status** is not any of the three values, this column is empty. |
   +-------------+---------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example:

Assume you run a statement on coordinator1, and no response is returned after a long period of time. In this case, establish another connection to coordinator1 to check the thread status on it.

::

    select * from pg_thread_wait_status where query_id > 0;
     node_name   | db_name  | thread_name  | query_id |       tid       | lwtid | ptid  | tlevel | smpid |     wait_status   |   wait_event
   --------------+----------+--------------+----------+-----------------+-------+-------+--------+-------+----------------------
    coordinator1 | gaussdb | gsql         | 20971544 | 140274089064208 | 22579 |       |      0 |     0 | wait node: datanode4 |
   (1 rows)

Furthermore, you can view the statement working status on each node in the entire cluster. In the following example, no DNs have threads blocked, and there is a huge amount of data to be read, causing slow execution.

::

   select * from pgxc_thread_wait_status where query_id=20971544;
     node_name   | db_name  | thread_name  | query_id |       tid       | lwtid | ptid  | tlevel | smpid |     wait_status   |  wait_event
   --------------+----------+--------------+----------+-----------------+-------+-------+--------+-------+----------------------
    datanode1    | gaussdb | coordinator1 | 20971544 | 139902867994384 | 22735 |       |      0 |     0 | wait node: datanode3 |
    datanode1    | gaussdb | coordinator1 | 20971544 | 139902838634256 | 22970 | 22735 |      5 |     0 | synchronize quit     |
    datanode1    | gaussdb | coordinator1 | 20971544 | 139902607947536 | 22972 | 22735 |      5 |     1 | synchronize quit     |
    datanode2    | gaussdb | coordinator1 | 20971544 | 140632156796688 | 22736 |       |      0 |     0 | wait node: datanode3 |
    datanode2    | gaussdb | coordinator1 | 20971544 | 140632030967568 | 22974 | 22736 |      5 |     0 | synchronize quit     |
    datanode2    | gaussdb | coordinator1 | 20971544 | 140632081299216 | 22975 | 22736 |      5 |     1 | synchronize quit     |
    datanode3    | gaussdb | coordinator1 | 20971544 | 140323627988752 | 22737 |       |      0 |     0 | wait node: datanode3 |
    datanode3    | gaussdb | coordinator1 | 20971544 | 140323523131152 | 22976 | 22737 |      5 |     0 | net flush data       |
    datanode3    | gaussdb | coordinator1 | 20971544 | 140323548296976 | 22978 | 22737 |      5 |     1 | net flush data
    datanode4    | gaussdb | coordinator1 | 20971544 | 140103024375568 | 22738 |       |      0 |     0 | wait node: datanode3
    datanode4    | gaussdb | coordinator1 | 20971544 | 140102919517968 | 22979 | 22738 |      5 |     0 | synchronize quit     |
    datanode4    | gaussdb | coordinator1 | 20971544 | 140102969849616 | 22980 | 22738 |      5 |     1 | synchronize quit     |
    coordinator1 | gaussdb | gsql         | 20971544 | 140274089064208 | 22579 |       |      0 |     0 | wait node: datanode4  |
   (13 rows)
