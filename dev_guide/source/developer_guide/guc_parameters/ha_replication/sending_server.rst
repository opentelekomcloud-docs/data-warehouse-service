:original_name: dws_04_0905.html

.. _dws_04_0905:

Sending Server
==============

wal_keep_segments
-----------------

**Parameter description**: Specifies the number of Xlog file segments. Specifies the minimum number of transaction log files stored in the **pg_xlog** directory. The standby server obtains log files from the primary server for streaming replication.

**Type**: SIGHUP

**Value range**: an integer ranging from 2 to INT_MAX

**Default value**: **65**

**Setting suggestions**:

-  During WAL archiving or recovery from a checkpoint on the server, the system retains more log files than the number specified by **wal_keep_segments**.
-  If this parameter is set to a too small value, a transaction log may have been overwritten by a new transaction log before requested by the standby server. As a result, the request fails, and the relationship between the primary and standby servers is interrupted.
-  If the HA system uses asynchronous transmission, increase the value of **wal_keep_segments** when data greater than 4 GB is continuously imported in COPY mode. Take T6000 board as an example. If the data to be imported reaches 50 GB, you are advised to set this parameter to **1000**. You can dynamically restore the setting of this parameter after data import is complete and the WAL synchronization is proper.

wal_sender_timeout
------------------

**Parameter description:** Specifies the maximum duration that the sending server waits for the WAL reception in the receiver.

**Type**: SIGHUP

.. important::

   -  If the primary server contains a huge volume of data, increase the value of this parameter for database rebuilding.
   -  This parameter cannot be set to a value larger than the value of **wal_receiver_timeout** or the timeout parameter for database rebuilding.

**Value range**: an integer ranging from 0 to INT_MAX. The unit is millisecond (ms).

**Default value**: **15s**

max_replication_slots
---------------------

**Parameter description**: Specifies the number of log replication slots on the primary server.

**Type**: POSTMASTER

**Value range**: an integer ranging from 0 to 262143

**Default value**: **8**

A physical replication slot provides an automatic method to ensure that an Xlog is not removed from a primary DN before all the standby and secondary DNs receive it. Physical replication slots are used to support HA clusters. The number of physical replication slots required by a cluster is as follows: ratio of standby and secondary DNs to the primary DN in a ring of DNs. For example, if an HA cluster has 1 primary DN, 1 standby DN, and 1 secondary DN, the number of required physical replication slots will be 2.

Plan the number of logical replication slots as follows:

-  A logical replication slot can carry changes of only one database for decoding. If multiple databases are involved, create multiple logical replication slots.
-  If logical replication is needed by multiple target databases, create multiple logical replication slots in the source database. Each logical replication slot corresponds to one logical replication link.

max_build_io_limit
------------------

**Parameter description:** Specifies the data volume that can be read from the disk per second when the primary server provides a build session to the standby server.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 1048576. The unit is KB. The value **0** indicates that the I/O flow is not restricted when the primary server provides a build session to the standby server.

**Default value**: **0**

**Setting suggestions:** Set this parameter based on the disk bandwidth and job model. If there is no flow restriction or job interference, for disks with good performance such as SSDs, a full build consumes a relatively small proportion of bandwidth and has little impact on service performance. In this case, you do not need to set the threshold. If the service performance of a common 10,000 rpm SAS disk deteriorates significantly during a build, you are advised to set the parameter to 20 MB.

This setting directly affects the build speed and completion time. Therefore, you are advised to set this parameter to a value larger than 10 MB. During off-peak hours, you are advised to remove the flow restriction to restore to the normal build speed.

.. note::

   -  This parameter is used during peak hours or when the disk I/O pressure of the primary server is high. It limits the build flow rate on the standby server to reduce the impact on primary server services. After the service peak hours, you can remove the restriction or reset the flow rate threshold.
   -  You are advised to set a proper threshold based on service scenarios and disk performance.
