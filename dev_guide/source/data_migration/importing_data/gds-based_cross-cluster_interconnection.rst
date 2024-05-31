:original_name: dws_04_0967.html

.. _dws_04_0967:

GDS-based Cross-Cluster Interconnection
=======================================

Function
--------

With data processing based on foreign tables, GDS is used to transfer data and synchronize data between multiple clusters.

Scenarios
---------

-  Data is synchronized from one cluster to another. Full data synchronization and data synchronization based on filter criteria are supported.
-  Currently, only the following syntax is supported.

   -  INSERT INTO *inner table* SELECT... FROM *linked_external_table 1* [WHERE];
   -  INSERT INTO *linked_external_table* SELECT \* FROM *inner table 1* [JOIN *inner table 2* \| WHERE];
   -  SELECT ... FROM *linked_external_table*;

Important Notes
---------------

-  The column name and type of the created foreign table must be the same as those of the corresponding source table, and the tables in the remote cluster must be row-store or column-store.
-  Before running the synchronization statement, ensure that the tables to be synchronized exist in the local and remote clusters.
-  The status of the two clusters is **Normal**.
-  Both clusters must be able to connect to each other using GDS.
-  The database code of both clusters must be the same. Otherwise, an error may be reported or the received data may be garbled characters.
-  The compatible database types specified for both clusters must be the same. Otherwise, an error may be reported or the received data may be garbled characters.
-  Ensure that the user performing table synchronization has the permission to access those tables.
-  Foreign tables for interconnection can be used only for cross-cluster data synchronization. In other scenarios, errors may occur or the operation may be invalid.
-  The foreign tables for interconnection do not support complex column expressions or complex syntax, including **join**, **sort**, **cursor**, **with**, and **set**.
-  SQL statements that are not pushed down cannot use this feature to synchronize data.
-  The explain plan and logical cluster are not supported.
-  Only the simple JDBC mode is supported.
-  If data is synchronized from the local cluster to a remote cluster, only internal table query is supported.
-  The GDS specified by the **syncsrv** option of foreign servers does not support the SSL mode.
-  After data synchronization is complete, only the number of data rows is verified.
-  The maximum number of concurrent services cannot be greater than half of the value of the GDS startup parameter **-t** and cannot be greater than the value of **max_active_statements**. Otherwise, services may fail due to timeout.

Preparations
------------

-  Configure the interconnection between both clusters.
-  Plan and deploy GDS servers. Ensure that all GDS servers can communicate with all nodes in the two clusters. That is, the security group of the GDS servers must allow the corresponding GDS port (for example, 5000) and DWS port (8000 by default) in the inbound direction. For details about how to deploy GDS, see :ref:`Installing, Configuring, and Starting GDS <dws_04_0193>`.

   .. note::

      When starting GDS, you can specify any directory as the data transit directory, for example, **/opt**. An example of the startup command is as follows:

      .. code-block::

         /opt/gds/bin/gds -d /opt -p 192.168.0.2:5000 -H 192.168.0.1/24 -l /opt/gds/bin/gds_log.txt -D -t 2

Procedure
---------

Assume that the table **tbl_remote** in the remote cluster is to be synchronized with the table **tbl_local** in the local cluster and the user performing the synchronization is **user_remote**. Note that the user must have the permission to access the **tbl_remote** table.

#. .. _en-us_topic_0000001188323692__en-us_topic_0000001233761713_li9997151784817:

   Create a server.

   .. code-block::

      CREATE SERVER server_remote FOREIGN DATA WRAPPER GC_FDW OPTIONS(
        address '192.168.178.207:8000',
        dbname 'db_remote',
        username 'user_remote',
        password 'xxxxxxxx',
        syncsrv 'gsfs://192.168.178.129:5000|gsfs://192.168.178.129:5000'
      );

   -  **server_remote** indicates the server name, which is used by the foreign table for interconnection.
   -  **address** indicates the IP address and port number of the CN in the remote cluster. Only one address is allowed.
   -  **dbname** indicates the database name of the remote cluster.
   -  **username** indicates the username used for connecting to the remote cluster. This user cannot be a system administrator.
   -  **password** indicates the password used for connecting to the remote cluster.
   -  **syncsrv** indicates the IP address and port number of the GDS server. If there are multiple addresses, use vertical bars (|) to separate them. The function of **syncsrv** is similar to that of **location** in the GDS foreign table.

      .. note::

         GaussDB(DWS) tests the network connected to the GDS addresses set by **syncsrv**.

         -  The test can only show the network status between the local cluster and the GDSs, but cannot show the network status between the remote cluster and GDS. You need to check the error message.
         -  After removing the unavailable GDSs, select a proper number of GDSs that do not cause service suspension to synchronize data.

