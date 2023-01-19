:original_name: dws_01_0156.html

.. _dws_01_0156:

Updating the MRS Data Source Configuration
==========================================

Scenario
--------

For MRS, if the following parameter configurations of the HDFS cluster change, data may fail to be imported to the data warehouse cluster from the HDFS cluster. Before importing data using the HDFS cluster, you must update the MRS data source configuration.

+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+
| Parameter                                                 | Description                                                                                                             |
+===========================================================+=========================================================================================================================+
| dfs.client.read.shortcircuit                              | Specifies whether to enable the local read function.                                                                    |
+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+
| dfs.client.read.shortcircuit.skip.checksum                | Specifies whether to skip data verification during the local read.                                                      |
+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+
| dfs.client.block.write.replace-datanode-on-failure.enable | Specifies whether to replace the location storing copies with the new node when data blocks fail to be written to HDFS. |
+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+
| dfs.encrypt.data.transfer                                 | Specifies whether to enable data encryption.                                                                            |
|                                                           |                                                                                                                         |
|                                                           | .. note::                                                                                                               |
|                                                           |                                                                                                                         |
|                                                           |    This parameter is available only for clusters with Kerberos authentication enabled.                                  |
+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+
| dfs.encrypt.data.transfer.algorithm                       | Specifies the encryption and decryption algorithm for key transmission.                                                 |
+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+
| dfs.encrypt.data.transfer.cipher.suites                   | Specifies the encryption and decryption algorithm for the transmission of actually stored data.                         |
+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+
| dfs.replication                                           | Specifies the default number of data copies.                                                                            |
+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+
| dfs.blocksiz                                              | Specifies the default size of a data block.                                                                             |
+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+
| hadoop.security.authentication                            | Specifies the security authentication mode.                                                                             |
+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+
| hadoop.rpc.protection                                     | Specifies the RPC communication protection mode.                                                                        |
+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+
| dfs.domain.socket.path                                    | Specifies the locally used **Domain socket** path.                                                                      |
+-----------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------+

Prerequisites
-------------

You have created an MRS data source connection for the data warehouse cluster.

Impact on the System
--------------------

When you are updating an MRS data source connection, the data warehouse cluster will automatically restart and cannot provide services.

Procedure
---------

#. On the GaussDB(DWS) management console, click **Clusters**.

#. In the cluster list, click the name of a cluster. On the page that is displayed, click **MRS Data Sources**.

#. In the MRS data source list, select the MRS data source that you want to update. In the **Operation** column, click **Update Configurations**.

   **MRS Cluster Status** and **Configuration Status** of the current connection will be updated. During configuration update, you cannot create a connection. The system checks whether the security group rule is correct. If the rule is incorrect, the system rectifies the fault.
