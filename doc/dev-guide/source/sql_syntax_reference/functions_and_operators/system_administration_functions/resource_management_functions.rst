:original_name: dws_06_0063.html

.. _dws_06_0063:

Resource Management Functions
=============================

This section describes the functions of the resource management module.

gs_switch_respool(query_id bigint, resource_pool_name name)
-----------------------------------------------------------

Description: Job resource pool switchover function is supported by cluster versions 9.1.0 and later.

The value of **resource_pool_name** must contain less than 64 characters. Upon calling this function, the system will decide whether to start a job in the queuing phase when switching to the new resource pool. If a job is running, the system will switch to the new resource pool to start the queuing job from the original resource pool. Additionally, the CPU control of the new resource pool will limit the queuing job.

This function supports resource pool switchover in both static and dynamic management and control modes, as well as resource pool switchover using short queries.

Return type: bool

.. caution::

   -  Only administrators or users with administrator permissions can use this function. Common users are not authorized to use it. To use this function, the cluster's CPU control function, specifically the **Cgroup** function, must be functioning normally. You can use the **pgxc_cgroup_reload_conf()** function to check whether **Cgroup** is normal. If the command output contains non-success information, rectify the fault.

   -  After the resource pool is switched, the function performs management and control based on the exception rules of the new resource pool.

   -  If a job is in the global concurrent queue, which means it is waiting in the global queue, it will not take effect right away after the resource pool is switched. At this point, the job has not entered the resource pool queue yet.

   -  1. Job CPU control switchover supports CNs and DNs. For example, if resource pool A uses 40 cores and resource pool B uses 5 cores, switching a job from pool A to pool B will execute the job on the 5 cores of pool B. This results in slower execution but isolates the job, ensuring that jobs in pool A are unaffected, thus degrading the statement.

   -  When switching resource pools, use similar statements to perform batch switchovers. Ensure that the jobs being switched are in the same state, such as all queuing or all running. If switching between different types of statements, it is recommended to switch them twice. Switching them together may exceed the concurrent statement limit, causing abnormal job wake-ups.

      ::

         SELECT gs_switch_respool (queryid,'xxx') FROM xxx where xxx;

   -  The restrictions on switching the statement resource pool are as follows:

      -  If jobs from resource pool A are switched to pool B, jobs at the head of the queue in pool A will be woken up. If the concurrent job limit in pool B is reached, job A1 will not run immediately.
      -  Switch job A2 from queuing in pool A to pool B. If the concurrent job limit in pool B is reached, job A2 will continue to queue. If not, job A2 will wake up and run immediately.
      -  During resource pool switchover, only the same lane can be switched. For example, switching the fast lane of pool A will switch the fast lane of pool B. If the short query acceleration function is not enabled in pool B, the job runs directly regardless of its status in pool B.

   -  This function can be used only when all nodes are normal. If a node is faulty, the function fails to be executed.

The following is an example:

While executing the statement with ID **74590868828368101**, the administrator successfully switches it to the **poolg1** resource pool.

::

   CALL gs_switch_respool(74590868828368101,'poolg1');
    gs_switch_respool
   -------------------
    t
   (1 row)

gs_increase_except_num(unique_sql_id int8)
------------------------------------------

Description: Records job exception information. The input parameter must be greater than 0. If this function is invoked, the number of job exceptions will be increased by 1 and the latest job exception time will be updated to the current time.

Return type: bool

Example:

::

   SELECT gs_increase_except_num(111);
   gs_increase_except_num
   ------------------------
   t
   (1 row)

gs_increase_except_num(unique_sql_id int8, except_num int4)
-----------------------------------------------------------

Description: Records job exception information. The input parameter must be greater than 0. If this function is invoked, the number of job exceptions will be increased by **except_num** and the latest job exception time will be updated to the current time.

Return type: bool

Example:

::

   SELECT gs_increase_except_num(111, 4);
   gs_increase_except_num
   -----------------------
   t
   (1 row)

gs_increase_except_num(unique_sql_id int8, except_num int4, except_time int8)
-----------------------------------------------------------------------------

Description: Records job exception information. The input parameter must be greater than 0. If this function is invoked, the number of job exceptions will be increased by **except_num** and the latest job exception time will be updated to **except_time**, a timestamp. This function is mainly invoked internally.

Return type: bool

Example:

