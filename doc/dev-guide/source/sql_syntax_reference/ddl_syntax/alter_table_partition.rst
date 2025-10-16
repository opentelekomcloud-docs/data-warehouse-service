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
-  For a range partitioned table, the boundary value of the added partition must be the same type as the partition key of the partitioned table. The key value of the added partition must exceed the upper limit of the last partition.
-  For a list partitioned table, if the DEFAULT partition has been defined, no new partition can be added.
-  Unless otherwise specified, the syntax of range partitioned tables is the same as that of column-store partitioned tables.
-  If the number of partitions in the target partitioned table has reached the maximum (32767), partitions cannot be added.

-  If a partitioned table has only one partition, the partition cannot be deleted.
-  When you run the **DROP PARTITION** command to delete a partition, the data in the partition is also deleted.
-  Use **PARTITION FOR()** to choose partitions. The number of specified values in the brackets should be the same as the column number in customized partition, and they must be consistent.
-  The **Value** partitioned table does not support the **Alter Partition** operation.
-  For OBS hot and cold tables:

   -  They do not support specifying the partition table's tablespace as the OBS tablespace for **MOVE**, **EXCHANGE**, **MERGE**, and **SPLIT** operations.
   -  When an **ALTER** statement is executed, the data in the cold partition should stay in the cold partition, and the data in the hot partition should remain in the hot partition. It is not allowed to move cold partition data to the local tablespace.
   -  Only the default tablespace is supported for cold partitions.
   -  Cold and hot partitions cannot be merged.
   -  Cold partition switching is not supported during the **EXCHANGE** operation.

.. warning::

   -  Avoid performing **ALTER TABLE**, **ALTER TABLE PARTITION**, **DROP PARTITION**, and **TRUNCATE** operations during peak hours to prevent long SQL statements from blocking these operations or SQL services.
   -  For more information about development and design specifications, see "GaussDB(DWS) Development and Design Proposal" in the *GaussDB(DWS) Developer Guide*.

Syntax
------

