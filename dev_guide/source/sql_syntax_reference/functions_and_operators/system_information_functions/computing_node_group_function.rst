:original_name: dws_06_0345.html

.. _dws_06_0345:

Computing Node Group Function
=============================

pv_compute_pool_workload()
--------------------------

Description: Load status of a computing Node Group.

Return type: void

Example:

::

   SELECT * from pv_compute_pool_workload();
    nodename  | rpinuse | maxrp | nodestate
   -----------+---------+-------+-----------
    datanode1 |       0 |  1000 | normal
    datanode2 |       0 |  1000 | normal
   (2 rows)
