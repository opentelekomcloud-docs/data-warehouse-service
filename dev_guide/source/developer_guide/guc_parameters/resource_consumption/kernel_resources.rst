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
