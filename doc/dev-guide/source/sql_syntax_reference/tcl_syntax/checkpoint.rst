:original_name: dws_06_0258.html

.. _dws_06_0258:

CHECKPOINT
==========

Function
--------

A checkpoint is a point in the transaction log sequence at which all data files have been updated to reflect the information in the log. All data files will be flushed to a disk.

**CHECKPOINT** forces a transaction log checkpoint. By default, WALs periodically specify checkpoints in a transaction log. You may use **gs_guc** to specify run-time parameters **checkpoint_segments** and **checkpoint_timeout** to adjust the atomized checkpoint intervals.

Precautions
-----------

-  Only a system administrator has the permission to call **CHECKPOINT**.
-  **CHECKPOINT** forces an immediate checkpoint when the related command is issued, without waiting for a regular checkpoint scheduled by the system.

Syntax
------

::

   CHECKPOINT;

Parameter Description
---------------------

None

Examples
--------

Set a checkpoint:

::

   CHECKPOINT;
