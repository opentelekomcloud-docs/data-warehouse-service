:original_name: dws_06_0143.html

.. _dws_06_0143:

ALTER TABLE PARTITION
=====================

Function
--------

**ALTER TABLE PARTITION** modifies table partitioning, including adding, deleting, splitting, merging partitions, and modifying partition attributes.

Precautions
-----------

-  The name of the added partition must be different from names of existing partitions in the partitioned table.
-  The partition key of the added partition must be the same type as that of the partitioned table. The key value of the added partition must exceed the upper limit of the last partition range.
-  If the number of partitions in the target partitioned table has reached the maximum (32767), partitions cannot be added.

-  If a partitioned table has only one partition, the partition cannot be deleted.
-  Use **PARTITION FOR()** to choose partitions. The number of specified values in the brackets should be the same as the column number in customized partition, and they must be consistent.
-  The **Value** partitioned table does not support the **Alter Partition** operation.
-  For OBS cold and hot tables:

   -  The tablespace of a partitioned table cannot be set to an OBS tablespace during the **MOVE**, **EXCHANGE**, **MERGE**, and **SPLIT** operations.
   -  When an **ALTER** statement is executed, the cold and hot data attributes in the partitions cannot be changed, that is, data in the cold partition should still be put in the cold partition after a data operation, and hot partition data should be put in the hot partition. Therefore, cold partition data cannot be migrated to the local tablespace.
   -  Only the default tablespace is supported for cold partitions.
   -  Cold and hot partitions cannot be merged.
   -  Cold partition switching is not supported for the **EXCHANGE** operation.

Syntax
------