#. Create a foreign table for interconnection.

   .. code-block::

      CREATE FOREIGN TABLE ft_tbl(
        col_1    type_name,
        col_2    type_name,
        ...
      ) SERVER server_remote OPTIONS (
        schema_name 'schema_remote',
        table_name 'tbl_remote',
        encoding 'utf8'
      );

   -  **schema_name** indicates the schema that the remote cluster table belongs to. If this option is not specified, **schema_name** is set to the schema of the foreign table.
   -  **table_name** indicates the remote cluster table name. If this option is not specified, **table_name** is set to the name of the foreign table.
   -  **encoding** indicates the encoding format of the remote cluster. If this option is not specified, the default encoding format of the source cluster database is used.

   .. note::

      -  The values of **schema_name** and **table_name** are case sensitive and must be the same as those of the remote schema and table.
      -  The foreign table for interconnection cannot contain any constraints in its columns.
      -  The column names and column types of the foreign table must be the same as those of the **tbl_remote** table.
      -  **SERVER** must be set to the server created in :ref:`Step 1 <en-us_topic_0000001188323692__en-us_topic_0000001233761713_li9997151784817>` and must contain the **syncsrv** attribute.

#. Use the foreign table for interconnection to synchronize data.

   -  If the local cluster is the destination cluster, you can run the following statements:

      Full data synchronization of all columns:

      ::

         INSERT INTO tbl_local SELECT * FROM ft_tbl;

      Data synchronization of all columns based on filter criteria:

      ::

         INSERT INTO tbl_local SELECT * FROM ft_tbl WHERE col_2 = XX;

      Full data synchronization of some columns:

      ::

         INSERT INTO tbl_local (col_1) SELECT col_1 FROM ft_tbl;

      Data synchronization of some columns based on filter criteria:

      ::

         INSERT INTO tbl_local (col_1) SELECT col_1 FROM ft_tbl WHERE col_2 = XX;

   -  If the local cluster is the source cluster, you can run the following statements:

      Synchronization of unsharded tables:

      ::

         INSERT INTO ft_tbl SELECT * FROM tbl_local;

      Data synchronization of the **join** results:

      ::

         INSERT INTO ft_tbl SELECT * FROM tbl_local1 join tbl_local2 ON XXX;

   .. note::

      -  If a connection failure is reported, check the server information and ensure that both clusters are connected.
      -  If an error indicating GDS connection failure is reported, check whether the GDS server specified by **syncsrv** has been started and whether it can communicate with all nodes in both clusters.
      -  If an error is reported indicating that the table does not exist, check whether the **option** information of the foreign table is correct.
      -  If an error is reported indicating that the column does not exist, check whether the column name of the foreign table is the same as that of the source table.
      -  If an error message is displayed indicating that a column is repeatedly defined, check whether the column name is too long. If yes, use the AS alias to simplify the column name.
      -  If an error is reported indicating that the column type cannot be parsed, check whether the statement contains a column expression.
      -  If a column mismatch error is reported, check whether the column information of the foreign table is the same as that of the corresponding table in the remote cluster.
      -  If an error is reported indicating that the syntax is not supported, check whether complex syntax is used, such as **join**, **distinct**, and **sort** .
      -  If garbled characters are displayed, check whether the encoding formats of both databases are the same.
      -  If the local cluster is the source cluster, there is a low probability that data is successfully synchronized to the remote cluster but the local cluster returns an execution failure. In this case, you are advised to check the number of synchronized data records.
      -  If the local cluster is the source cluster, data synchronization controlled by transaction blocks and sub-transactions can be queried only after the total transaction is committed.

#. Delete the foreign table for interconnection.

   .. code-block::

      DROP FOREIGN TABLE ft_tbl;
