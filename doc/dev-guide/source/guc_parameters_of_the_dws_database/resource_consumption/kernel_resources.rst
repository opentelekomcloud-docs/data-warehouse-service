:original_name: dws_04_0895.html

.. _dws_04_0895:

Kernel Resources
================

This section describes kernel resource parameters. Whether these parameters take effect depends on OS settings.

max_files_per_node
------------------

**Parameter description**: Specifies the maximum number of files that can be opened by a single SQL statement on a single node. Generally, you do not need to set this parameter.

**Type**: SUSET

**Value range**: an integer ranging from **-1** to **INT_MAX**. The value **-1** indicates that the maximum number is limited.

**Default value**: **50000**

.. note::

   -  For a newly installed cluster of 9.1.0 or later, the default value of this parameter is **50000**.
   -  In an upgrade scenario, if the original cluster supports the **max_files_per_node** parameter, the default value of this parameter remains the same for forward compatibility.
   -  In the upgrade scenario, if the original cluster does not support the **max_files_per_node** parameter, the default value of this parameter is **-1** after the upgrade.
   -  If error message "The last file name is [%s] and %d files have already been opened on data node [%s] with a maximum of %d files." is displayed during statement execution, increase the value of **max_files_per_node**.
