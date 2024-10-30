:original_name: dws_04_0907.html

.. _dws_04_0907:

Standby Server
==============

build_backup_param
------------------

**Parameter description**: Specifies the minimum specifications for disk backup during incremental build.

**Type**: SIGHUP

**Value range**: a string

**Default value**: (1%, 1G, 1G)

.. note::

   This parameter specifies whether the **pg_rewind_bak** directory is generated during incremental build. The character string takes effect only when it is configured in the 'x %, yG, zG' format. This parameter is valid only when **gs_guc set** is set to a valid value. **x** indicates the percentage of minimum remaining space, **y** indicates the minimum remaining space, and **z** indicates the total disk space.

   The **pg_rewind_bak** file is generated and backed up only when both of the following conditions are met:

   -  Condition 1: The total disk capacity is greater than or equals to **z** GB. If this condition is not met, the backup is not performed. If this condition is met, the system continues to check condition 2.
   -  Condition 2: The remaining disk space is greater than or equals to **y** GB and the percentage of the remaining disk space is greater than or equals to **x %**.