::

   SELECT gs_increase_except_num(111, 4, 714623414421256);
   gs_increase_except_num
   -----------------------
   t
   (1 row)

gs_append_blocklist(unique_sql_id int8)
---------------------------------------

Description: Adds a job to the blocklist and update the blocklist information in **GS_BLOCKLIST_QUERY**.

Return type: bool

Example:

::

   SELECT gs_append_blocklist(111);
   gs_append_blocklist
   --------------------
   t
   (1 row)

gs_remove_blocklist(unique_sql_id int8)
---------------------------------------

Description: Removes a job from the blocklist. and update the blocklist information in **GS_BLOCKLIST_QUERY**.

Return type: bool

Example:

::

   SELECT gs_remove_blocklist(111);
   gs_append_blocklist
   --------------------
   t
   (1 row)

gs_increase_sql_except_num(sql_hash text)
-----------------------------------------

Description: Records job exception information. If this function is invoked, the number of job exceptions will be increased by 1 and the latest job exception time will be updated to the current time. This function is supported only by 9.1.0.200 and later cluster versions and is used for internal calling.

Return type: bool

The following is an example:

::

   SELECT gs_increase_sql_except_num('sql_7bf6381d2a5a456a1936027198ad8b12');
   gs_increase_sql_except_num
   ------------------------
   t
   (1 row)

gs_increase_sql_except_num(sql_hash text, except_num int4)
----------------------------------------------------------

Description: Records job exception information. If this function is invoked, the number of job exceptions will be increased by **except_num** and the latest job exception time will be updated to the current time. This function is supported only by 9.1.0.200 and later cluster versions and is used for internal calling.

Return type: bool

The following is an example:

::

   SELECT gs_increase_sql_except_num('sql_7bf6381d2a5a456a1936027198ad8b12', 4);
   gs_increase_sql_except_num
   -----------------------
   t
   (1 row)

gs_increase_sql_except_num(sql_hash text, except_num int4, except_time int8)
----------------------------------------------------------------------------

Description: Records job exception information. If this function is invoked, the number of job exceptions will be increased by **except_num** and the latest job exception time will be updated to **except_time**. **except_time** functions as a timestamp here. This function is supported only by 9.1.0.200 and later cluster versions and is used for internal calling.

Return type: bool

The following is an example:

::

   SELECT gs_increase_sql_except_num('sql_7bf6381d2a5a456a1936027198ad8b12', 4, 714623414421256);
   gs_increase_sql_except_num
   -----------------------
   t
   (1 row)

gs_append_blocklist(sql_hash text)
----------------------------------

Description: Adds a job to the blocklist and updates the blocklist information in **GS_BLOCKLIST_SQL**. This function is supported only by clusters of version 9.1.0.200 or later.

Return type: bool

The following is an example:

::

   SELECT gs_append_blocklist('sql_7bf6381d2a5a456a1936027198ad8b12');
   gs_append_blocklist
   --------------------
   t
   (1 row)

gs_remove_blocklist(sql_hash text)
----------------------------------

Description: Removes a job from the blocklist and updates the blocklist information in **GS_BLOCKLIST_SQL**. This function is supported only by clusters of version 9.1.0.200 or later.

Return type: bool

The following is an example:

::

   SELECT gs_remove_blocklist('sql_7bf6381d2a5a456a1936027198ad8b12');
   gs_remove_blocklist
   --------------------
   t
   (1 row)

gs_wlm_rebuild_except_rule_hash()
---------------------------------

Description: Rebuilds the memory hash table of exception rules on the current node. The monitoring thread obtains the exception rule thresholds from the memory hash table in real time. When the hash table is abnormal, this function can be used to rebuild the table.

Return type: bool

Example:

::

   SELECT gs_wlm_rebuild_except_rule_hash();
   gs_wlm_rebuild_except_rule_hash
   --------------------
   t
   (1 row)

gs_wlm_readjust_user_space(oid)
-------------------------------

Description: This function calibrates the permanent storage space of a user. The input parameter is the user OID. If the input parameter is set to **0**, the permanent storage space of all users is calibrated.

Return type: text

Example:

::

   SELECT gs_wlm_readjust_user_space(0);
   gs_wlm_readjust_user_space
   ----------------------------
   Exec Success
   (1 row)

pgxc_wlm_readjust_schema_space()
--------------------------------