-  Modify the syntax of the table partition.

   ::

      ALTER TABLE [ IF EXISTS ] { table_name  [*] | ONLY table_name | ONLY ( table_name  )}
          action [, ... ];

   **action** indicates the following clauses for maintaining partitions. For the partition continuity when multiple clauses are used for partition maintenance, GaussDB(DWS) does **DROP PARTITION** and then **ADD PARTITION**, and finally runs the rest clauses in sequence.

   ::

      move_clause  |
          exchange_clause  |
          row_clause  |
          merge_clause  |
          modify_clause  |
          split_clause  |
          add_clause  |
          drop_clause

   -  The **move_clause** syntax is used to move the partition to a new tablespace.

      ::

         MOVE PARTITION { partition_name | FOR ( partition_value [, ...] ) } TABLESPACE tablespacename

   -  The **exchange_clause** syntax is used to move the data from a general table to a specified partition.

      ::

         EXCHANGE PARTITION { ( partition_name ) | FOR ( partition_value [, ...] ) }
             WITH TABLE {[ ONLY ] ordinary_table_name | ordinary_table_name * | ONLY ( ordinary_table_name )}
             [ { WITH | WITHOUT } VALIDATION ] [ VERBOSE ]

      The ordinary table and the partitioned table whose data is to be exchanged must meet the following requirements:

      -  The number of columns of the ordinary table is the same as that of the partitioned table, and their information should be consistent, including the column name, data type, constraint, collation, storage parameter, compression, and data type of a deleted column.
      -  The compression information of the ordinary table and partitioned table should be consistent.
      -  The distribution column information of the ordinary table and the partitioned table should be consistent.
      -  The number and information of indexes of the ordinary table and the partitioned table should be consistent.
      -  The number and information of constraints of the ordinary table and the partitioned table should be consistent.
      -  The ordinary table cannot be a temporary table or unlogged table.
      -  The ordinary table and the partitioned table must be in the same logical cluster or node group.
      -  If other columns following the last valid column in the partitioned table are deleted and the deleted columns are not considered, the partitioned table can be exchanged with the ordinary table as long as the columns of the two tables are the same.
      -  The table-level parameter **colversion** of a column-store ordinary table must be the same as that of a column-store partitioned table. Partition swap between colversion2.0 and colversion1.0 is not allowed.

      When the execution is complete, the data and tablespace of the ordinary table and the partitioned table are exchanged. In this case, statistics about the ordinary table and the partitioned table become unreliable. Both tables should be analyzed again.

   -  The syntax of **row_clause** is used to set the row movement switch of a partitioned table.

      ::

         { ENABLE | DISABLE } ROW MOVEMENT

   -  The **merge_clause** syntax is used to merge partitions into one.

      ::

         MERGE PARTITIONS { partition_name } [, ...] INTO PARTITION partition_name


   -  The syntax of **modify_clause** is used to set whether a partition index is usable.

      ::

         MODIFY PARTITION partition_name { UNUSABLE LOCAL INDEXES | REBUILD UNUSABLE LOCAL INDEXES }

   -  The **split_clause** syntax is used to split one partition into partitions.

      ::

         SPLIT PARTITION { partition_name | FOR ( partition_value [, ...] ) } { split_point_clause | no_split_point_clause }

      -  The syntax of specified **split_point_clause** is as follows:

         ::

            AT ( partition_value ) INTO ( PARTITION partition_name  , PARTITION partition_name  )

         .. important::

            The size of split point should be in the range of splitting partition key. The split point can only split one partition into two.

      -  The syntax of **no_split_point_clause** is as follows:

         ::

            INTO { ( partition_less_than_item [, ...] ) | ( partition_start_end_item [, ...] ) }

         .. important::

            -  The first new partition key specified by **partition_less_than_item** must be larger than that of the former partition (if any), and the last partition key specified by **partition_less_than_item** must be equal to that of the splitting partition.
            -  The start point (if any) of the first new partition specified by **partition_start_end_item** must be equal to the partition key (if any) of the previous partition. The end point (if any) of the last partition specified by **partition_start_end_item** must be equal to the partition key of the splitting partition.
            -  **partition_less_than_item** supports a maximum of four partition keys and **partition_start_end_item** supports only one partition key. For details about the supported data types, see :ref:`Partition Key <en-us_topic_0000001099150744__lb144da954d4c4ac58c1e9ae1391e59ac>`.

      -  The syntax of **partition_less_than_item** is as follows:

         ::

            PARTITION partition_name VALUES LESS THAN ( { partition_value | MAXVALUE }  [, ...] )
                [ TABLESPACE tablespacename ]

      -  The syntax of **partition_start_end_item** is as follows. For details about the constraints, see :ref:`partition_start_end_item syntax <en-us_topic_0000001099150744__li2094151861116>`.

         ::

            PARTITION partition_name {
                    {START(partition_value) END (partition_value) EVERY (interval_value)} |
                    {START(partition_value) END ({partition_value | MAXVALUE})} |
                    {START(partition_value)} |
                    {END({partition_value | MAXVALUE})}
            } [TABLESPACE tablespace_name]

   -  The syntax of **add_clause** is used to add a partition to one or more specified partitioned tables.

      ::

         ADD {partition_less_than_item | partition_start_end_item}

   -  The syntax of **drop_clause** is used to remove a specified partition from a partitioned table.

      ::

         DROP PARTITION  { partition_name | FOR (  partition_value [, ...] )  }

-  The syntax of modifying a table partition name is as follows:

   ::

      ALTER TABLE [ IF EXISTS ] { table_name [*] | ONLY table_name | ONLY ( table_name  )}
          RENAME PARTITION { partition_name | FOR ( partition_value [, ...] ) } TO partition_new_name;

Parameter Description
---------------------

-  **table_name**

   Specifies the name of a partitioned table.

   Value range: an existing partitioned table name

-  **partition_name**

   Specifies the name of a partition.

   Value range: an existing partition name

-  **partition_value**

   Specifies the key value of a partition.

   The value specified by **PARTITION FOR ( partition_value [, ...] )** can uniquely identify a partition.

   Value range: value range of the partition key for the partition to be renamed

-  **UNUSABLE LOCAL INDEXES**

   Sets all the indexes unusable in the partition.

-  **REBUILD UNUSABLE LOCAL INDEXES**

   Rebuilds all the indexes in the partition.

