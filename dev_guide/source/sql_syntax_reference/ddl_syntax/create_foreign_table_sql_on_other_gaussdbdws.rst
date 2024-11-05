:original_name: dws_06_0162.html

.. _dws_06_0162:

CREATE FOREIGN TABLE (SQL on other GaussDB(DWS))
================================================

Function
--------

Creates a foreign table for collaborative analysis in the current database. The foreign table is used to access tables stored in other databases for collaborative analysis.

The foreign table is read-only. It can only be queried using **SELECT**.

Syntax
------

::

   CREATE FOREIGN TABLE [ IF NOT EXISTS ] table_name
   ( [ { column_name type_name | LIKE source_table } [, ...] ] )
   SERVER server_name
   OPTIONS ( { option_name ' value ' } [, ...] )
   [ READ ONLY ]
   [ DISTRIBUTE BY {ROUNDROBIN} ]
   [ TO { GROUP groupname | NODE ( nodename [, ... ] ) } ];

Parameter Description
---------------------

-  **IF NOT EXISTS**

   Does not throw an error if a table with the same name exists. A notice is issued in this case.

-  **table_name**

   Specifies the name of the foreign table to be created.

   Value range: a string. It must comply with the naming convention.

-  **column_name**

   Specifies the name of a column in the foreign table. Columns are separated by commas (,).

   Value range: a string. It must comply with the naming convention.

   .. note::

      Constraints or indexes cannot be created on columns.

-  **type_name**

   Specifies the data type of the column.

   .. note::

      Sequence and custom types are not allowed.

-  **SERVER** server_name

   Specifies the server name, which is user-definable.

   Value range: a string indicating an existing server. It must comply with the naming convention.

-  **OPTIONS ( { option_name ' value ' } [, ...] )**

   Specifies the following parameters for a foreign table:

   -  **table_name**: table name of the associated cluster. If it is omitted, the foreign table name will be used.

   -  **schema_name**: schema of the associated cluster. If it is omitted, the schema of the foreign table will be used.

   -  **encoding**: encoding set of the associated cluster. If it is omitted, the database encoding set of the associated cluster will be used.

   -  **dataencoding**: used to synchronize GBK data and UTF8 data between two latin1 databases during GDS interconnection. This parameter is supported by version 8.2.0 or later clusters.

      When this parameter is set, **encoding** indicates the actual encoding of data in the remote cluster, and **dataencoding** indicates the actual encoding of data in the local cluster.

   .. note::

      Assume that the database in the local cluster is **latin1_local_db** and that in the remote cluster is **latin1_remote_db**, and **latin1_remote_db** stores GBK data. When migrating GBK data from **latin1_remote_db** to **latin1_local_db** in UTF8 encoding, you need to set the foreign table parameters **dataencoding** to U\ **TF8** and **encoding** to **GBK** in the local cluster.

-  gds_compress

   To reduce the network transmission bandwidth, GDS interconnection provides this parameter for compression. Currently, this parameter can only be set to **snappy**. By default, data is not compressed. This parameter is supported by version 8.2.0 or later clusters.

   Before using this function, ensure that the versions of the local cluster, remote cluster, and GDS are the same.

   .. note::

      **gds_compress** is used together with **file_type**. If the value of **file_type** is **interconn**, GDS must be upgraded to 8.2.0 or later. Otherwise, the error message "**ERROR: un-support format**" is displayed.

-  **READ ONLY**

   Indicates that a table is a read-only foreign table.

-  **DISTRIBUTE BY ROUNDROBIN**

   Specifies **ROUNDROBIN** as the distribution mode for the foreign table.

-  **TO { GROUP groupname \| NODE ( nodename [, ... ] ) }**

   Currently, **TO GROUP** cannot be used. **TO NODE** is used for internal scale-out tools.

Examples
--------

#. Create a foreign server named **server_remote**. The corresponding foreign data wrapper is **GC_FDW**.

   ::

      CREATE SERVER server_remote FOREIGN DATA WRAPPER GC_FDW OPTIONS (address '10.10.0.100:25000,10.10.0.101:25000',dbname 'test', username 'test', password '{password}');

   .. note::

      -  The IP addresses and port numbers of associated CNs are specified in **OPTIONS**. You are advised to set this parameter to an LVS address or multiple CN addresses.

#. Create a foreign table named **region**.

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

#. View the created **region** foreign table.

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
      FDW permission: read only
      Has OIDs: no
      Distribute By: ROUND ROBIN
      Location Nodes: ALL DATANODES

Helpful Links
-------------

:ref:`DROP FOREIGN TABLE <dws_06_0192>`, :ref:`ALTER FOREIGN TABLE (SQL on other GaussDB(DWS)) <dws_06_0125>`