Description: This function calibrates the permanent storage space of a schema.

Return type: text

Example:

::

   SELECT pgxc_wlm_readjust_schema_space();
   pgxc_wlm_readjust_schema_space
   --------------------------------
   Exec Success
   (1 row)

pgxc_wlm_readjust_relfilenode_size_table()
------------------------------------------

Description: Space statistics calibration function. It does not recreate the **PG_RELFILENODE_SIZE** system catalog but recalibrates the user and schema space.

.. note::

   Due to transaction isolation between the calibration function and other services, the calibration function is invisible to other services that are being executed. As a result, the calibration function does not involve space changes of such services. To avoid space difference errors after calibration, you are advised to use the space calibration function to perform calibration when the space is stable.

Return type: text

Example:

::

   SELECT pgxc_wlm_readjust_schema_space();
   pgxc_wlm_readjust_relfilenode_size_table
   -----------------------------------------
   Exec Success
   (1 row)

pgxc_wlm_readjust_relfilenode_size_table(integer)
-------------------------------------------------

Description: Space statistics calibration function

.. note::

   Due to transaction isolation between the calibration function and other services, the calibration function is invisible to other services that are being executed. As a result, the calibration function does not involve space changes of such services. To avoid space difference errors after calibration, you are advised to use the space calibration function to perform calibration when the space is stable.

Parameter: The value ranges from 0 to 4. Different input parameter values indicate different calibration granularities.

-  If the input parameter is set to **0** (default value), the **PG_RELFILENODE_SIZE** system catalog is not rebuilt and the user and schema space are recalibrated.
-  If the input parameter is set to **1**, the **PG_RELFILENODE_SIZE** system catalog is rebuilt and the user and schema space are recalibrated.
-  If the input parameter is set to **2**, the **PG_RELFILENODE_SIZE** system catalog is rebuilt.
-  If the input parameter is set to **3**, the schema space is recalibrated.
-  If the input parameter is set to **4**, the user space is recalibrated.

Return type: text

Example:

::

   SELECT * FROM pgxc_wlm_readjust_relfilenode_size_table(1);
       result
   --------------
    Exec success
   (1 row)

pgxc_wlm_get_schema_space(cstring)
----------------------------------

Description: Obtains the schema space of each instance in a specified logical cluster on the CN.

Return type: record

The following table describes return columns.

============ ====== ========================
Column       Type   Description
============ ====== ========================
schemaname   text   Schema name
schemaid     oid    Schema OID
databasename text   Database name
databaseid   oid    Database OID
nodename     text   Instance name
nodegroup    text   Name of the node group
usedspace    bigint Size of the used space
permspace    bigint Upper limit of the space
============ ====== ========================

Examples:

