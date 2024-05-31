:original_name: dws_04_0949.html

.. _dws_04_0949:

Importing Data from One GaussDB(DWS) Cluster to Another
=======================================================

Function
--------

You can create foreign tables to perform associated queries and import data between clusters.

Scenarios
---------

-  Import data from one GaussDB(DWS) cluster to another.
-  Perform associated queries between clusters.

Precautions
-----------

-  The two clusters must be in the same region and AZ, and can communicate with each other through the VPC network.
-  The created foreign table must be of the same type and have the same columns as its corresponding remote table, which can only be a row-store, column-store, hash, or replication table.
-  If the associated table in another cluster is a replication table or has data skew, the query performance may be poor.
-  The status of the two clusters is **Normal**.
-  Do not modify, add, or delete the DDL of the source data table in the remote cluster. Otherwise, the query results may be inconsistent.
-  The two clusters can process SQL on other GaussDB databases based on a foreign table.
-  Ensure that the two databases have the same encoding. Otherwise, an error may occur or the received data may be garbled characters.
-  If statistics have been collected on the remote table, run **ANALYZE** on the foreign table to obtain a better execution plan.
-  Only 8.0.0 and later versions are supported.

Procedure
---------

#. Create a server.

   ::

      CREATE SERVER server_remote FOREIGN DATA WRAPPER GC_FDW OPTIONS
         (address '10.180.157.231:8000,10.180.157.130:8000' ,
        dbname 'gaussdb',
        username 'xyz',
        password 'xxxxxx'
      );

   .. note::

      -  **server_remote** is the server name used for the foreign table.
      -  **address** indicates the IP addresses and port numbers of CNs in the remote cluster. If LVS is configured, you are advised to enter only one LVS address. Otherwise, you are advised to set multiple CNs as server addresses.
      -  **dbname** is the database name of the remote cluster.
      -  **username** is the username used for connecting to the remote cluster. This user cannot be a system administrator.
      -  **password** is the password used for logging in to the remote cluster.

#. Create a foreign table.

   ::

      CREATE FOREIGN TABLE region
      (
          R_REGIONKEY INT4,
          R_NAME TEXT,
          R_COMMENT TEXT
      )
      SERVER
          server_remote
      OPTIONS
      (
          schema_name 'test',
          table_name 'region',
          encoding 'gbk'
      );

   .. note::

      -  Foreign table columns cannot contain any constraints.
      -  The column names types of the foreign table must be the same as those of its corresponding remote table.
      -  **schema_name** specifies the schema of the foreign table corresponding to the remote cluster. If this parameter is not specified, the default schema is used.
      -  **table_name** specifies the name of the foreign table corresponding to the remote cluster. If this parameter is not specified, the default foreign table name is used.
      -  **encoding** specifies the encoding format of the remote cluster. If this parameter is not specified, the default encoding format is used.

#. View the foreign table.

   ::

      \d+ region

                                    Foreign table "public.region"
         Column    |  Type   | Modifiers | FDW Options | Storage  | Stats target | Description
      -------------+---------+-----------+-------------+----------+--------------+-------------
       r_regionkey | integer |           |             | plain    |              |
       r_name      | text    |           |             | extended |              |
       r_comment   | text    |           |             | extended |              |
      Server: server_remote
      FDW Options: (schema_name 'test', table_name 'region', encoding 'gbk')
      FDW permition: read only
      Has OIDs: no
      Distribute By: ROUND ROBIN
      Location Nodes: ALL DATANODES

#. Check the created server.

   ::

      \des+ server_remote
                                                                                                                                     List of foreign servers
           Name      |  Owner  | Foreign-data wrapper | Access privileges | Type | Version |
                        FDW Options                                                                                    | Description
      ---------------+---------+----------------------+-------------------+------+---------+-----------------------------------------------------------------
      -----------------------------------------------------------------------------------------------------------------+-------------
       server_remote | dbadmin | gc_fdw               |                   |      |         | (address '10.180.157.231:8000,10.180.157.130:8000', dbname 'gaussdb'
      , username 'xyz', password 'xxxxxx') |
      (1 row)

#. Use the foreign table to import data or perform associated queries.

   -  Import data.

      ::

         CREATE TABLE local_region
         (
             R_REGIONKEY INT4,
             R_NAME TEXT,
             R_COMMENT TEXT
         );
         INSERT INTO local_region SELECT * FROM region;

      .. note::

         -  If a connection failure is reported, check the server information and ensure that the specified clusters are connected.
         -  If an error is reported, indicating that the table does not exist, check whether the **option** information of the foreign table is correct.
         -  If a column mismatch error is reported, check whether the column information of the foreign table is consistent with that of the corresponding table in the remote cluster.
         -  If a version inconsistency error is reported, upgrade the cluster and try again.
         -  If garbled characters are displayed, check the encoding format of the source data, re-create a foreign table, and specify the correct coding format.

   -  Perform an associated query.

      ::

         SELECT * FROM region, local_region WHERE local_region.R_NAME = region.R_NAME;

      .. note::

         -  A foreign table can be used as a local table to perform complex jobs.
         -  If statistics have been collected on the remote cluster, run **ANALYZE** on the foreign table to obtain a better execution plan.
         -  If there are fewer DNs in the local cluster than in the remote cluster, the local cluster needs to use SMP for better performance.

#. Delete the foreign table.

   ::

      DROP FOREIGN TABLE region;
