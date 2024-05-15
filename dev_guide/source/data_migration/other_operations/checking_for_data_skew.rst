:original_name: dws_04_0228.html

.. _dws_04_0228:

Checking for Data Skew
======================

Scenarios
---------

Data skew causes the query performance to deteriorate. Before importing all the data from a table consisting of over 10 million records, you are advised to import some of the data and check whether data skew occurs and whether the distribution keys need to be changed. Troubleshoot the problems if any. It is costly to address data skew and change the distribution keys after a large amount of data has been imported.

Context
-------

GaussDB(DWS) uses a massively parallel processing (MPP) system of the shared-nothing architecture. The MPP performs horizontal partitioning to store tuples in service data tables on all DNs using proper distribution policies.

The following user table distribution policies are supported:

-  Replication: stores a full table on each DN. You are advised to use the replication mode for tables containing a small volume of data.
-  Hash: A distribution key must be specified for a user table. When a record is inserted, the system hashes it based on the distribution key and then stores it on the corresponding DN. You are advised to use the hash distribution policy for tables with a large volume of data.
-  Round-robin: Each row in the table is sent to each DN in turn. Therefore, data is evenly distributed on each DN. If no proper distribution column can be found in a table with a large amount of data in hash mode, you are advised to use the round-robin distribution policy.

If an inappropriate distribution key is used, data skew may occur when you use the hash policy. Check for data skew when you use the hash distribution policy so that data can be evenly distributed to each DN. You are advised to use the column with few replicated values as the distribution key.

Procedure
---------

#. .. _en-us_topic_0000001233681759__l1ee01c1fa7dd4411b64ee6dfd8052b9a:

   Analyze data source features and select candidate distribution columns that have more distinct values and evenly distributed data.

#. .. _en-us_topic_0000001233681759__l281215ae79184a889db073ca60eef8f2:

   Select a candidate column from :ref:`1 <en-us_topic_0000001233681759__l1ee01c1fa7dd4411b64ee6dfd8052b9a>` to create a target table.

   ::

      CREATE [ [ GLOBAL | LOCAL ] { TEMPORARY | TEMP } | UNLOGGED ] TABLE [ IF NOT EXISTS ] table_name
          ({ column_name data_type [ compress_mode ] [ COLLATE collation ] [ column_constraint [ ... ] ]
          | table_constraint    | LIKE source_table [ like_option [...] ] }
          [, ... ])    [ WITH ( {storage_parameter = value} [, ... ] ) ]
          [ ON COMMIT { PRESERVE ROWS | DELETE ROWS | DROP } ]
          [ COMPRESS | NOCOMPRESS ]    [ TABLESPACE tablespace_name ]
          [ DISTRIBUTE BY { REPLICATION
                          | ROUNDROBIN
                          | { HASH ( column_name [,...] ) } } ];

#. Import a small batch of data to the target table.

   When importing a single data file, you can evenly split this file and import a part of it to check for the data skew in the target table.

#. Check for data skew. (Replace *table_name* with the actual table name.)

   ::

      SELECT a.count,b.node_name FROM (SELECT count(*) AS count,xc_node_id FROM table_name GROUP BY xc_node_id) a, pgxc_node b WHERE a.xc_node_id=b.node_id ORDER BY a.count desc;

#. .. _en-us_topic_0000001233681759__lb6b04bdf47b3436cb325e8cf06d3c55e:

   If the data distribution deviation is less than 10% across DNs, data is evenly distributed and an appropriate distribution key has been selected. Delete the small batch of imported data and import full data to complete data migration.

   If data distribution deviation across DNs is greater than or equal to 10%, data skew occurs. Remove this distribution key from the candidates in :ref:`1 <en-us_topic_0000001233681759__l1ee01c1fa7dd4411b64ee6dfd8052b9a>`, delete the target table, and repeat :ref:`2 <en-us_topic_0000001233681759__l281215ae79184a889db073ca60eef8f2>` through :ref:`5 <en-us_topic_0000001233681759__lb6b04bdf47b3436cb325e8cf06d3c55e>`.

   .. note::

      The data distribution deviation indicates the difference between the actual data volume on DNs and the average data volume on DNs. You can view the distribution difference in the :ref:`PGXC_GET_TABLE_SKEWNESS <dws_04_0805>` view.

