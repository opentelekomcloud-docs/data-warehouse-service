:original_name: dws_01_1071.html

.. _dws_01_1071:

Configuring a Snapshot
======================

You can configure the parameters for creating and restoring a snapshot.

.. note::

   -  This feature applies only to clusters of 8.2.0 or later. (For clusters of versions earlier than 8.2.0, only some parameters can be configured.)
   -  The parameters take effect on all the snapshot creation and restoration tasks.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of the target cluster. The **Cluster Information** page is displayed.

#. Click the **Snapshots** tab page and click **Configure Parameters**. All the configurable parameters of the current cluster will be displayed.

#. Configure parameters as required. For details, see :ref:`Table 1 <en-us_topic_0000001658895274__en-us_topic_0000001478356381_table16486142263310>`.

   |image1|

#. Click **Save**.

Snapshot parameters
-------------------

.. _en-us_topic_0000001658895274__en-us_topic_0000001478356381_table16486142263310:

.. table:: **Table 1** Snapshot information

   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | Parameter           | Type                  | Description                                                                                                                                                                         | Default Value                                            |
   +=====================+=======================+=====================================================================================================================================================================================+==========================================================+
   | parallel-process    | Backup parameter      | Number of concurrent processes on each node during Roach backup.                                                                                                                    | The value is the number of DNs on the current node.      |
   |                     |                       |                                                                                                                                                                                     |                                                          |
   |                     |                       | .. note::                                                                                                                                                                           |                                                          |
   |                     |                       |                                                                                                                                                                                     |                                                          |
   |                     |                       |    This parameter can be configured for clusters earlier than 8.2.0.                                                                                                                |                                                          |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | compression-type    | Backup parameter      | Compression algorithm.                                                                                                                                                              | LZ4                                                      |
   |                     |                       |                                                                                                                                                                                     |                                                          |
   |                     |                       | -  zlib                                                                                                                                                                             |                                                          |
   |                     |                       | -  LZ4                                                                                                                                                                              |                                                          |
   |                     |                       |                                                                                                                                                                                     |                                                          |
   |                     |                       | .. note::                                                                                                                                                                           |                                                          |
   |                     |                       |                                                                                                                                                                                     |                                                          |
   |                     |                       |    This parameter can be configured for clusters earlier than 8.2.0.                                                                                                                |                                                          |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | compression-level   | Backup parameter      | Compression level. The value range is 0 to 9.                                                                                                                                       | 6                                                        |
   |                     |                       |                                                                                                                                                                                     |                                                          |
   |                     |                       | -  **0**: fast backup and no compression                                                                                                                                            |                                                          |
   |                     |                       | -  **9**: slow backup and maximum compression                                                                                                                                       |                                                          |
   |                     |                       |                                                                                                                                                                                     |                                                          |
   |                     |                       | .. note::                                                                                                                                                                           |                                                          |
   |                     |                       |                                                                                                                                                                                     |                                                          |
   |                     |                       |    This parameter can be configured for clusters earlier than 8.2.0.                                                                                                                |                                                          |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | buffer-size         | Backup parameter      | Buffer size of the Roach upload media. The value range is 256 to 16,384, in MB.                                                                                                     | 256                                                      |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | buffer-block-size   | Backup parameter      | Data block size of the data file to be read by Roach. The value range is 5,242,880 to 268,435,456, in bytes.                                                                        | 67108864                                                 |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | cpu-cores           | Backup parameter      | Number of CPU cores that can be used when Roach starts multiple threads concurrently                                                                                                | 1/2 of the total number of logical CPU cores on the node |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | master-timeout      | Backup parameter      | Timeout period for the communication between the Roach master and agent nodes. The value range is 600 to 3600, in seconds.                                                          | 3600                                                     |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | max-backup-io-speed | Backup parameter      | I/O flow control during Roach backup. The value range is 0 to 2048, in MB/s. The value must be greater than the value of **buffer-block-size**. The value **0** indicates no limit. | 0                                                        |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | backup-mode         | Backup parameter      | Full backup mode.                                                                                                                                                                   | 0                                                        |
   |                     |                       |                                                                                                                                                                                     |                                                          |
   |                     |                       | -  **0**: phase-1 backup                                                                                                                                                            |                                                          |
   |                     |                       | -  **1**: phase-2 backup                                                                                                                                                            |                                                          |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | cbm-parse-mode      | Backup parameter      | Incremental backup mode.                                                                                                                                                            | 0                                                        |
   |                     |                       |                                                                                                                                                                                     |                                                          |
   |                     |                       | -  **0**: one-time CBM scan (high memory usage and high performance)                                                                                                                |                                                          |
   |                     |                       | -  **1**: multiple CBM scans (stable memory usage and low performance)                                                                                                              |                                                          |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | parallel-process    | Restoration parameter | Number of concurrent processes on each node during Roach backup. By default, the value is the number of primary DNs on the current node plus 1.                                     | 1                                                        |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | cpu-cores           | Restoration parameter | Number of CPU cores that can be used when Roach starts multiple threads concurrently                                                                                                | The default value is 1/2 of the number of CPU cores.     |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+
   | logging-level       | Restoration parameter | Log levels:                                                                                                                                                                         | INFO                                                     |
   |                     |                       |                                                                                                                                                                                     |                                                          |
   |                     |                       | -  **FATAL**: Unrecoverable faults that cause the system suspension. This is the most severe level.                                                                                 |                                                          |
   |                     |                       | -  **ERROR**: Major errors.                                                                                                                                                         |                                                          |
   |                     |                       | -  **WARNING**: Exceptions. In this case, the system may continue to process tasks.                                                                                                 |                                                          |
   |                     |                       | -  **INFO**: Notes.                                                                                                                                                                 |                                                          |
   |                     |                       | -  **DEBUG**: Debugging details.                                                                                                                                                    |                                                          |
   |                     |                       | -  **DEBUG2**: Detailed debugging information, which is generally not displayed. This is the least severe level.                                                                    |                                                          |
   +---------------------+-----------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------+

.. |image1| image:: /_static/images/en-us_image_0000001759518501.png