::

   SELECT * FROM pgxc_wlm_get_schema_space('group1');
        schemaname     | schemaid | databasename | databaseid |   nodename   |  nodegroup   | usedspace | permspace
   --------------------+----------+--------------+------------+--------------+--------------+-----------+-----------
    pg_catalog         |       11 | test1        |      16384 | datanode1    | installation |   9469952 |        -1
    public             |     2200 | postgres     |      15253 | datanode1    | installation |  25280512 |        -1
    pg_toast           |       99 | test1        |      16384 | datanode1    | installation |   1859584 |        -1
    cstore             |      100 | test1        |      16384 | datanode1    | installation |         0 |        -1
    data_redis         |    18106 | postgres     |      15253 | datanode1    | installation |    655360 |        -1
    data_redis         |    18116 | test1        |      16384 | datanode1    | installation |         0 |        -1
    public             |     2200 | test1        |      16384 | datanode1    | installation |     16384 |        -1
    dbms_om            |     3987 | postgres     |      15253 | datanode1    | installation |         0 |        -1
    dbms_job           |     3988 | postgres     |      15253 | datanode1    | installation |         0 |        -1
    dbms_om            |     3987 | test1        |      16384 | datanode1    | installation |         0 |        -1
    dbms_job           |     3988 | test1        |      16384 | datanode1    | installation |         0 |        -1
    sys                |    11693 | postgres     |      15253 | datanode1    | installation |         0 |        -1
    sys                |    11693 | test1        |      16384 | datanode1    | installation |         0 |        -1
    utl_file           |    14644 | postgres     |      15253 | datanode1    | installation |         0 |        -1
    utl_raw            |    14669 | postgres     |      15253 | datanode1    | installation |         0 |        -1
    dbms_sql           |    14674 | postgres     |      15253 | datanode1    | installation |         0 |        -1
    dbms_output        |    14662 | postgres     |      15253 | datanode1    | installation |         0 |        -1
    dbms_random        |    14666 | postgres     |      15253 | datanode1    | installation |         0 |        -1
    dbms_lob           |    14701 | postgres     |      15253 | datanode1    | installation |         0 |        -1
    information_schema |    14300 | postgres     |      15253 | datanode1    | installation |    294912 |        -1
    information_schema |    14300 | test1        |      16384 | datanode1    | installation |    294912 |        -1
    utl_file           |    14644 | test1        |      16384 | datanode1    | installation |         0 |        -1
    dbms_output        |    14662 | test1        |      16384 | datanode1    | installation |         0 |        -1
    dbms_random        |    14666 | test1        |      16384 | datanode1    | installation |         0 |        -1
    utl_raw            |    14669 | test1        |      16384 | datanode1    | installation |         0 |        -1
    dbms_sql           |    14674 | test1        |      16384 | datanode1    | installation |         0 |        -1
    dbms_lob           |    14701 | test1        |      16384 | datanode1    | installation |         0 |        -1
    pg_catalog         |       11 | postgres     |      15253 | datanode1    | installation |  13049856 |        -1
    redisuser          |    16387 | postgres     |      15253 | datanode1    | installation |    630784 |        -1
    pg_toast           |       99 | postgres     |      15253 | datanode1    | installation |   3080192 |        -1
    cstore             |      100 | postgres     |      15253 | datanode1    | installation |   2408448 |        -1
    pg_catalog         |       11 | test1        |      16384 | datanode2    | installation |   9469952 |        -1
    public             |     2200 | postgres     |      15253 | datanode2    | installation |  25214976 |        -1
    pg_toast           |       99 | test1        |      16384 | datanode2    | installation |   1859584 |        -1
    cstore             |      100 | test1        |      16384 | datanode2    | installation |         0 |        -1
    data_redis         |    18106 | postgres     |      15253 | datanode2    | installation |    655360 |        -1
    data_redis         |    18116 | test1        |      16384 | datanode2    | installation |         0 |        -1
    public             |     2200 | test1        |      16384 | datanode2    | installation |     16384 |        -1
    dbms_om            |     3987 | postgres     |      15253 | datanode2    | installation |         0 |        -1
    dbms_job           |     3988 | postgres     |      15253 | datanode2    | installation |         0 |        -1
    dbms_om            |     3987 | test1        |      16384 | datanode2    | installation |         0 |        -1
    dbms_job           |     3988 | test1        |      16384 | datanode2    | installation |         0 |        -1

pgxc_wlm_analyze_schema_space(cstring)
--------------------------------------

Description: Obtains the schema space of a specified logical cluster on the CN.

Return type: record

The following table describes return columns.

+--------------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Column       | Type    | Description                                                                                                                                                                    |
+==============+=========+================================================================================================================================================================================+
| schemaname   | text    | Schema name                                                                                                                                                                    |
+--------------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| databasename | text    | Database name                                                                                                                                                                  |
+--------------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| nodegroup    | text    | Name of the node group                                                                                                                                                         |
+--------------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| total_value  | bigint  | Total cluster space in the current schema                                                                                                                                      |
+--------------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| avg_value    | bigint  | Average space of instances in the current schema                                                                                                                               |
+--------------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| skew_percent | integer | Skew ratio                                                                                                                                                                     |
+--------------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| extend_info  | text    | Extended information, including the maximum space of a single instance, minimum space of a single instance, and names of the instances with the maximum space or minimum space |
+--------------+---------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples:

