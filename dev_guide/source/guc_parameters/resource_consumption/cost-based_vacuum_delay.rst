:original_name: dws_04_0896.html

.. _dws_04_0896:

Cost-based Vacuum Delay
=======================

The purpose of cost-based vacuum delay is to allow administrators to reduce the I/O impact of **VACUUM** and **ANALYZE** statements on concurrently active databases. For example, when maintenance statements such as **VACUUM** and **ANALYZE** do not need to be executed quickly and do not interfere with other database operations, administrators can use this function to achieve this purpose.

.. important::

   Certain operations hold critical locks and should be complete as quickly as possible. In GaussDB(DWS), cost-based vacuum delays do not take effect during such operations. To avoid uselessly long delays in such cases, the actual delay is calculated as follows and is the maximum value of the following calculation results:

   -  vacuum_cost_delay*accumulated_balance/vacuum_cost_limit
   -  vacuum_cost_delay*4

During the execution of the ANALYZE \| ANALYSE and VACUUM statements, the system maintains an internal counter that keeps track of the estimated cost of the various I/O operations that are performed. When the accumulated cost reaches a limit (specified by **vacuum_cost_limit**), the process performing the operation will sleep for a short period of time (specified by **vacuum_cost_delay**). Then, the counter resets and the operation continues.

By default, this feature is disabled. To enable this feature, set **vacuum_cost_delay** to a value other than 0.

vacuum_cost_delay
-----------------

**Parameter description**: Specifies the length of time that the process will sleep when **vacuum_cost_limit** has been exceeded.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 100. The unit is millisecond (ms). A positive number enables cost-based vacuum delay and **0** disables cost-based vacuum delay.

**Default value**: **0**

.. important::

   -  On many systems, the effective resolution of sleep length is 10 ms. Therefore, setting this parameter to a value that is not a multiple of 10 has the same effect as setting it to the next higher multiple of 10.
   -  This parameter is set to a small value, such as 10 or 20 milliseconds.

vacuum_cost_limit
-----------------

**Parameter description:** Specifies the cost limit. The cleanup process will sleep if this limit is exceeded.

**Type**: USERSET

**Value range**: an integer ranging from 1 to 10000. The unit is ms.

**Default value:** **200**

vacuum_cost_page_hit
--------------------

**Parameter description:** Specifies the estimated cost for vacuuming a buffer found in the shared buffer. It represents the cost to lock the buffer pool, look up the shared Hash table, and scan the page.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 10000. The unit is millisecond (ms).

**Default value**: **1**

vacuum_cost_page_miss
---------------------

**Parameter description:** Specifies the estimated cost for vacuuming a buffer read from the disk. It represents the cost to lock the buffer pool, look up the shared Hash table, read the desired block from the disk, and scan the block.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 10000. The unit is millisecond (ms).

**Default value**: **2**

vacuum_cost_page_dirty
----------------------

**Parameter description:** Specifies the estimated cost charged when vacuum modifies a block that was previously clean. It represents the I/Os required to flush the dirty block out to disk again.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 10000. The unit is millisecond (ms).

**Default value**: **20**