-  Modify the syntax of the table partition.

   ::

      ALTER TABLE [ IF EXISTS ] { table_name  [*] | ONLY table_name | ONLY ( table_name  )}
          action [, ... ];

   **action** indicates the following clauses for maintaining partitions. For the partition continuity when multiple clauses are used for partition maintenance, GaussDB(DWS) does **DROP PARTITION** and then **ADD PARTITION**, and finally runs the rest clauses in sequence.

   ::

          modify_clause  |
          rebuild_clause |
          exchange_clause  |
          row_clause  |
          merge_clause  |
          modify_clause  |
          split_clause  |
          add_clause  |
          drop_clause  |
          truncate_partitioned_clause

   -  The syntax of **modify_clause** is used to set whether a partition index is usable.

      ::

         MODIFY PARTITION partition_name { UNUSABLE LOCAL INDEXES | REBUILD UNUSABLE LOCAL INDEXES }

   -  The **rebuild_clause** syntax is used to rebuild the index of a partition. This syntax is supported only by clusters of version 8.3.0.100 or later.

      ::

         REBUILD PARTITION partition_name [ WITHOUT UNUSABLE ]

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
      -  To exchange partitions, an ordinary table and a partitioned table need to be in the same logical cluster or node group. Otherwise, data from each table is inserted into the other table, which makes the partition exchange time depend on the table data volume. This can be very slow for large tables and partitioned tables.
      -  In online scale-out and redistribution scenarios, the exchange partition statement may interfere with the redistribution of common tables and partitioned tables (if there are lock conflicts between the partition exchange and redistribution statement). Usually, the redistribution of common tables and partitioned tables is retried twice after being interrupted, but if the same table is exchanged too often, the redistribution may fail multiple times. If the redistribution process of an ordinary table is interrupted by the partition exchange operation, the data has been replaced with the data in the original partition table during the redistribution retry. In this case, full redistribution will be performed again.
      -  If other columns following the last valid column in the partitioned table are deleted and the deleted columns are not considered, the partitioned table can be exchanged with the ordinary table as long as the columns of the two tables are the same.
      -  The table-level parameter **colversion** of a column-store ordinary table must be the same as that of a column-store partitioned table. Partition swap between colversion2.0 and colversion1.0 is not allowed.

      When the execution is complete, the data and tablespace of the ordinary table and the partitioned table are exchanged. In this case, statistics about the ordinary table and the partitioned table become unreliable. Both tables should be analyzed again.

   -  The syntax of **row_clause** is used to set the row movement switch of a partitioned table.

      ::

         { ENABLE | DISABLE } ROW MOVEMENT

   -  The **merge_clause** syntax is used to merge partitions into one.

      ::

         MERGE PARTITIONS { partition_name } [, ...] INTO PARTITION partition_name

      .. important::

         -  The partition before the keyword **INTO** is called the source partition, and the partition after the **INTO** is called the target partition.
         -  The number of source partitions cannot be less than **2**.
         -  The source partition name must be unique.
         -  The source partition cannot have unusable indexes. Otherwise, an error will be reported.
         -  The target partition name must either be the same as the name of the last source partition or different from all partition names of the table.
         -  The boundaries of the target partition are the union of the boundaries of all the source partitions.
         -  For a range partitioned table, all source partitions must have contiguous boundaries.
         -  For list partitioning, if the source partition contains a DEFAULT partition, the boundary of the target partition is also DEFAULT.

   -  The syntax of **modify_clause** is used to set whether a partition index is usable.

      ::

         MODIFY PARTITION partition_name { UNUSABLE LOCAL INDEXES | REBUILD UNUSABLE LOCAL INDEXES }

   -  The **split_clause** syntax is used to split one partition into partitions.

      **The split_clause syntax for range partitioning is as follows:**

      ::

         SPLIT PARTITION { partition_name | FOR ( partition_value [, ...] ) } { split_point_clause | no_split_point_clause }

      -  The syntax of **split_point_clause** is as follows:

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
            -  **partition_less_than_item** supports a maximum of four partition keys and **partition_start_end_item** supports only one partition key. For details about the supported data types, see :ref:`Partition Key <en-us_topic_0000001764675414__lb144da954d4c4ac58c1e9ae1391e59ac>`.
            -  **partition_less_than_item** and **partition_start_end_item** cannot be used in the same statement.

      -  The syntax of **partition_less_than_item** is as follows:

         ::

            PARTITION partition_name VALUES LESS THAN ( { partition_value | MAXVALUE }  [, ...] )


      -  The syntax of **partition_start_end_item** is as follows. For details about the constraints, see :ref:`partition_start_end_item syntax <en-us_topic_0000001764675414__li2094151861116>`.

         ::

            PARTITION partition_name {
                    {START(partition_value) END (partition_value) EVERY (interval_value)} |
                    {START(partition_value) END ({partition_value | MAXVALUE})} |
                    {START(partition_value)} |
                    {END({partition_value | MAXVALUE})}
            }

      **The syntax of split_clause for list partitioning is as follows:**

      ::

         SPLIT PARTITION { partition_name | FOR ( partition_value [, ...] ) } { split_values_clause | split_no_values_clause }

      -  The syntax of **split_values_clause** that specifies a split point is as follows:

         ::

            VALUES ( { (partition_value) [, ...] } | DEFAULT } ) INTO ( PARTITION partition_name  , PARTITION partition_name  )

         .. important::

            -  If the source partition is not a :ref:`DEFAULT partition <en-us_topic_0000001764675414__li105701736194813>`, the boundary specified by the split point is a non-void proper subset of the source partition boundary. If the source partition is a DEFAULT partition, the boundary specified by the split point cannot overlap with the boundaries of other non-DEFAULT partitions.
            -  The boundary specified by the split point is the boundary of the first partition after the keyword **INTO**. The difference between the boundary of the source partition and the specified boundary of the split point is the boundary of the second partition.
            -  If the source partition is the DEFAULT partition, the boundary of the second partition is still DEFAULT.

      -  The syntax of **split_no_values_clause** that specifies no split points is as follows:

         ::

            INTO ( list_partition_item [, ....], PARTITION partition_name )

         .. important::

            -  The syntax of :ref:`list_partition_item <en-us_topic_0000001764675414__li135021622911>` is the same as the syntax specifying a partition in creating a list partitioned table, except that the boundary value here cannot be DEFAULT.
            -  Except for the last partition, the boundaries of other partitions must be explicitly defined. The defined boundary cannot be DEFAULT and must be a non-empty proper subset of the source partition boundary. The boundary of the last partition is the difference set between the source partition boundary and other partition boundaries, and the boundary of the last partition is not empty (that is, the difference set cannot be empty).
            -  If the source partition is a DEFAULT partition, the boundary of the last partition is DEFAULT.

   -  The syntax of **add_clause** is used to add a partition to one or more specified partitioned tables.

      **The add_clause syntax in range partitioning is as follows:**

      ::

         ADD { partition_less_than_item... | partition_start_end_item }

      .. important::

         -  The :ref:`partition_less_than_item <en-us_topic_0000001764675414__li1147714355320>` syntax can only be used for range partitioned tables. Otherwise, an error will be reported.
         -  The syntax of :ref:`partition_less_than_item <en-us_topic_0000001764675414__li1147714355320>` is the same as the syntax specifying partitions in creating a range partitioned table.
         -  If the boundary value of the last partition is a MAXVALUE, new partitions cannot be added. Otherwise, an error will be reported.

      **The add_clause syntax for list partitioning is as follows:**

      ::

         ADD list_partition_item

      .. important::

         -  The :ref:`list_partition_item <en-us_topic_0000001764675414__li135021622911>` syntax can only be used for a list partitioned table. Otherwise, an error will be reported.
         -  The :ref:`list_partition_item <en-us_topic_0000001764675414__li135021622911>` syntax is the same as the syntax specifying a partition in creating a list partitioned table.
         -  If the current partition table contains DEFAULT partitions, no new partitions can be added. Otherwise, an error will be reported.

   -  The syntax of **drop_clause** is used to remove a specified partition from a partitioned table.

      ::

         DROP PARTITION  { partition_name | FOR (  partition_value [, ...] )  }

   -  The **drop_clause** syntax supports deleting multiple partitions. (This feature is supported by clusters of 8.1.3.100 and later versions.)

      ::

         DROP PARTITION  { partition_name [, ... ] }

   -  The **truncate_partitioned_clause** clause is used to clear data in a table partition.

      ::

         TRUNCATE PARTITION { partition_name | FOR (  partition_value  [, ...] )  };

      .. important::

         **partition_value** indicates the partition key value. Multiple partition key values can be specified. Use commas (,) to separate multiple partition key values.

         When the **PARTITION FOR** clause is used, the entire partition where **partition_value** is located is cleared.

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