::

   SELECT * FROM pgxc_wlm_analyze_schema_space('group1');
        schemaname     | databasename |  nodegroup   | total_value | avg_value | skew_percent |                  extend_info
   --------------------+--------------+--------------+-------------+-----------+--------------+-----------------------------------------------
    pg_catalog         | test1        | installation |    56819712 |   9469952 |            0 | min:9469952 datanode1,max:9469952 datanode1
    public             | postgres     | installation |   150495232 |  25082538 |            0 | min:24903680 datanode6,max:25280512 datanode1
    pg_toast           | test1        | installation |    11157504 |   1859584 |            0 | min:1859584 datanode1,max:1859584 datanode1
    cstore             | test1        | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    data_redis         | postgres     | installation |     1966080 |    327680 |           50 | min:0 datanode4,max:655360 datanode1
    data_redis         | test1        | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    public             | test1        | installation |       98304 |     16384 |            0 | min:16384 datanode1,max:16384 datanode1
    dbms_om            | postgres     | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    dbms_job           | postgres     | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    dbms_om            | test1        | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    dbms_job           | test1        | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    sys                | postgres     | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    sys                | test1        | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    utl_file           | postgres     | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    utl_raw            | postgres     | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    dbms_sql           | postgres     | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    dbms_output        | postgres     | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    dbms_random        | postgres     | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    dbms_lob           | postgres     | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    information_schema | postgres     | installation |     1769472 |    294912 |            0 | min:294912 datanode1,max:294912 datanode1
    information_schema | test1        | installation |     1769472 |    294912 |            0 | min:294912 datanode1,max:294912 datanode1
    utl_file           | test1        | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    dbms_output        | test1        | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    dbms_random        | test1        | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    utl_raw            | test1        | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    dbms_sql           | test1        | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    dbms_lob           | test1        | installation |           0 |         0 |            0 | min:0 datanode1,max:0 datanode1
    pg_catalog         | postgres     | installation |    75431936 |  12571989 |            3 | min:12124160 datanode4,max:13049856 datanode1
    redisuser          | postgres     | installation |     1884160 |    314026 |           50 | min:16384 datanode4,max:630784 datanode1
    pg_toast           | postgres     | installation |    17154048 |   2859008 |            7 | min:2637824 datanode4,max:3080192 datanode1
    cstore             | postgres     | installation |    15294464 |   2549077 |            5 | min:2408448 datanode1,max:2703360 datanode6
   (31 rows)

gs_wlm_set_queryband_action(cstring,cstring,int4)
-------------------------------------------------

Description: Sets the action and query order of **query_band**.

Return type: boolean

The following table describes the input parameters.

+--------+---------+-----------------------------------------------------------------+
| Name   | Type    | Description                                                     |
+========+=========+=================================================================+
| qband  | cstring | Query band key-value pair. The maximum length is 63 characters. |
+--------+---------+-----------------------------------------------------------------+
| action | cstring | Action associated to a query band                               |
+--------+---------+-----------------------------------------------------------------+
| order  | int4    | Query band query order. The default value is **-1**.            |
+--------+---------+-----------------------------------------------------------------+

Examples:

::

   SELECT * FROM gs_wlm_set_queryband_action('a=1','respool=p1');
    gs_wlm_set_queryband_action
   -----------------------------
    t
   (1 row)

   SELECT * FROM gs_wlm_set_queryband_action('a=3','respool=p1;priority=rush',1);
    gs_wlm_set_queryband_action
   -----------------------------
    t
   (1 row)

gs_wlm_set_queryband_order(cstring,int4)
----------------------------------------

Description: Sets the **query_band** query order.

Return type: boolean

The following table describes the input parameters.

===== ======= ====================================================
Name  Type    Description
===== ======= ====================================================
qband cstring **query_band** key-value pairs
order int4    Query band query order. The default value is **-1**.
===== ======= ====================================================

Examples:

::

   SELECT * FROM gs_wlm_set_queryband_order('a=1',2);
    gs_wlm_set_queryband_action
   -----------------------------
    t
   (1 row)

gs_wlm_get_queryband_action(cstring)
------------------------------------

Description: Obtains the action and query order of **query_band**.

Return type: record

The following table describes return columns.

+------------+---------+----------------------------------------------------------+
| Column     | Type    | Description                                              |
+============+=========+==========================================================+
| qband      | cstring | **query_band** key-value pairs                           |
+------------+---------+----------------------------------------------------------+
| respool_id | Oid     | OID of the resource pool associated with **query_band**  |
+------------+---------+----------------------------------------------------------+
| respool    | text    | Name of the resource pool associated with **query_band** |
+------------+---------+----------------------------------------------------------+
| priority   | text    | Intra-queue priority associated with **query_band**      |
+------------+---------+----------------------------------------------------------+
| qborder    | int4    | **query_band** query order                               |
+------------+---------+----------------------------------------------------------+

Examples:

