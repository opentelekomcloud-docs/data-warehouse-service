:original_name: dws_04_0980.html

.. _dws_04_0980:

PGXC_RESPOOL_RUNTIME_INFO
=========================

**PGXC_RESPOOL_RUNTIME_INFO** displays the running information about all resource pool jobs on all CNs.

.. table:: **Table 1** PGXC_RESPOOL_RUNTIME_INFO columns

   +-----------+------+-------------------------------------------------------------------------------------------------------------+
   | Column    | Type | Description                                                                                                 |
   +===========+======+=============================================================================================================+
   | nodename  | Name | CN name.                                                                                                    |
   +-----------+------+-------------------------------------------------------------------------------------------------------------+
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