-  **WITHOUT UNUSABLE**

   Ignores indexes in the **UNUSABLE** state when rebuilding indexes on the partition.

-  **ENABLE/DISABLE ROW MOVEMENT**

   Specifies the row movement switch.

   Valid value:

   -  **ENABLE**: The row movement switch is enabled.
   -  **DISABLE**: The row movement switch is disabled.

   The switch is disabled by default.

   .. note::

      -  To enable cross-partition update, specify **ENABLE ROW MOVEMENT**. However, if **SELECT FOR UPDATE** is executed concurrently to query the partitioned table, the query results may be inconsistent. Therefore, exercise caution when performing this operation.
      -  If the tuple value is updated on the partition key during the **UPDATE** action, the partition where the tuple is located is altered. Setting of this parameter enables error messages to be reported or movement of the tuple between partitions.

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

Examples
--------

-  The syntax of **add_clause** is used to add a partition to one or more specified partitioned tables.

   Add the **ca_address_sk** partition to the range partitioned table **customer_address**. The value of **ca_address_sk** ranges from 700 to 900.

   ::

      ALTER TABLE customer_address ADD PARTITION P5 VALUES LESS THAN (900);

   Add partitions **[5000, 5300)**, **[5300, 5600)**, **[5600, 5900)**, and **[5900, 6000)** to the partitioned table **customer_address**:

   ::

      ALTER TABLE customer_address_SE ADD PARTITION p6 START(5000) END(6000) EVERY(300);

   Add the MAXVALUE partition **p6** to the range partitioned table **customer_address**:

   ::

      ALTER TABLE customer_address ADD PARTITION p6 END(MAXVALUE);

   Add partition **P6** to a list partitioned table:

   ::

      ALTER TABLE data_list ADD PARTITION P6 VALUES (202302,202303);

-  The **modify_clause** clause is used to set whether a partition index is usable.

   Create the local index **student_grade_index** for the partitioned table **customer_address** and set partition index names:

   ::

      CREATE INDEX customer_address_index ON customer_address(ca_address_id) LOCAL
      (
              PARTITION P1_index,
              PARTITION P2_index,
              PARTITION P3_index,
              PARTITION P4_index,
              PARTITION P5_index,
              PARTITION P6_index
      );

   Rebuild all indexes on partition **P1** in the partitioned table **customer_address**:

   ::

      ALTER TABLE customer_address MODIFY PARTITION P1 REBUILD UNUSABLE LOCAL INDEXES;

   Disable all indexes on partition **P3** of the partitioned table **customer_address**:

   ::

      ALTER TABLE customer_address MODIFY PARTITION P3 UNUSABLE LOCAL INDEXES;