#. (Optional) If you fail to select an appropriate distribution key after performing the preceding steps, select multiple columns from the candidates as distribution keys.

Examples
--------

Assume you want to select an appropriate distribution key for the **staffs** table.

#. Analyze the source data for the **staffs** table and select the **staff_ID**, **FIRST_NAME**, and **LAST_NAME** columns as candidate distribution keys.

#. Select the **staff_ID** column as the distribution key and create the target table **staffs**.

   ::

      CREATE TABLE staffs
      (
        staff_ID       NUMBER(6) not null,
        FIRST_NAME     VARCHAR2(20),
        LAST_NAME      VARCHAR2(25),
        EMAIL          VARCHAR2(25),
        PHONE_NUMBER   VARCHAR2(20),
        HIRE_DATE      DATE,
        employment_ID  VARCHAR2(10),
        SALARY         NUMBER(8,2),
        COMMISSION_PCT NUMBER(2,2),
        MANAGER_ID     NUMBER(6),
        section_ID     NUMBER(4)
      )
      DISTRIBUTE BY hash(staff_ID);

#. Import a small batch of data to the target table **staffs**.

   There are eight DNs in the cluster based on the following query, and you are advised to import 80,000 records.

   ::

      SELECT count(*) FROM pgxc_node where node_type='D';
       count
      -------
           8
      (1 row)

#. Verify the data skew of the target table **staffs** whose distribution key is **staff_ID**:

   ::

      SELECT a.count,b.node_name FROM (select count(*) as count,xc_node_id FROM staffs GROUP BY xc_node_id) a, pgxc_node b WHERE a.xc_node_id=b.node_id ORDER BY a.count desc;
      count | node_name
      ------+-----------
      11010 | datanode4
      10000 | datanode3
      12001 | datanode2
       8995 | datanode1
      10000 | datanode5
       7999 | datanode6
       9995 | datanode7
      10000 | datanode8
      (8 rows)

#. The preceding query result indicates that the distribution deviation across DNs is greater than 10%. The data skew occurs. Therefore, delete **staff_ID** from the distribution key candidates and delete the **staffs** table.

   ::

      DROP TABLE staffs;

#. Use **staff_ID**, **FIRST_NAME**, and **LAST_NAME** as distribution keys and create the target table **staffs**.

   ::

      CREATE TABLE staffs
      (
        staff_ID       NUMBER(6) not null,
        FIRST_NAME     VARCHAR2(20),
        LAST_NAME      VARCHAR2(25),
        EMAIL          VARCHAR2(25),
        PHONE_NUMBER   VARCHAR2(20),
        HIRE_DATE      DATE,
        employment_ID  VARCHAR2(10),
        SALARY         NUMBER(8,2),
        COMMISSION_PCT NUMBER(2,2),
        MANAGER_ID     NUMBER(6),
        section_ID     NUMBER(4)
      )
      DISTRIBUTE BY hash(staff_ID,FIRST_NAME,LAST_NAME);

#. Verify the data skew of the target table **staffs** whose distribution keys are **staff_ID**, **FIRST_NAME**, and **LAST_NAME**.

   ::

      SELECT a.count,b.node_name FROM (select count(*) as count,xc_node_id FROM staffs GROUP BY xc_node_id) a, pgxc_node b WHERE a.xc_node_id=b.node_id ORDER BY a.count desc;
      count | node_name
      ------+-----------
      10010 | datanode4
      10000 | datanode3
      10001 | datanode2
       9995 | datanode1
      10000 | datanode5
       9999 | datanode6
       9995 | datanode7
      10000 | datanode8
      (8 rows)

#. The preceding query result indicates that the data deviation across DNs is less than 10%. The data is evenly distributed and the appropriate distribution keys have been selected.

#. Delete the imported small-batch data.

   ::

      TRUNCATE TABLE staffs;

#. Import the full data to complete data migration.