::

   SELECT * FROM gs_wlm_get_queryband_action('a=1');
   qband | respool_id | respool | priority | qborder
   -------+------------+---------+----------+---------
    a=1   |      16388 | p1      | Medium   |      -1
   (1 row)

gs_cgroup_reload_conf()
-----------------------

Description: This function loads the Cgroup configuration file online on the current instance.

Return type: record

The following table describes return columns.

========= ==== ====================================================
Column    Type Description
========= ==== ====================================================
node_name text Instance name
node_host text IP address of the node where the instance is located
result    text Whether Cgroup online loading is successful
========= ==== ====================================================

Examples:

::

   SELECT * FROM gs_cgroup_reload_conf();
    node_name |   node_host    | result
   -----------+----------------+---------
    cn_5001   | 192.168.178.35 | success

pgxc_cgroup_reload_conf()
-------------------------

Description: This function loads the Cgroup configuration file online on all instances of the system.

Return type: record

The following table describes return columns.

========= ==== ====================================================
Column    Type Description
========= ==== ====================================================
node_name text Instance name
node_host text IP address of the node where the instance is located
result    text Whether Cgroup online loading is successful
========= ==== ====================================================

Examples:

::

   SELECT * FROM pgxc_cgroup_reload_conf();
     node_name   |    node_host    | result
   --------------+-----------------+---------
    dn_6025_6026 | 192.168.178.177 | success
    dn_6049_6050 | 192.168.179.79  | success
    dn_6051_6052 | 192.168.179.79  | success
    dn_6055_6056 | 192.168.179.79  | success
    dn_6067_6068 | 192.168.181.57  | success
    dn_6023_6024 | 192.168.178.39  | success
    dn_6009_6010 | 192.168.181.21  | success
    dn_6011_6012 | 192.168.181.21  | success
    dn_6015_6016 | 192.168.181.21  | success
    dn_6029_6030 | 192.168.178.177 | success
    dn_6031_6032 | 192.168.178.177 | success
    dn_6045_6046 | 192.168.179.45  | success
    cn_5001      | 192.168.178.35  | success
    cn_5003      | 192.168.178.39  | success
    dn_6061_6062 | 192.168.181.179 | success
    cn_5006      | 192.168.179.45  | success
    cn_5004      | 192.168.178.177 | success
    cn_5002      | 192.168.181.21  | success
    cn_5005      | 192.168.178.187 | success
    dn_6019_6020 | 192.168.178.39  | success
    dn_6007_6008 | 192.168.178.35  | success
    dn_6071_6072 | 192.168.181.57  | success
    dn_6003_6004 | 192.168.178.35  | success
    dn_6013_6014 | 192.168.181.21  | success
    dn_6035_6036 | 192.168.178.187 | success
    dn_6037_6038 | 192.168.178.187 | success
    dn_6001_6002 | 192.168.178.35  | success
    dn_6063_6064 | 192.168.181.179 | success
    dn_6005_6006 | 192.168.178.35  | success
    dn_6057_6058 | 192.168.181.179 | success
    dn_6069_6070 | 192.168.181.57  | success
    dn_6027_6028 | 192.168.178.177 | success
    dn_6059_6060 | 192.168.181.179 | success
    dn_6041_6042 | 192.168.179.45  | success
    dn_6043_6044 | 192.168.179.45  | success
    dn_6047_6048 | 192.168.179.45  | success
    dn_6033_6034 | 192.168.178.187 | success
    dn_6065_6066 | 192.168.181.57  | success
    dn_6021_6022 | 192.168.178.39  | success
    dn_6017_6018 | 192.168.178.39  | success
    dn_6039_6040 | 192.168.178.187 | success
    dn_6053_6054 | 192.168.179.79  | success
   (42 rows)

pgxc_cgroup_reload_conf(text)
-----------------------------

Description: This function loads the Cgroup configuration file online on a node. The input parameter is the IP address of the node.

Return type: record

The following table describes return columns.

========= ==== ====================================================
Column    Type Description
========= ==== ====================================================
node_name text Instance name
node_host text IP address of the node where the instance is located
result    text Whether Cgroup online loading is successful
========= ==== ====================================================

Examples:

::

   SELECT * FROM pgxc_cgroup_reload_conf('192.168.178.35');
     node_name   |   node_host    | result
   --------------+----------------+---------
    cn_5001      | 192.168.178.35 | success
    dn_6007_6008 | 192.168.178.35 | success
    dn_6003_6004 | 192.168.178.35 | success
    dn_6001_6002 | 192.168.178.35 | success
    dn_6005_6006 | 192.168.178.35 | success
   (5 rows)

gs_wlm_node_recover(boolean isForce)
------------------------------------

Description: This function updates and restores job information and counts on the CCN in dynamic resource management mode. It can be executed only by administrators, and is usually used to restore a faulty CN after it was restarted. This function is called by the Cluster Manager (CM). Its usage is as follows:

-  If this function is executed by CN, it instructs the CCN to clear job information and counts on the CN.
-  If this function is executed by CCN, it resets job counts and obtains the latest slow lane job information from the CN.

Return type: bool

gs_wlm_node_clean(cstring nodename)
-----------------------------------

Description: On the CCN in dynamic resource management mode, clears the job information and counts of a specified CN. This function can be executed only by administrators, and is usually used to restore a faulty CN after it was restarted. This function is called by the Cluster Manager (CM). Generally, users are not advised to call it.

Return type: bool

pg_stat_get_wlm_node_resource_info(int4)
----------------------------------------

Description: This function displays the summary of all DN resources.

Return type: record

The following table describes return columns.

=============== ======= =============================
Column          Type    Description
=============== ======= =============================
min_mem_util    integer Minimum memory usage of a DN
max_mem_util    integer Maximum memory usage of a DN
min_cpu_util    integer Minimum CPU usage of a DN
max_cpu_util    integer Maximum CPU usage of a DN
min_io_util     integer Minimum I/O usage of a DN
max_io_util     integer Maximum I/O usage of a DN
phy_usemem_rate integer Maximum physical memory usage
=============== ======= =============================

pg_stat_get_workload_struct_info()
----------------------------------

Description: Load management function for locating CCN queuing problems. This function is an internal function. To use this function, contact technical support.

Return type: record

pgxc_query_resource_info(query_id bigint)
-----------------------------------------

Description: This function displays resource monitoring information about the statement with a specified query ID on all DNs. It is supported only by clusters of version 9.1.0.100 or later.

Return type: record

The following table describes return columns.

+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Column      | Type   | Description                                                                                                                                                    |
+=============+========+================================================================================================================================================================+
| node_name   | text   | Instance name, which contains only DNs.                                                                                                                        |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| user_id     | oid    | User ID.                                                                                                                                                       |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| queryid     | bigint | Internal query ID used for statement execution.                                                                                                                |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| used_mem    | int    | Memory used by the statement on the current DN. The unit is MB.                                                                                                |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| cpu_time    | bigint | CPU time of a statement on the current DN. The unit is ms.                                                                                                     |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| used_cpu    | double | Number of CPUs used by the statement on the current DN.                                                                                                        |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| spill_size  | bigint | Amount of data spilled to disks on the current DN. The default value is 0. The unit is MB.                                                                     |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| read_bytes  | bigint | Number of logical read bytes used by the statement on the current DN. The unit is KB.                                                                          |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| write_bytes | bigint | Number of logical write bytes used by the statement on the current DN. The unit is KB.                                                                         |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| read_count  | bigint | Number of logical reads used by the statement on the current DN.                                                                                               |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| write_count | bigint | Number of logical writes used by the statement on the current DN.                                                                                              |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| read_speed  | int    | Logical read rate used by the statement on the current DN. The unit is KB/s.                                                                                   |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| write_speed | int    | Logical write rate used by the statement on the current DN. The unit is KB/s.                                                                                  |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| curr_iops   | int    | I/O operations per second of the statement on the current DN. It is recorded as a count in a column-store table and as a count of 10,000 in a row-store table. |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| send_pkg    | bigint | Total number of communication packages sent by a statement across all DNs.                                                                                     |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| recv_pkg    | bigint | Total number of communication packages received by a statement across all DNs.                                                                                 |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| send_bytes  | bigint | Total sent data of the statement stream, in byte.                                                                                                              |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| recv_bytes  | bigint | Total received data of the statement stream, in byte.                                                                                                          |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| send_speed  | int    | Network sending rate of the statement on the current DN. The unit is KB/s.                                                                                     |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
| recv_speed  | int    | Network receiving rate of the statement on the current DN. The unit is KB/s.                                                                                   |
+-------------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------+