-  The **split_clause** clause is used to split one partition into partitions.

   Split partition **P6** in the range partitioned table **customer_address** at **1200**:

   ::

      ALTER TABLE customer_address SPLIT PARTITION P6 AT(1200) INTO (PARTITION P6a,PARTITION P6b);

   Split the partition at 200 in the range partitioned table **customer_address** into multiple partitions:

   ::

      ALTER TABLE customer_address SPLIT PARTITION FOR(200) INTO(PARTITION p_part START(100) END(300) EVERY(50));

   Split partition **P2** in the list partitioned table **data_list** into two partitions: **p2a** and **p2b**.

   ::

      ALTER TABLE data_list SPLIT PARTITION P2 VALUES(202210) INTO (PARTITION p2a,PARTITION p2b);

-  **exchange_clause**: migrates data from an ordinary table to a specified partition.

   The following example demonstrates how to migrate data from table **math_grade** to partition **math** in partitioned table **student_grade**. Create a partitioned **table student_grade**.

   ::

      CREATE TABLE student_grade (
              stu_name     char(5),
              stu_no       integer,
              grade        integer,
              subject      varchar(30)
      )
      PARTITION BY LIST(subject)
      (
              PARTITION gym VALUES('gymnastics'),
              PARTITION phys VALUES('physics'),
              PARTITION history VALUES('history'),
              PARTITION math VALUES('math')
      );

   Add data to the partition table **student_grade**.

   ::

      INSERT INTO student_grade VALUES
              ('Ann', 20220101, 75, 'gymnastics'),
              ('Jeck', 20220103, 60, 'math'),
              ('Anna', 20220108, 56, 'history'),
              ('Jann', 20220107, 82, 'physics'),
              ('Molly', 20220104, 91, 'physics'),
              ('Sam', 20220105, 72, 'math');

   Query the records of partition **math** in **student_grade**.

   ::

      SELECT * FROM student_grade PARTITION (math);
       stu_name |  stu_no  | grade | subject
      ----------+----------+-------+---------
       Jeck     | 20220103 |    60 | math
       Sam      | 20220105 |    72 | math
      (2 rows)

   Create an ordinary table **math_grade** that matches the definition of the partitioned table **student_grade**.

   ::

      CREATE TABLE math_grade
      (
              stu_name     char(5),
              stu_no       integer,
              grade        integer,
              subject      varchar(30)
      );

   Add data to table **math_grade**. The data in the **student_grade** partition table conforms to the partition rule of partition **math**.

   ::

      INSERT INTO math_grade VALUES
              ('Ann', 20220101, 75, 'math'),
              ('Jeck', 20220103, 60, 'math'),
              ('Anna', 20220108, 56, 'math'),
              ('Jann', 20220107, 82, 'math');

   Migrate data from table **math_grade** to partition **math** in the partitioned table **student_grade**.

   ::

      ALTER TABLE student_grade EXCHANGE PARTITION (math) WITH TABLE math_grade;

   The query results of table **student_grade** shows that the data in table **math_grade** has been exchanged with the data in partition **math**.

   ::

      SELECT * FROM student_grade PARTITION (math);
       stu_name |  stu_no  | grade | subject
      ----------+----------+-------+---------
       Anna     | 20220108 |    56 | math
       Jeck     | 20220103 |    60 | math
       Ann      | 20220101 |    75 | math
       Jann     | 20220107 |    82 | math
      (4 rows)

   The query result of table **math_grade** shows that the records previously stored in partition **math** have been moved to table **student_grade**.

   ::

      SELECT * FROM math_grade;
       stu_name |  stu_no  | grade | subject
      ----------+----------+-------+---------
       Jeck     | 20220103 |    60 | math
       Sam      | 20220105 |    72 | math
      (2 rows)

-  The **truncate_partitioned_clause** clause is used to clear data in a table partition.

   Clear the **p1** partition of the **student_grade** table.

   ::

      ALTER TABLE student_grade TRUNCATE PARTITION p1;

-  The **row_clause** clause is used to set the row movement switch of a partitioned table.

   Enable migration for the partitioned table **customer_address**:

   ::

      ALTER TABLE customer_address ENABLE ROW MOVEMENT;

-  The **merge_clause** clause is used to merge partitions into one.

   Combine partitions **P2** and **P3** in the range partitioned table **customer_address** into one:

   ::

      ALTER TABLE customer_address MERGE PARTITIONS P2, P3 INTO PARTITION P_M;

-  The syntax of **drop_clause** is used to remove a specified partition from a partitioned table.

   Delete partition **P6** from the partitioned table **customer_address**:

   ::

      ALTER TABLE customer_address DROP PARTITION P6;

   Delete partitions **P3**, **P4**, and **P5** from the partitioned table **customer_address**.

   ::

      ALTER TABLE customer_address DROP PARTITION P3, P4, P5;

Helpful Links
-------------

:ref:`CREATE TABLE PARTITION <dws_06_0179>`, :ref:`DROP TABLE <dws_06_0208>`
