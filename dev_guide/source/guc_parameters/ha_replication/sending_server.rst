:original_name: dws_04_0905.html

.. _dws_04_0905:

Sending Server
==============

wal_keep_segments
-----------------

**Parameter description**: Specifies the number of Xlog file segments. Specifies the minimum number of transaction log files stored in the **pg_xlog** directory. The standby server obtains log files from the primary server for streaming replication.

**Type**: SIGHUP

**Value range**: an integer ranging from 2 to INT_MAX

**Default value**: **128**

**Setting suggestions**:

-  During WAL archiving or recovery from a checkpoint on the server, the system retains more log files than the number specified by **wal_keep_segments**.
-  If this parameter is set to a too small value, a transaction log may have been overwritten by a new transaction log before requested by the standby server. As a result, the request fails, and the relationship between the primary and standby servers is interrupted.
-  If the HA system uses asynchronous transmission, increase the value of **wal_keep_segments** when data greater than 4 GB is continuously imported in COPY mode. Take T6000 board as an example. If the data to be imported reaches 50 GB, you are advised to set this parameter to **1000**. You can dynamically restore the setting of this parameter after data import is complete and the WAL synchronization is proper.

max_build_io_limit
------------------

**Parameter description:** Specifies the data volume that can be read from the disk per second when the primary server provides a build session to the standby server.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 1048576. The unit is KB.

**Default value:** 0, indicating that the I/O flow is not restricted when the primary server provides a build session to the standby server.

**Setting suggestions:** Set this parameter based on the disk bandwidth and job model. If there is no flow restriction or job interference, for disks with good performance such as SSDs, a full build consumes a relatively small proportion of bandwidth and has little impact on service performance. In this case, you do not need to set the threshold. If the service performance of a common 10,000 rpm SAS disk deteriorates significantly during a build, you are advised to set the parameter to 20 MB.

This setting directly affects the build speed and completion time. Therefore, you are advised to set this parameter to a value larger than 10 MB. During off-peak hours, you are advised to remove the flow restriction to restore to the normal build speed.

.. note::

   -  This parameter is used during peak hours or when the disk I/O pressure of the primary server is high. It limits the build flow rate on the standby server to reduce the impact on primary server services. After the service peak hours, you can remove the restriction or reset the flow rate threshold.
   -  You are advised to set a proper threshold based on service scenarios and disk performance.
