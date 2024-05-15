:original_name: dws_06_0162.html

.. _dws_06_0162:

CREATE FOREIGN TABLE (SQL on other GaussDB(DWS))
================================================

Function
--------

In the current database, **CREATE FOREIGN TABLE** creates a foreign table for collaborative analysis. The foreign table is used to access tables stored in other databases for collaborative analysis.

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

      CREATE SERVER server_remote FOREIGN DATA WRAPPER GC_FDW OPTIONS (address '10.10.0.100:25000,10.10.0.101:25000',dbname 'test', username 'test', password '{Password}');

   .. note::

      -  The IP addresses and port numbers of associated CNs are specified in **OPTIONS**. You are advised to set this parameter to an LVS address or multiple CN addresses.

#. Create a foreign table named **region**.

   ::

      DROP FOREIGN TABLE IF EXISTS region;
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

   |image1|

Helpful Links
-------------

:ref:`DROP FOREIGN TABLE <dws_06_0192>`, :ref:`ALTER FOREIGN TABLE (SQL on other GaussDB(DWS)) <dws_06_0125>`

.. |image1| image:: /_static/images/en-us_image_0000001860372785.png
