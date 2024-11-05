:original_name: dws_04_0682.html

.. _dws_04_0682:

GLOBAL_REDO_STAT
================

**GLOBAL_REDO_STAT** displays the total statistics of XLOG redo operations on all nodes in a cluster. Except the **avgiotim** column (indicating the average redo write time of all nodes), the names of the other columns in this view are the same as those in the :ref:`PV_REDO_STAT <dws_04_0856>` view. The respective meanings of the other columns are the sum of the values of the same columns in the **PV_REDO_STAT** view on each node.

.. note::

   This view is accessible only to users with system administrator rights.
