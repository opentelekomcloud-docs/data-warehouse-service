:original_name: dws_04_0895.html

.. _dws_04_0895:

Kernel Resources
================

This section describes kernel resource parameters. Whether these parameters take effect depends on OS settings.

max_files_per_process
---------------------

**Parameter description**: Specifies the maximum number of simultaneously open files allowed by each server process. If the kernel is enforcing a proper limit, setting this parameter is not required.

But on some platforms, especially on most BSD systems, the kernel allows independent processes to open far more files than the system can really support. If the message "Too many open files" is displayed, try to reduce the setting. Generally, the number of file descriptors must be greater than or equal to the maximum number of concurrent tasks multiplied by the number of primary DNs on the current physical machine (``*max_files_per_process*3``).

**Type**: POSTMASTER

**Value range**: an integer ranging from 25 to INT_MAX

**Default value**: **1000**

max_files_per_node
------------------

**Parameter description**: Specifies the maximum number of files that can be opened by a single SQL statement on a single node. Generally, you do not need to set this parameter. This parameter is supported by version 8.1.3 or later clusters.

**Type**: SUSET

**Value range**: an integer ranging from **-1** to **INT_MAX**. The value **-1** indicates that the maximum number is not limited.

**Default value**: **-1**

.. note::

   -  The default value of this parameter is **-1** in a new cluster. In an upgrade scenario, the default value of this parameter is retained for forward compatibility.
   -  If error message "The last file name is [%s] and %d files have already been opened on data node [%s] with a maximum of %d files." is displayed during statement execution, increase the value of **max_files_per_node**.

enable_fd_check
---------------

**Parameter description**: Specifies whether to perform verification when FD is used. This parameter is supported only by 8.2.1.300 and later versions.

**Type**: SIGHUP

**Value range**: Boolean

-  **on** indicates that FD verification is enabled.
-  **off** indicates that FD verification is enabled.

**Default value**: **on**
