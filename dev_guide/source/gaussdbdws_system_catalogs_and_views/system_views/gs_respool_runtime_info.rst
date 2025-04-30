:original_name: dws_04_0976.html

.. _dws_04_0976:

GS_RESPOOL_RUNTIME_INFO
=======================

**GS_RESPOOL_RUNTIME_INFO** displays information about the running of jobs in all resource pools on the current CN.

.. table:: **Table 1** GS_RESPOOL_RUNTIME_INFO columns

   +-----------+------+-------------------------------------------------------------------------------------------------------------+
   | Column    | Type | Description                                                                                                 |
   +===========+======+=============================================================================================================+
   | nodegroup | Name | Name of the logical cluster the resource pool belongs to. The default cluster is **installation**.          |
   +-----------+------+-------------------------------------------------------------------------------------------------------------+
   | rpname    | Name | Resource pool name.                                                                                         |
   +-----------+------+-------------------------------------------------------------------------------------------------------------+
   | ref_count | Int  | Number of jobs that reference the resource pool. This count includes both controlled and uncontrolled jobs. |
   +-----------+------+-------------------------------------------------------------------------------------------------------------+
   | fast_run  | Int  | Number of jobs currently running in the resource pool's fast lane.                                          |
   +-----------+------+-------------------------------------------------------------------------------------------------------------+
   | fast_wait | Int  | Number of jobs currently queued in the resource pool's fast lane.                                           |
   +-----------+------+-------------------------------------------------------------------------------------------------------------+
   | slow_run  | Int  | Number of jobs currently running in the resource pool's slow lane.                                          |
   +-----------+------+-------------------------------------------------------------------------------------------------------------+
   | slow_wait | Int  | Number of jobs currently queued in the resource pool's slow lane.                                           |
   +-----------+------+-------------------------------------------------------------------------------------------------------------+