-  **ENABLE/DISABLE ROW MOVEMENT**

   Specifies the row movement switch.

   If the tuple value is updated on the partition key during the **UPDATE** action, the partition where the tuple is located is altered. Setting of this parameter enables error messages to be reported or movement of the tuple between partitions.

   Valid value:

   -  **ENABLE**: The row movement switch is enabled.
   -  **DISABLE**: The row movement switch is disabled.

   The switch is disabled by default.

-  **ordinary_table_name**

   Specifies the name of the ordinary table whose data is to be migrated.

   Value range: an existing ordinary table name

-  **{ WITH \| WITHOUT } VALIDATION**

   Checks whether the ordinary table data meets the specified partition key range of the partition to be migrated.

   Valid value:

   -  **WITH**: checks whether the common table data meets the partition key range of the partition to be exchanged. If any data does not meet the required range, an error is reported.
   -  **WITHOUT**: does not check whether the common table data meets the partition key range of the partition to be exchanged.

   The default value is **WITH**.

   The check is time consuming, especially when the data volume is large. Therefore, use **WITHOUT** when you are sure that the current common table data meets the partition key range of the partition to be exchanged.

-  **VERBOSE**

   When **VALIDATION** is **WITH**, if the ordinary table contains data that is out of the partition key range, insert the data to the correct partition. If there is no correct partition where the data can be route to, an error is reported.

   .. important::

      Only when **VALIDATION** is **WITH**, **VERBOSE** can be specified.

-  **partition_new_name**

   Specifies the new name of a partition.

   Value range: a string. It must comply with the naming convention.

Example
-------

Delete partition **P8**.

::

   ALTER TABLE tpcds.web_returns_p1 DROP PARTITION P8;

Add a partition **WR_RETURNED_DATE_SK** with values ranging from 2453005 to 2453105.

::

   ALTER TABLE tpcds.web_returns_p1 ADD PARTITION P8 VALUES LESS THAN (2453105);

Add a partition **WR_RETURNED_DATE_SK** with values ranging from 2453105 to **MAXVALUE**.

::

   ALTER TABLE tpcds.web_returns_p1 ADD PARTITION P9 VALUES LESS THAN (MAXVALUE);

Rename the **P7** partition as **P10**.

::

   ALTER TABLE tpcds.web_returns_p1 RENAME PARTITION P7 TO P10;

Rename the **P6** partition as **P11**.

::

   ALTER TABLE tpcds.web_returns_p1 RENAME PARTITION FOR (2452639) TO P11;

Query rows in the **P10** partition.

::

   SELECT count(*) FROM tpcds.web_returns_p1 PARTITION (P10);
    count
   --------
    9362
   (1 row)

Split the **P8** partition at 2453010.

::

   ALTER TABLE tpcds.web_returns_p2 SPLIT PARTITION P8 AT (2453010) INTO
   (
           PARTITION P9,
           PARTITION P10
   );

Merge the **P6** and **P7** partitions into one.

::

   ALTER TABLE tpcds.web_returns_p2 MERGE PARTITIONS P6, P7 INTO PARTITION P8;

Modify the migration attribute of a partitioned table.

::

   ALTER TABLE tpcds.web_returns_p2 DISABLE ROW MOVEMENT;

Add partitions [5000, 5300), [5300, 5600), [5600, 5900), and [5900, 6000).

::

   ALTER TABLE tpcds.startend_pt ADD PARTITION p6 START(5000) END(6000) EVERY(300);

Add the partition p7, specified by **MAXVALUE**.

::

   ALTER TABLE tpcds.startend_pt ADD PARTITION p7 END(MAXVALUE);

Rename the partition where 5950 is located to p71.

::

   ALTER TABLE tpcds.startend_pt RENAME PARTITION FOR(5950) TO p71;

Split the partition [4000, 5000) where 4500 is located.

::

   ALTER TABLE tpcds.startend_pt SPLIT PARTITION FOR(4500) INTO(PARTITION q1 START(4000) END(5000) EVERY;

Links
-----

:ref:`CREATE TABLE PARTITION <dws_06_0179>`, :ref:`DROP TABLE <dws_06_0208>`
