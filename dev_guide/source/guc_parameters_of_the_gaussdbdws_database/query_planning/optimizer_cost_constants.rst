:original_name: dws_04_0910.html

.. _dws_04_0910:

Optimizer Cost Constants
========================

This section describes the optimizer cost constants. The cost variables described in this section are measured on an arbitrary scale. Only their relative values matter, therefore scaling them all in or out by the same factor will result in no differences in the optimizer's choices. By default, these cost variables are based on the cost of sequential page fetches, that is, **seq_page_cost** is conventionally set to **1.0** and the other cost variables are set with reference to the parameter. However, you can use a different scale, such as actual execution time in milliseconds.

seq_page_cost
-------------

**Parameter description**: Specifies the optimizer's estimated cost of a disk page fetch that is part of a series of sequential fetches.

**Type**: USERSET

**Value range**: a floating point number ranging from 0 to DBL_MAX

**Default value**: **1**

random_page_cost
----------------

**Parameter description**: Specifies the optimizer's estimated cost of an out-of-sequence disk page fetch.

**Type**: USERSET

**Value range**: a floating point number ranging from 0 to DBL_MAX

**Default value**: **4**

.. note::

   -  Although the server allows you to set the value of **random_page_cost** to less than that of **seq_page_cost**, it is not physically sensitive to do so. However, setting them equal makes sense if the database is entirely cached in RAM, because in that case there is no penalty for fetching pages out of sequence. Also, in a heavily-cached database you should lower both values relative to the CPU parameters, since the cost of fetching a page already in RAM is much smaller than it would normally be.
   -  This value can be overwritten for tables and indexes in a particular tablespace by setting the tablespace parameter of the same name.
   -  Comparing to **seq_page_cost**, reducing this value will cause the system to prefer index scans and raising it makes index scans relatively more expensive. You can increase or decrease both values at the same time to change the disk I/O cost relative to CPU cost.

cpu_tuple_cost
--------------

**Parameter description**: Specifies the optimizer's estimated cost of processing each row during a query.

**Type**: USERSET

**Value range**: a floating point number ranging from 0 to DBL_MAX

**Default value**: **0.01**

cpu_index_tuple_cost
--------------------

**Parameter description**: Specifies the optimizer's estimated cost of processing each index entry during an index scan.

**Type**: USERSET

**Value range**: a floating point number ranging from 0 to DBL_MAX

**Default value:** **0.005**

cpu_operator_cost
-----------------

**Parameter description**: Specifies the optimizer's estimated cost of processing each operator or function during a query.

**Type**: USERSET

**Value range**: a floating point number ranging from 0 to DBL_MAX

**Default value**: **0.0025**

effective_cache_size
--------------------

**Parameter description**: Specifies the optimizer's assumption about the effective size of the disk cache that is available to a single query.

When setting this parameter you should consider both GaussDB(DWS)'s shared buffer and the kernel's disk cache. Also, take into account the expected number of concurrent queries on different tables, since they will have to share the available space.

This parameter has no effect on the size of shared memory allocated by GaussDB(DWS). It is used only for estimation purposes and does not reserve kernel disk cache. The value is in the unit of disk page. Usually the size of each page is 8192 bytes.

**Type**: USERSET

**Value range**: an integer ranging is from 1 to INT_MAX. The unit is 8 KB.

A value greater than the default one may enable index scanning, and a value less than the default one may enable sequence scanning.

**Default value**: **128MB**

allocate_mem_cost
-----------------

**Parameter description**: Specifies the query optimizer's estimated cost of creating a Hash table for memory space using Hash join. This parameter is used for optimization when the Hash join estimation is inaccurate.

**Type**: USERSET

**Value range**: a floating point number ranging from 0 to DBL_MAX

**Default value**: **0**

smp_thread_cost
---------------

**Parameter description**: Specifies the optimizer's cost for calculating parallel threads of an operator. This parameter is used for tuning if **query_dop** is not suitable for system load management. (This parameter is supported only by clusters of version 8.2.0 or later.)

**Type**: USERSET

**Value range**: a floating point number ranging from 1 to 10000

**Default value**: **1000**
