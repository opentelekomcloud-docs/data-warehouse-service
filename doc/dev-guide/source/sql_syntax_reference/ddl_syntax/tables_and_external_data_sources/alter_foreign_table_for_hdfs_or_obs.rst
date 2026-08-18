:original_name: dws_06_0124.html

.. _dws_06_0124:

ALTER FOREIGN TABLE (for HDFS or OBS)
=====================================

Function
--------

Modifies an HDFS or OBS foreign table.

A foreign table is a virtual table within a database or big data platform that does not physically store data. Instead, it serves as a mapping to data residing in external storage systems, such as HDFS or OBS. This mapping is established using metadata, which includes the table structure, the location of the data, and its file format. It enables users to directly access or operate external data using standard SQL statements. A foreign table serves as a data proxy that logically maps data in external file systems to table structures that can be identified by databases.

Foreign tables are designed to break data silos. Foreign tables allow database users to access external system data in a familiar way through metadata mapping and export data to external systems. In this way, data can be efficiently collaborated, analyzed, and shared across systems, especially in big data scenarios, significantly reducing data movement costs and improving collaboration efficiency.

Precautions
-----------

None

Syntax
------

-  Set the attributes of a foreign table.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ]  table_name
          OPTIONS ( {[ ADD | SET | DROP ] option ['value']} [, ... ]);

-  Set the owner of the foreign table.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          OWNER TO new_owner;

-  Update a foreign table column.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] table_name
          MODIFY ( { column_name data_type | column_name [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ] | column_name [ CONSTRAINT constraint_name ] NULL } [, ...] );

-  Modify the columns of a foreign table.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          action [, ... ];

   The **action** syntax is as follows:

   ::

      ALTER [ COLUMN ] column_name [ SET DATA ] TYPE data_type
         | ALTER [ COLUMN ] column_name { SET | DROP } NOT NULL
         | ALTER [ COLUMN ] column_name SET STATISTICS [PERCENT] integer
         | ALTER [ COLUMN ] column_name OPTIONS ( {[ ADD | SET | DROP ] option ['value'] } [, ... ])
         | MODIFY column_name data_type
         | MODIFY column_name [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ]
         | MODIFY column_name [ CONSTRAINT constraint_name ] NULL

   For details, see :ref:`ALTER TABLE <dws_06_0142>`.

-  Add an informational constraint for a foreign table.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          ADD [ CONSTRAINT constraint_name ]
          { PRIMARY KEY | UNIQUE } ( column_name )
          [ NOT ENFORCED [ ENABLE QUERY OPTIMIZATION | DISABLE QUERY OPTIMIZATION ] | ENFORCED ];

   For details about the parameters for adding an informational constraint, see :ref:`Parameter Description <en-us_topic_0000001811634745__s755e54aa01f04a4bb44806bedcebdab4>`.

-  Remove a foreign table informational constraint.

   ::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
          DROP CONSTRAINT constraint_name ;

-  Set the row-level access control switch for foreign tables (supported by cluster versions 9.1.0 and later).

   .. code-block::

      ALTER FOREIGN TABLE [ IF EXISTS ] tablename
              ENABLE | DISABLE | FORCE | NO FORCE ROW LEVEL SECURITY

Parameter Description
---------------------

.. table:: **Table 1** ALTER FOREIGN TABLE (For HDFS or OBS) parameters

   +-----------------+------------------------------------------------------------------------------------+-------------------------------------------+
   | Parameter       | Description                                                                        | Value Range                               |
   +=================+====================================================================================+===========================================+
   | IF EXISTS       | Sends a message instead of an error if the specified foreign table does not exist. | ``-``                                     |
   +-----------------+------------------------------------------------------------------------------------+-------------------------------------------+
   | tablename       | Specifies the name of an existing foreign table to be modified.                    | Name of an existing foreign table.        |
   +-----------------+------------------------------------------------------------------------------------+-------------------------------------------+
   | new_owner       | Specifies the new owner of the foreign table.                                      | A string that indicates a valid username. |
   +-----------------+------------------------------------------------------------------------------------+-------------------------------------------+
   | data_type       | Specifies the new data type of an existing column.                                 | A string.                                 |
   +-----------------+------------------------------------------------------------------------------------+-------------------------------------------+
   | constraint_name | Specifies the name of a constraint to be added or deleted.                         | A string.                                 |
   +-----------------+------------------------------------------------------------------------------------+-------------------------------------------+
   | column_name     | Specifies an existing column name.                                                 | A string.                                 |
   +-----------------+------------------------------------------------------------------------------------+-------------------------------------------+

For how to modify other parameters in a foreign table, see :ref:`Parameter Description <en-us_topic_0000001811634545__s3e87132692794964b56e3ba420e7b544>` in **ALTER TABLE**.

Examples
--------

#. Create **HDFS_Server** and set the corresponding foreign data wrapper to **HDFS_FDW** or **DFS_FDW**.

   ::

      CREATE SERVER hdfs_server FOREIGN DATA WRAPPER HDFS_FDW OPTIONS (address '10.10.0.100:25000,10.10.0.101:25000',hdfscfgpath '/opt/hadoop_client/HDFS/hadoop/etc/hadoop',type'HDFS');

#. Create an HDFS foreign table that does not contain partition columns.

   ::

      DROP FOREIGN TABLE IF EXISTS ft_region;
      CREATE FOREIGN TABLE ft_region
      (
          R_REGIONKEY INT4,
          R_NAME TEXT,
          R_COMMENT TEXT
      )
      SERVER
          hdfs_server
      OPTIONS
      (
          FORMAT 'orc',
          encoding 'utf8',
          FOLDERNAME '/user/hive/warehouse/gauss.db/region_orc11_64stripe/'
      )
      DISTRIBUTE BY
           roundrobin;

#. Change the type of the **r_name** column in the foreign table **ft_region** to **text**.

   ::

      ALTER FOREIGN TABLE ft_region ALTER r_name TYPE TEXT;

#. Mark the **r_name** column of the foreign table **ft_region** as **not null**.

   ::

      ALTER FOREIGN TABLE ft_region ALTER r_name SET NOT NULL;

Helpful Links
-------------

:ref:`CREATE FOREIGN TABLE (SQL on OBS or Hadoop) <dws_06_0161>` and :ref:`DROP FOREIGN TABLE <dws_06_0192>`
