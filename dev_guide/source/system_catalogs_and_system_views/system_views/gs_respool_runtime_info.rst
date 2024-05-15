:original_name: dws_04_0976.html

.. _dws_04_0976:

GS_RESPOOL_RUNTIME_INFO
=======================

**GS_RESPOOL_RUNTIME_INFO** displays information about the running of jobs in all resource pools on the current CN.

.. table:: **Table 1** GS_RESPOOL_RUNTIME_INFO columns

   +-----------+------+----------------------------------------------------------------------------------------------------------------------------------+
   | Name      | Type | Description                                                                                                                      |
   +===========+======+==================================================================================================================================+
   | nodegroup | name | Name of the logical cluster of the resource pool. The default value is **installation**.                                         |
   +-----------+------+----------------------------------------------------------------------------------------------------------------------------------+
   | rpname    | name | Resource pool name                                                                                                               |
   +-----------+------+----------------------------------------------------------------------------------------------------------------------------------+
   | ref_count | int  | Number of jobs referenced by resource pools. The number is counted regardless of whether a job is controlled by a resource pool. |
   +-----------+------+----------------------------------------------------------------------------------------------------------------------------------+
   | fast_run  | int  | Number of running jobs in the fast lane of the resource pool                                                                     |
   +-----------+------+----------------------------------------------------------------------------------------------------------------------------------+
   | fast_wait | int  | Number of jobs queued in the fast lane of the resource pool                                                                      |
   +-----------+------+----------------------------------------------------------------------------------------------------------------------------------+
   | slow_run  | int  | Number of running jobs in the slow lane of the resource pool                                                                     |
   +-----------+------+----------------------------------------------------------------------------------------------------------------------------------+
   | slow_wait | int  | Number of jobs queued in the slow lane of the resource pool                                                                      |
   +-----------+------+----------------------------------------------------------------------------------------------------------------------------------+
