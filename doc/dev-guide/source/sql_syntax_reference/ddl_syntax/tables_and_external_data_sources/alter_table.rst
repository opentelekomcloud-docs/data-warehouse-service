:original_name: dws_06_0142.html

.. _dws_06_0142:

ALTER TABLE
===========

Function
--------

**ALTER TABLE** is used to modify tables, including modifying table definitions, renaming tables, renaming specified columns in tables, renaming table constraints, setting table schemas, enabling or disabling row-level access control, and adding or updating multiple columns.

Only the owner of a table, a user granted with the ALTER permission for the table, or a system administrator has the permission to run the **ALTER TABLE** statement. To change the owner or schema of a table, you must be the table owner or a system administrator and be a direct or indirect member of the new role.

Precautions
-----------

.. table:: **Table 1** Precautions for ALTER TABLE

   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Constraint                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
   +=============================================+================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | Storage method of tables                    | After a table is created, the parameter **ORIENTATION** (which specifies whether the table is a row-store or column-store table) cannot be modified.                                                                                                                                                                                                                                                                                                           |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Supported range of column-store tables      | Column-store tables support **PARTIAL CLUSTER KEY** but do not support table-level foreign key constraints. In 8.1.1 or later, you can use the **PRIMARY KEY** constraint and table-level **UNIQUE** constraint during the column-store table creation.                                                                                                                                                                                                        |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Modification of column-store tables         | -  The **NOT NULL** constraint and **PRIMARY KEY** constraint can be added to existing column-store tables using **ALTER**. This constraint is supported only by clusters of version 8.2.0 or later.                                                                                                                                                                                                                                                           |
   |                                             | -  If you delete the PRIMARY KEY constraint by specifying the constraint name, the NOT NULL constraint is not deleted. You can manually delete the NOT NULL constraint as needed.                                                                                                                                                                                                                                                                              |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Columns of column-store tables              | The column constraints supported by column-store tables include **NULL**, **NOT NULL**, and **DEFAULT** constant values. Only the **DEFAULT** value can be modified and deleted (**SET DEFAULT** and **DROP DEFAULT**), and only the **NOT NULL** constraint can be deleted.                                                                                                                                                                                   |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operations supported by column-store tables | -  **ADD COLUMN** is used to add a column. The types of new and modified columns should be the data types supported by column storage (for details, see :ref:`Data Types <dws_06_0008>`).                                                                                                                                                                                                                                                                      |
   |                                             | -  **ALTER TYPE** is used to change the data type of a column. The **USING** option of **ALTER TYPE** supports only constant expressions and expressions involving the column.                                                                                                                                                                                                                                                                                 |
   |                                             | -  **SET STATISTICS** is used to set the statistics collection target of a single column.                                                                                                                                                                                                                                                                                                                                                                      |
   |                                             | -  Table name can be modification.                                                                                                                                                                                                                                                                                                                                                                                                                             |
   |                                             | -  DROP COLUMN is used to delete a column.                                                                                                                                                                                                                                                                                                                                                                                                                     |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Modification of distribution columns        | The data type of the distribution column in an existing table cannot be modified.                                                                                                                                                                                                                                                                                                                                                                              |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Row-store REPLICATION tables                | A system column cannot be designated as a primary key in a row-store REPLICATION distributed table.                                                                                                                                                                                                                                                                                                                                                            |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Auto-increment columns                      | Auto-increment columns cannot be added, or a column in which the **DEFAULT** value contains the nextval() expression cannot be added either.                                                                                                                                                                                                                                                                                                                   |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Modification of OBS cold and hot tables     | -  The **cold_tablespace** and **storage_policy** parameters of **ALTER RESET** cannot be used in OBS hot or cold tables, and **COLVERSION** cannot be changed to **1.0** for such tables.                                                                                                                                                                                                                                                                     |
   |                                             | -  You can change a column-store table whose **COLVERSION** is **2.0** to an OBS hot or cold table. The **COLD_TABLESPACE** and **STORAGE_POLICY** parameters must be added.                                                                                                                                                                                                                                                                                   |
   |                                             | -  You can use **ALTER TABLE** to change the values of **STORAGE_POLICY** for **RELOPTIONS**. After the cold/hot switchover policy is changed, the cold/hot attribute of the existing cold data will not change. The new policy takes effect for the next cold/hot switchover.                                                                                                                                                                                 |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | V3 tables                                   | **COLVERSION** cannot be modified for V3 tables (**COLVERSION** is **3.0**), and non-V3 tables (**COLVERSION** is **2.0**) cannot be switched to V3 tables.                                                                                                                                                                                                                                                                                                    |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Operations that trigger table rebuilding    | When an **ALTER TABLE** operation is performed on a table, it triggers table rebuilding. During this process, data is dumped into a new data file. Once the process is complete, the original file is deleted. If the table is large, the process can use a significant amount of disk space. When the disk space is insufficient, exercise caution when performing the **ALTER TABLE** operation on large tables to prevent the cluster from being read-only. |
   |                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
   |                                             | -  Change the data type of a column.                                                                                                                                                                                                                                                                                                                                                                                                                           |
   |                                             | -  Add columns (including the oid column) to a row-store table.                                                                                                                                                                                                                                                                                                                                                                                                |
   |                                             | -  Modify **COLVERSION** for a column-store table.                                                                                                                                                                                                                                                                                                                                                                                                             |
   |                                             | -  Specify the **DEFAULT** constant values for a column added to a column-store table. If the **DEFAULT** values contain volatile functions or are not **NULL** and do not belong to a specific data type, ensure they are correctly defined.                                                                                                                                                                                                                  |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Other                                       | -  When you modify the **COLVERSION** or **enable_delta** parameter of a column-store table, other **ALTER** operations cannot be performed.                                                                                                                                                                                                                                                                                                                   |
   |                                             | -  You cannot specify a tablespace when running **ALTER TABLE** for an unlogged table. When you run **ALTER TABLE** for a non-unlogged table, do not specify **pg_unlogged** as the tablespace.                                                                                                                                                                                                                                                                |
   |                                             | -  Distribution columns cannot be modified for global temporary tables.                                                                                                                                                                                                                                                                                                                                                                                        |
   +---------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. warning::

   -  Avoid performing **ALTER TABLE**, **ALTER TABLE PARTITION**, **DROP PARTITION**, and **TRUNCATE** operations during peak hours to prevent long SQL statements from blocking these operations or SQL services.
   -  For more information about development and design specifications, see "DWS Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

ALTER TABLE Syntax Format
-------------------------

::

   ALTER TABLE [ IF EXISTS ] { table_name [*] | ONLY table_name | ONLY ( table_name ) }
       action [, ... ];

**action** can be the following modification operations:

-  Change the data type of an existing column in the table. You can modify the null constraint, default value, and comment information at the same time. Only the type conversion of the same category (between values, strings, and time) is allowed.

   .. code-block::

      ALTER TABLE [ IF EXISTS ] table_name
          MODIFY ( { column_name data_type | [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ] |
              [ CONSTRAINT constraint_name ] NULL | DEFAULT default_expr | COMMENT 'text' } [, ...] );

-  Rename the table. Changing the name does not affect the stored data. Tables cannot be renamed across schemas. The new table name can contain the schema name of the original table. If **new_table_name** does not specify a schema, the new table and the original table are in the same schema.

   ::

      ALTER TABLE [ IF EXISTS ] table_name
          RENAME TO new_table_name;
      ALTER TABLE [ IF EXISTS ] table_name
          RENAME TO schema.new_table_name;

-  Rename the specified column in the table.

   ::

      ALTER TABLE [ IF EXISTS ] { table_name [*] | ONLY table_name | ONLY ( table_name )}
          RENAME [ COLUMN ] column_name TO new_column_name;

-  Rename the constraint of the table.

   ::

      ALTER TABLE [ IF EXISTS ] { table_name  [*] | ONLY table_name | ONLY ( table_name  )}
          RENAME CONSTRAINT constraint_name TO new_constraint_name;

-  Set the schema of the table.

   ::

      ALTER TABLE [ IF EXISTS ] table_name
          SET SCHEMA new_schema;

   .. note::

      -  Currently, **SET SCHEMA** can only set schemas to user schemas. It cannot set a schema to a system internal schema.
      -  The schema setting moves the table into another schema. Associated indexes and constraints owned by table columns are migrated as well. Currently, the schema for sequences cannot be changed. If the table has sequences, delete the sequences, and create them again or delete the ownership between the table and sequences. In this way, the table schema can be changed.
      -  To change the schema of a table, you must also have CREATE privilege on the new schema. To add the table as a new child of a parent table, you must own the parent table as well. To alter the owner, you must also be a direct or indirect member of the new owning role, and that role must have CREATE permission on the table's schema. These restrictions mean that you can only rebuild or delete tables. The system administrator can modify all permissions of any table in any way.

-  Add columns.

   ::

      ALTER TABLE [ IF EXISTS ] table_name
          ADD ( { column_name data_type [ compress_mode ] [ COLLATE collation ] [ column_constraint [ ... ] ]} [, ...] );

-  Update columns.

   ::

      ALTER TABLE [ IF EXISTS ] table_name
          MODIFY ( { column_name data_type | column_name [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ] | column_name [ CONSTRAINT constraint_name ] NULL } [, ...] );

-  Delete an index from a specified table.

   ::

      ALTER TABLE table_name DROP INDEX index_name;

.. note::

   -  All the actions except for **RENAME** and **SET SCHEMA** can be combined into a list of multiple alterations to apply in parallel. For example, it is possible to add several columns or alter the type of several columns in a single command. This is useful with large tables, since only one pass over the table need be made.
   -  Adding a **CHECK** or **NOT NULL** constraint requires scanning the table to verify that existing rows meet the constraint.
   -  Adding a column with a non-null default or changing the type of an existing column will require the entire table to be rewritten. Table rebuilding may take a significant amount of time for a large table; and will temporarily require as much as double the disk space.

Clauses of action Related to Table Operations
---------------------------------------------

There are several clauses of **action**:

::

   column_clause
       | ADD table_constraint [ NOT VALID ]
       | ADD table_constraint_using_index
       | VALIDATE CONSTRAINT constraint_name
       | DROP CONSTRAINT [ IF EXISTS ]  constraint_name [ RESTRICT | CASCADE ]
       | CLUSTER ON index_name
       | SET WITHOUT CLUSTER
       | SET ( {storage_parameter = value} [, ... ] )
       | RESET ( storage_parameter [, ... ] )
       | OWNER TO new_owner
       | SET TABLESPACE new_tablespace
       | SET {COMPRESS|NOCOMPRESS}
       | DISTRIBUTE BY { REPLICATION | ROUNDROBIN | { HASH ( column_name [,...] ) } }
       | TO { GROUP groupname | NODE ( nodename [, ... ] ) }
       | ADD NODE ( nodename [, ... ] )
       | DELETE NODE ( nodename [, ... ] )
       | DISABLE TRIGGER [ trigger_name | ALL | USER ]
       | ENABLE TRIGGER [ trigger_name | ALL | USER ]
       | ENABLE REPLICA TRIGGER trigger_name
       | ENABLE ALWAYS TRIGGER trigger_name
       | DISABLE ROW LEVEL SECURITY
       | ENABLE ROW LEVEL SECURITY
       | FORCE ROW LEVEL SECURITY
       | NO FORCE ROW LEVEL SECURITY
       | REFRESH STORAGE

-  **ADD table_constraint [ NOT VALID ]**

   Adds a new table constraint. When used with the **NOT VALID** option, this constraint is valid only for foreign keys and **CHECK** constraints. If the **NOT VALID** option is added to the constraint, the check on whether the existing records in the table meet the initial constraint is skipped.

-  **ADD table_constraint_using_index**

   Adds primary key constraint or unique constraint based on the unique index.

-  **VALIDATE CONSTRAINT constraint_name**

   Validates a foreign key or check constraint that was previously created as **NOT VALID**, by scanning the table to ensure there are no rows for which the constraint is not satisfied. Nothing happens if the constraint is already marked valid.

-  **DROP CONSTRAINT [ IF EXISTS ] constraint_name [ RESTRICT \| CASCADE ]**

   Drops a table constraint.

-  **CLUSTER ON index_name**

   Selects the default index for future **CLUSTER** operations. It does not actually re-cluster the table.

-  **SET WITHOUT CLUSTER**

   Removes the most recently used **CLUSTER** index specification from the table. This operation affects future cluster operations that do not specify an index.

-  **SET ( {storage_parameter = value} [, ... ] )**

   Changes one or more storage parameters for the table.

-  **RESET ( storage_parameter [, ... ] )**

   Resets one or more storage parameters to their defaults. As with **SET**, a table rewrite might be needed to update the table entirely.

-  **OWNER TO new_owner**

   Changes the owner of the table, sequence, or view to the specified user.

-  **SET {COMPRESS|NOCOMPRESS}**

   Sets the compression feature of a table. The table compression feature affects only the storage mode of data inserted in a batch subsequently and does not affect storage of existing data. Setting the table compression feature will result in the fact that there are both compressed and uncompressed data in the table.

-  **DISTRIBUTE BY { REPLICATION \| ROUNDROBIN \| { HASH ( column_name [,...] ) } }**

   Changing a table's distribution mode will physically redistribute the table data based on the new distribution mode. After the distribution mode is changed, you are advised to manually run the **ANALYZE** statement to collect new statistics about the table.

   .. note::

      -  This operation is a major change operation, involving table distribution information modification and physical data redistribution. During the modification, services are blocked. After the modification, the original execution plan of services will change. Perform this operation according to the standard change process.
      -  This operation is a resource-intensive operation. If you need to modify the distribution mode of large tables, perform the operation when the computing and storage resources are sufficient. Ensure that the remaining space of the entire cluster and the tablespace where the original table is located is sufficient to store a table that has the same size as the original table and is distributed in the new distribution mode.

-  **TO { GROUP groupname \| NODE ( nodename [, ... ] ) }**

   The syntax is only available in extended mode (when GUC parameter **enable_cluster_resize** is **on**). Exercise caution when enabling the mode. It is used for tools like internal dilatation tools. Common users should not use the mode.

-  **ADD NODE ( nodename [, ... ] )**

   It is only available for tools like internal dilatation. General users should not use the mode.

-  **DELETE NODE ( nodename [, ... ] )**

   It is only available for internal scale-in tools. Common users should not use the syntax.

-  **DISABLE TRIGGER [ trigger_name \| ALL \| USER ]**

   Disables a single trigger specified by **trigger_name**, disables all triggers, or disables only user triggers (excluding internally generated constraint triggers, for example, deferrable unique constraint triggers and exclusion constraints triggers).

   .. note::

      Exercise caution when using this function because data integrity cannot be ensured as expected if the triggers are not executed.

-  **ENABLE TRIGGER [ trigger_name \| ALL \| USER ]**

   Enables a single trigger specified by **trigger_name**, enables all triggers, or enables only user triggers.

-  **ENABLE REPLICA TRIGGER trigger_name**

   Determines that the trigger firing mechanism is affected by the configuration variable **session_replication_role**. When the replication role is **origin** (default value) or **local**, a simple trigger is fired.

   When **ENABLE REPLICA** is configured for a trigger, it is fired only when the session is in **replica** mode.

-  **ENABLE ALWAYS TRIGGER trigger_name**

   Determines that all triggers are fired regardless of the current replication mode.

-  **DISABLE/ENABLE ROW LEVEL SECURITY**

   Enables or disables row-level access control for a table.

   If row-level access control is enabled for a data table but no row-level access control policy is defined, the row-level access to the data table is not affected. If row-level access control for a table is disabled, the row-level access to the table is not affected even if a row-level access control policy has been defined. For details, see :ref:`CREATE ROW LEVEL SECURITY POLICY <dws_06_0169>`.

   .. note::

      Row-level access control cannot be enabled for HDFS tables, foreign tables, and temporary tables.

-  **NO FORCE/FORCE ROW LEVEL SECURITY**

   Forcibly enables or disables row-level access control for a table.

   By default, the table owner is not affected by the row-level access control feature. However, if row-level access control is forcibly enabled, the table owner (excluding system administrators) will be affected. System administrators are not affected by any row-level access control policies.

-  **REFRESH STORAGE**

   Changes the local hot partitions that meet the criteria specified in the **storage_policy** parameter of an OBS multi-temperature table to the cold partitions stored in the OBS.

   For example, if **storage_policy** is set to **'LMT:10'** for an OBS hot or cold table when it is created, the partitions that are not updated within the last 10 days are switched to cold partitions in the OBS.

Clauses of column_clause Related to Column Operations
-----------------------------------------------------

There are several clauses of **column_clause**:

::

   ADD [ COLUMN ] column_name data_type [ compress_mode ] [ COLLATE collation ] [ column_constraint [ ... ] ]
       | MODIFY [ COLUMN ] column_name data_type
       | MODIFY [ COLUMN ] column_name [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ]
       | MODIFY [ COLUMN ] column_name [ CONSTRAINT constraint_name ] NULL
       | MODIFY [ COLUMN ] column_name DEFAULT default_expr
       | MODIFY [ COLUMN ] column_name ON UPDATE on_update_expr
       | MODIFY [ COLUMN ] column_name COMMENT comment_text
       | DROP [ COLUMN ] [ IF EXISTS ] column_name [ RESTRICT | CASCADE ]
       | ALTER [ COLUMN ] column_name [ SET DATA ] TYPE data_type [ COLLATE collation ] [ USING expression ]
       | ALTER [ COLUMN ] column_name { SET DEFAULT expression | DROP DEFAULT }
       | ALTER [ COLUMN ] column_name { SET | DROP } NOT NULL
       | ALTER [ COLUMN ] column_name SET STATISTICS [PERCENT] integer
       | ADD STATISTICS (( column_1_name, column_2_name [, ...] ))
       | ADD { INDEX | UNIQUE [ INDEX ] } [ index_name ] ( { { column_name | ( expression ) } [ COLLATE collation ] [ opclass ] [ ASC | DESC ] [ NULLS LAST ] } [, ...] ) [ USING method ] [ NULLS [ NOT ] DISTINCT | NULLS IGNORE ] [ COMMENT 'text' ] LOCAL [ ( { PARTITION index_partition_name } [, ...] ) ] [ WITH ( { storage_parameter = value } [, ...] ) ]
       | ADD { INDEX | UNIQUE [ INDEX ] } [ index_name ] ({ { column_name | ( expression ) } [ COLLATE collation ] [ opclass ] [ ASC | DESC ] [ NULLS { FIRST | LAST } ] }[, ...] ) [ USING method ] [ NULLS [ NOT ] DISTINCT | NULLS IGNORE ] [ COMMENT 'text' ] [ WITH ( {storage_parameter = value} [, ... ] ) ] [ WHERE predicate ]
       | DROP { INDEX | KEY } index_name
       | CHANGE [ COLUMN ] old_column_name new_column_name data_type [ [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ] |
           [ CONSTRAINT constraint_name ] NULL | DEFAULT default_expr | COMMENT 'text' ]
       | DELETE STATISTICS (( column_1_name, column_2_name [, ...] ))
       | ALTER [ COLUMN ] column_name SET ( {attribute_option = value} [, ... ] )
       | ALTER [ COLUMN ] column_name RESET ( attribute_option [, ... ] )
       | ALTER [ COLUMN ] column_name SET STORAGE { PLAIN | EXTERNAL | EXTENDED | MAIN }

-  **ADD [ COLUMN ] column_name data_type [ compress_mode ] [ COLLATE collation ] [ column_constraint [ ... ] ]**

   Adds a column to a table. If a column is added with **ADD COLUMN**, all existing rows in the table are initialized with the column's default value (**NULL** if no **DEFAULT** clause is specified).

-  **ADD ( { column_name data_type [ compress_mode ] } [, ...] )**

   Adds columns in the table.

-  **MODIFY [ COLUMN ] column_name data_type**

   Modifies the data type of an existing field in a table. Note that the data type of the distribution column cannot be modified.

-  **MODIFY [ COLUMN ] column_name [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ]**

   Adds a NOT NULL constraint to a column of a table. Currently, this clause is unavailable to column-store tables.

-  **MODIFY [ COLUMN ] column_name [ CONSTRAINT constraint_name ] NULL**

   Deletes the NOT NULL constraint to a certain column in the table.

-  **MODIFY [ COLUMN ] column_name DEFAULT default_expr**

   Changes the default value of the table.

-  **MODIFY [ COLUMN ] column_name ON UPDATE on_update_expr**

   Modifies the ON UPDATE expression of a specified column in a table. The column must be of the timestamp or timestamptz type. If **on_update_expr** is NULL, the **ON UPDATE** clause is deleted.

-  **MODIFY [ COLUMN ] column_name COMMENT comment_text**

   Modifies the comment of the table.

-  **DROP [ COLUMN ] [ IF EXISTS ] column_name [ RESTRICT \| CASCADE ]**

   Drops a column from a table. Index and constraint related to the column are automatically dropped. If an object not belonging to the table depends on the column, **CASCADE** must be specified, such as foreign key reference and view.

   The **DROP COLUMN** form does not physically remove the column, but simply makes it invisible to SQL operations. Subsequent insert and update operations in the table will store a **NULL** value for the column. Therefore, column deletion takes a short period of time but does not immediately release the table space on the disks, because the space occupied by the deleted column is not reclaimed. The space will be reclaimed when **VACUUM** is executed.

-  **ALTER [ COLUMN ] column_name [ SET DATA ] TYPE data_type [ COLLATE collation ] [ USING expression ]**

   Change the data type of a field in the table. Only the type conversion of the same category (between values, character strings, and time) is allowed. Indexes and simple table constraints on the column will automatically use the new data type by reparsing the originally supplied expression.

   **ALTER TYPE** requires an entire table be rewritten. This is an advantage sometimes, because it frees up unnecessary space from a table. For example, to reclaim the space occupied by a deleted column, the fastest method is to use the command.

   ::

      ALTER TABLE table ALTER COLUMN anycol TYPE anytype;

   In this command, **anycol** indicates any column existing in the table and **anytype** indicates the type of the prototype of the column. The result is no visible semantic change to the table, but this command forces a rewrite, thereby removing data that is no longer in use.

   **USING** in **ALTER TYPE** can specify any expression involving the old values of the row; that is, it can refer to any columns other than the one being converted. **ALTER TYPE** can be used to implement common type conversion. Because of this flexibility, the **USING** expression is not applied to the column's default value (if any). The result might not be a constant expression as required for a default. This means that when there is no implicit or assignment cast from old to new type, **ALTER TYPE** might fail to convert the default even though a **USING** clause is supplied. In this case, run **DROP DEFAULT** to delete the default value, run **ALTER TYPE**, and then run **SET DEFAULT** to set a new default value. Similar issues apply to indexes and constraints involving the column.

-  **ALTER [ COLUMN ] column_name { SET DEFAULT expression \| DROP DEFAULT }**

   Sets or removes the default value for a column. The default values only apply to subsequent **INSERT** commands; they do not cause rows already in the table to change. Defaults can also be created for views, in which case they are inserted into **INSERT** statements on the view before the view's **ON INSERT** rule is applied.

-  **ALTER [ COLUMN ] column_name { SET \| DROP } NOT NULL**

   Changes whether a column is marked to allow **NULL** values or to reject **NULL** values. You can only use **SET NOT NULL** when the column contains no **NULL** values.

-  **ALTER [ COLUMN ] column_name SET STATISTICS [PERCENT] integer**

   Specifies the per-column statistics-gathering target for subsequent **ANALYZE** operations. The value ranges from **0** to **10000**. Set it to **-1** to revert to using the default system statistics target.

-  **ADD { INDEX \| UNIQUE [ INDEX ] } [ index_name ] ( { { column_name \| ( expression ) } [ COLLATE collation ] [ opclass ] [ ASC \| DESC ] [ NULLS LAST ] } [, ...] ) [ USING method ] [ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ] [ COMMENT 'text' ] LOCAL [ ( { PARTITION index_partition_name } [, ...] ) ] [ WITH ( { storage_parameter = value } [, ...] ) ]**

   Create an index for the partitioned table. For details about the parameters, see :ref:`CREATE INDEX <dws_06_0165>`.

-  **ADD { INDEX \| UNIQUE [ INDEX ] } [ index_name ] ({ { column_name \| ( expression ) } [ COLLATE collation ] [ opclass ] [ ASC \| DESC ] [ NULLS { FIRST \| LAST } ] }[, ...] ) [ USING method ] [ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ] [ COMMENT 'text' ] [ WITH ( {storage_parameter = value} [, ... ] ) ] [ WHERE predicate ]**

   Create an index on the table. For details about the parameters, see :ref:`CREATE INDEX <dws_06_0165>`.

-  **DROP { INDEX \| KEY } index_name**

   Deletes an index from a table.

-  **CHANGE [ COLUMN ] old_column_name new_column_name data_type [ [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ] \|**

   **[ CONSTRAINT constraint_name ] NULL \| DEFAULT default_expr \| COMMENT 'text' ]**

   Modifies the column information in the table, such as column names and column field information.

-  **{ADD \| DELETE} STATISTICS ((column_1_name, column_2_name [, ...]))**

   Adds or deletes the declaration of collecting multi-column statistics to collect multi-column statistics as needed when **ANALYZE** is performed for a table or a database. The statistics about a maximum of 32 columns can be collected at a time. You are not allowed to add or delete the declaration for system tables or foreign tables

-  **ALTER [ COLUMN ] column_name SET ( {attribute_option = value} [, ... ] )**

   **ALTER [ COLUMN ] column_name RESET ( attribute_option [, ... ] )**

   Sets or resets per-attribute options.

   The attribute option parameters are **n_distinct**, **n_distinct_inherited**, and **cstore_cu_sample_ratio**. **n_distinct** specifies and fixes the statistics of a table's distinct values. **n_distinct_inherited** specifies and inherits the distinct value statistics. **cstore_cu_sample_ratio** specifies the CU ratio for **ANALYZE** on a column-store table. Currently, the **n_distinct_inherited** parameter cannot be set or reset.

   -  n_distinct

      Sets the distinct value statistics of the column.

      Value range: -1.0 to INT_MAX

      Default value: **0**, indicating that this parameter is not set.

   -  n_distinct_inherited

      Sets the distinct value statistics of the column in an inherited table.

      Value range: -1.0 to INT_MAX

      Default value: **0**, indicating that this parameter is not set.

   -  cstore_cu_sample_ratio

      Specifies the expansion multiple in the calculation of CUs to be sampled during ANALYZE on a column-store table.

      Value range: 1.0-10000.0

      Default value: **1.0**

-  **ALTER [ COLUMN ] column_name SET STORAGE { PLAIN \| EXTERNAL \| EXTENDED \| MAIN }**

   Sets the storage mode for a column. This clause specifies whether this column is held inline or in a secondary TOAST table, and whether the data should be compressed. This statement can only be used for row-based tables. **SET STORAGE** only sets the strategy to be used for future table operations.

Options of Other Parameters
---------------------------

-  **column_constraint** is as follows:

   ::

      [ CONSTRAINT constraint_name ]
          { NOT NULL |
            NULL |
            CHECK ( expression ) |
            DEFAULT default_expr  |
            UNIQUE [ NULLS [ NOT ] DISTINCT | NULLS IGNORE ] index_parameters |
            PRIMARY KEY index_parameters }
          [ DEFERRABLE | NOT DEFERRABLE | INITIALLY DEFERRED | INITIALLY IMMEDIATE ]

-  **compress_mode** of a column is as follows:

   ::

      [ DELTA | PREFIX | DICTIONARY | NUMSTR | NOCOMPRESS ]

-  **table_constraint_using_index** used to add the primary key constraint or unique constraint based on the unique index is as follows:

   ::

      [ CONSTRAINT constraint_name ]
          { UNIQUE | PRIMARY KEY } USING INDEX index_name
          [ DEFERRABLE | NOT DEFERRABLE | INITIALLY DEFERRED | INITIALLY IMMEDIATE ]

-  Add foreign key constraint **REFERENCES**.

   ::

      [ CONSTRAINT constraint_name ]
       FOREIGN KEY ( column_name [, ... ] ) REFERENCES reftable [ ( refcolumn [, ... ] ) ] }

-  **table_constraint** is as follows:

   ::

      [ CONSTRAINT constraint_name ]
          { CHECK ( expression ) |
            UNIQUE [ NULLS [ NOT ] DISTINCT | NULLS IGNORE ] ( column_name [, ... ] ) index_parameters |
            PRIMARY KEY ( column_name [, ... ] ) index_parameters }

          [ DEFERRABLE | NOT DEFERRABLE | INITIALLY DEFERRED | INITIALLY IMMEDIATE ]

-  **index_parameters** is as follows:

   ::

      [ WITH ( {storage_parameter = value} [, ... ] ) ]
          [ USING INDEX TABLESPACE tablespace_name ]

.. _en-us_topic_0000001811634545__s3e87132692794964b56e3ba420e7b544:

Parameter Description
---------------------

.. table:: **Table 2** ALTER TABLE parameters

   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                                                                   | Description                                                                                                                                                                                                                                                                    | Value Range                                                                                                                           |
   +=============================================================================================+================================================================================================================================================================================================================================================================================+=======================================================================================================================================+
   | IF EXISTS                                                                                   | Sends a prompt instead of an error if the table name does not exist.                                                                                                                                                                                                           | ``-``                                                                                                                                 |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | ONLY                                                                                        | Performs operations only on the specified table name. If **ONLY** is not specified:                                                                                                                                                                                            | ``-``                                                                                                                                 |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             | The table and all subtables will be modified.                                                                                                                                                                                                                                  |                                                                                                                                       |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             | Note that you can only add or delete columns to or from a parent or child table, or change the type of a column. The parent table and its subsequent representatives must always have the same columns and types.                                                              |                                                                                                                                       |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | table_name                                                                                  | Specifies the name of the table to be modified.                                                                                                                                                                                                                                | Valid table name.                                                                                                                     |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             | You can add the asterisk (``*``) option following the table name to specify that all subtables are scanned, which is the default operation.                                                                                                                                    |                                                                                                                                       |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | new_table_name                                                                              | New name of a table.                                                                                                                                                                                                                                                           | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.                      |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | **column_name**, **column_1_name**, and **column_2_name**                                   | Specifies the name of an existing or new field.                                                                                                                                                                                                                                | Valid column name.                                                                                                                    |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | new_column_name                                                                             | Specifies the new name of a column.                                                                                                                                                                                                                                            | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.                      |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | constraint_name                                                                             | Specifies the name of a constraint.                                                                                                                                                                                                                                            | Valid constraint name.                                                                                                                |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | new_constraint_name                                                                         | Specifies the new name of a table constraint.                                                                                                                                                                                                                                  | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.                      |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | schema_name                                                                                 | Specifies the schema name of a table.                                                                                                                                                                                                                                          | Valid schema name.                                                                                                                    |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | new_schema                                                                                  | Specifies the new schema name.                                                                                                                                                                                                                                                 | Valid schema name.                                                                                                                    |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | index_name                                                                                  | Specifies the index name.                                                                                                                                                                                                                                                      | Valid index name.                                                                                                                     |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | new_owner                                                                                   | Specifies the name of the new owner of the table.                                                                                                                                                                                                                              | Valid username or role name.                                                                                                          |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | new_tablespace                                                                              | Specifies the name of the new tablespace to which the table belongs.                                                                                                                                                                                                           | Valid tablespace name.                                                                                                                |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | data_type                                                                                   | Specifies the type of a new column or a new type of an existing column.                                                                                                                                                                                                        | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.                      |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | storage_parameter                                                                           | Specifies the storage parameters of a table.                                                                                                                                                                                                                                   | For details, see :ref:`Table 3 <en-us_topic_0000001811634545__table1384463565217>`.                                                   |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | compress_mode                                                                               | Specifies the compression option of the table column. Currently, this parameter is valid only for row-store tables. The clause specifies the algorithm preferentially used by the column.                                                                                      | ``-``                                                                                                                                 |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | collation                                                                                   | Specifies the collation rule name of a column. The optional **COLLATE** clause specifies a collation for the new column; if omitted, the collation is the default for the new column.                                                                                          | ``-``                                                                                                                                 |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | USING expression                                                                            | A **USING** clause specifies how to compute the new column value from the old; if omitted, the default conversion is an assignment cast from old data type to new. A **USING** clause must be provided if there is no implicit or assignment cast from the old to new type.    | ``-``                                                                                                                                 |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | NOT NULL \| NULL                                                                            | Sets whether the column allows null values.                                                                                                                                                                                                                                    | ``-``                                                                                                                                 |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | integer                                                                                     | Specifies a signed integer constant. When **PERCENT** is used, statistics are collected based on the percentage of table data.                                                                                                                                                 | 0~100                                                                                                                                 |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | attribute_option                                                                            | Specifies an attribute option.                                                                                                                                                                                                                                                 | ``-``                                                                                                                                 |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | PLAIN \| EXTERNAL \| EXTENDED \| MAIN                                                       | Specifies a column storage mode.                                                                                                                                                                                                                                               | ``-``                                                                                                                                 |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             | -  **PLAIN** must be used for fixed-length values (such as integers). It must be inline and uncompressed.                                                                                                                                                                      |                                                                                                                                       |
   |                                                                                             | -  **MAIN** is for inline, compressible data.                                                                                                                                                                                                                                  |                                                                                                                                       |
   |                                                                                             | -  **EXTERNAL** is for external, uncompressed data. Use of **EXTERNAL** will make substring operations on **text** and **bytea** values run faster, at the penalty of increased storage space.                                                                                 |                                                                                                                                       |
   |                                                                                             | -  **EXTENDED** is for external, compressed data. **EXTENDED** is the default for most data types that support non-**PLAIN** storage.                                                                                                                                          |                                                                                                                                       |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | CHECK ( expression )                                                                        | New or updated rows must satisfy for an insert or update operation to succeed. Expressions evaluating to TRUE succeed. If any row of an insert or update operation produces a FALSE result, an error exception is raised and the insert or update does not alter the database. | ``-``                                                                                                                                 |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             | A check constraint specified as a column constraint should reference only the column's values, while an expression appearing in a table constraint can reference multiple columns.                                                                                             |                                                                                                                                       |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             | Currently, **CHECK** expression does not include subqueries and cannot use variables apart from the current column.                                                                                                                                                            |                                                                                                                                       |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | DEFAULT default_expr                                                                        | Assigns a default data value for a column.                                                                                                                                                                                                                                     | ``-``                                                                                                                                 |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             | The data type of the default expression must match the data type of the column.                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             | The default expression will be used in any insert operation that does not specify a value for the column. If **DEFAULT** is not explicitly specified, the default value is **NULL**.                                                                                           |                                                                                                                                       |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             | If a suffix operator, such as (!). is used in **default_expr**, enclose the operator in parentheses.                                                                                                                                                                           |                                                                                                                                       |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | UNIQUE [ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ] index_parameters                          | The **UNIQUE** constraint specifies that a group of one or more columns of a table can contain only unique values.                                                                                                                                                             | ``-``                                                                                                                                 |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   | UNIQUE ( column_name [, ... ] ) [ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ] index_parameters | The **[ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ]** field is used to specify how to process null values in the index column of the Unique index. For details, see :ref:`Table 4 <en-us_topic_0000001811634545__table14283475417>`.                                              |                                                                                                                                       |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             | Default value: This parameter is left empty by default. NULL values can be inserted repeatedly.                                                                                                                                                                                |                                                                                                                                       |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | PRIMARY KEY index_parameters                                                                | The primary key constraint specifies that a column or columns of a table can contain only unique (non-duplicate) and non-null values.                                                                                                                                          | ``-``                                                                                                                                 |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   | PRIMARY KEY ( column_name [, ... ] ) index_parameters                                       |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | DEFERRABLE \| NOT DEFERRABLE \| INITIALLY DEFERRED \| INITIALLY IMMEDIATE                   | Sets whether the constraint is deferrable. This option is unavailable to column-store tables.                                                                                                                                                                                  | ``-``                                                                                                                                 |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             | -  **DEFERRABLE**: deferrable can be postponed until the end of the transaction using the **SET CONSTRAINTS** command.                                                                                                                                                         |                                                                                                                                       |
   |                                                                                             | -  **NOT DEFERRABLE**: checks immediately after the execution of each command.                                                                                                                                                                                                 |                                                                                                                                       |
   |                                                                                             | -  **INITIALLY IMMEDIATE**: checks immediately after the execution of each statement.                                                                                                                                                                                          |                                                                                                                                       |
   |                                                                                             | -  **INITIALLY DEFERRED**: checks when the transaction ends.                                                                                                                                                                                                                   |                                                                                                                                       |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | WITH ( {storage_parameter = value} [, ... ] )                                               | Specifies an optional storage parameter for a table or an index.                                                                                                                                                                                                               | ``-``                                                                                                                                 |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | COMPRESS|NOCOMPRESS                                                                         | -  **NOCOMPRESS**: If the **NOCOMPRESS** keyword is specified, the existing compression feature of the table is not changed.                                                                                                                                                   | ``-``                                                                                                                                 |
   |                                                                                             | -  **COMPRESS**: If the **COMPRESS** keyword is specified, the table compression feature is triggered if tuples are inserted in a batch.                                                                                                                                       |                                                                                                                                       |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | CASCADE                                                                                     | Automatically drops objects that depend on the dropped column or constraint (for example, views referencing the column).                                                                                                                                                       | ``-``                                                                                                                                 |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | RESTRICT                                                                                    | Refuses to drop the column or constraint if there are any dependent objects. This is the default behavior.                                                                                                                                                                     | ``-``                                                                                                                                 |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+
   | cache_policy                                                                                | Specifies the table cache policy. This parameter is supported only in storage-compute decoupled 3.0 clusters.                                                                                                                                                                  | -  **ALL**: Hot cache is used for the entire table.                                                                                   |
   |                                                                                             |                                                                                                                                                                                                                                                                                | -  **NONE**: Cold cache is used for the entire table.                                                                                 |
   |                                                                                             | Default value: **ALL**                                                                                                                                                                                                                                                         | -  **HPN**: The first *N* partitions in a partitioned table use hot cache. The rest of the partitions use cold cache.                 |
   |                                                                                             |                                                                                                                                                                                                                                                                                | -  **HPL:** *P1, P2, ...*. In a partitioned table, the specified partitions use hot cache. The rest of the partitions use cold cache. |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             |                                                                                                                                                                                                                                                                                | .. note::                                                                                                                             |
   |                                                                                             |                                                                                                                                                                                                                                                                                |                                                                                                                                       |
   |                                                                                             |                                                                                                                                                                                                                                                                                |    -  For foreign tables and non-partitioned tables, only the **ALL** and **NONE** cache policies are supported.                      |
   |                                                                                             |                                                                                                                                                                                                                                                                                |    -  Only range-partitioned and list-partitioned internal tables support HPN and HPL cache policies.                                 |
   +---------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001811634545__table1384463565217:

.. table:: **Table 3** storage_parameter parameters

   +---------------------------------------------------------------------------------+--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter Type                                                                  | Parameter          | Description                                                                                                                                                                                                                                                                           | Value Range                                                                                                                                                                                                          |
   +=================================================================================+====================+=======================================================================================================================================================================================================================================================================================+======================================================================================================================================================================================================================+
   | Partition management parameters (newly added)                                   | PERIOD             | Sets the period for automatically creating partitions in partition management.                                                                                                                                                                                                        | Interval type.                                                                                                                                                                                                       |
   |                                                                                 |                    |                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                      |
   |                                                                                 |                    | .. note::                                                                                                                                                                                                                                                                             | For details about the value range of **PERIOD** and the constraints on enabling this function, see the **PERIOD** parameter in :ref:`WITH Clause Parameters <en-us_topic_0000001764675414__section183201214104111>`. |
   |                                                                                 |                    |                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                      |
   |                                                                                 |                    |    -  If this parameter is not configured when you create a table, you can run the **set** statements to configure this parameter and enable automatic partition creation. If this parameter has been configured before, you can run the **set** statements to modify this parameter. |                                                                                                                                                                                                                      |
   |                                                                                 |                    |    -  You can run the **reset** command to disable the automatic partition creation function. However, if the automatic partition deletion function is enabled, the automatic partition creation function cannot be disabled.                                                         |                                                                                                                                                                                                                      |
   +---------------------------------------------------------------------------------+--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                                                                                 | TTL                | Sets the partition expiration time for automatically deleting partitions in partition management.                                                                                                                                                                                     | Interval type.                                                                                                                                                                                                       |
   |                                                                                 |                    |                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                      |
   |                                                                                 |                    | .. note::                                                                                                                                                                                                                                                                             | For details about the value range of **TTL** and restrictions on enabling this function, see the **TTL** parameters in :ref:`WITH Clause Parameters <en-us_topic_0000001764675414__section183201214104111>`.         |
   |                                                                                 |                    |                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                      |
   |                                                                                 |                    |    -  If this parameter is not configured when you create a table, you can run the **set** statements to configure this parameter and enable automatic partition deletion. If this parameter has been configured before, you can run the **set** statements to modify this parameter. |                                                                                                                                                                                                                      |
   |                                                                                 |                    |    -  You can run the **reset** command to disable the automatic partition deletion.                                                                                                                                                                                                  |                                                                                                                                                                                                                      |
   +---------------------------------------------------------------------------------+--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameters related to column-store tables in Turbo storage format (newly added) | enable_turbo_store | Specifies whether the column-store table is in the turbo storage format. This is supported only by 9.1.0.100 and later cluster versions.                                                                                                                                              | ``-``                                                                                                                                                                                                                |
   |                                                                                 |                    |                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                      |
   |                                                                                 |                    | .. note::                                                                                                                                                                                                                                                                             |                                                                                                                                                                                                                      |
   |                                                                                 |                    |                                                                                                                                                                                                                                                                                       |                                                                                                                                                                                                                      |
   |                                                                                 |                    |    -  Common column-store tables in version 3.0 do not support the turbo storage format, while HStore Opt tables in version 3.0 only support the turbo storage format.                                                                                                                |                                                                                                                                                                                                                      |
   |                                                                                 |                    |    -  In version 2.0, there is no restriction on column-store tables.                                                                                                                                                                                                                 |                                                                                                                                                                                                                      |
   +---------------------------------------------------------------------------------+--------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

The NULL value of the index column in the unique index is processed as follows:

Use the **[NULLS [NOT] DISTINCT \| NULLS IGNORE]** field to set how null values are handled in the unique index column.

When the inserted data is compared with the original data in the table, the NULL value can be processed in any of the following ways:

-  NULLS DISTINCT: NULL values are unequal and can be inserted repeatedly.
-  NULLS NOT DISTINCT: NULL values are equal. If all index columns are NULL, NULL values cannot be inserted repeatedly. If some index columns are NULL, data can be inserted only when non-null values are different.
-  NULLS IGNORE: NULL values are skipped during the equivalent comparison. If all index columns are NULL, NULL values can be inserted repeatedly. If some index columns are NULL, data can be inserted only when non-null values are different.

.. _en-us_topic_0000001811634545__table14283475417:

.. table:: **Table 4** Processing of NULL values in index columns in unique indexes

   +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
   | Constraint         | All Index Columns Are NULL     | Some Index Columns Are NULL.                                                                               |
   +====================+================================+============================================================================================================+
   | NULLS DISTINCT     | Can be inserted repeatedly.    | Can be inserted repeatedly.                                                                                |
   +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
   | NULLS NOT DISTINCT | Cannot be inserted repeatedly. | Cannot be inserted if the non-null values are equal. Can be inserted if the non-null values are not equal. |
   +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
   | NULLS IGNORE       | Can be inserted repeatedly.    | Cannot be inserted if the non-null values are equal. Can be inserted if the non-null values are not equal. |
   +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+

Table Operation Examples
------------------------

Create the sample table **customer**.

::

   DROP TABLE IF EXISTS customer;
   CREATE TABLE customer
   (
       C_CUSTKEY     BIGINT       ,
       C_NAME        VARCHAR(25)  ,
       C_ADDRESS     VARCHAR(40)  ,
       C_NATIONKEY   INT          ,
       C_PHONE       CHAR(15)     ,
       C_ACCTBAL     DECIMAL(15,2)
   )
   DISTRIBUTE BY HASH(C_CUSTKEY);

Create the column-store table **customer_address**.

::

   DROP TABLE IF EXISTS customer_address;
   CREATE TABLE customer_address
   (
       ca_address_sk       INTEGER                  NOT NULL   ,
       ca_address_id       CHARACTER(16)            NOT NULL   ,
       ca_street_number    CHARACTER(10)                       ,
       ca_street_name      CHARACTER varying(60)               ,
       ca_street_type      CHARACTER(15)                       ,
       ca_suite_number     CHARACTER(10)
   )
   WITH (ORIENTATION = COLUMN, COMPRESSION=HIGH,COLVERSION=2.0)
   DISTRIBUTE BY HASH (ca_address_sk);

-  Change the schema of a table.

   Set the schema of the **customer** table to **myschema**.

   ::

      CREATE SCHEMA myschema;
      ALTER TABLE customer SET SCHEMA myschema;

-  Add a primary key constraint or unique constraint based on the unique index.

   Create a unique index **customer_constraint1** for the customer table, add a primary key constraint based on the existing unique index, and rename the index.

   ::

      CREATE UNIQUE INDEX customer_constraint1 ON myschema.customer(C_CUSTKEY);
      ALTER TABLE myschema.customer ADD CONSTRAINT customer_constraint2 PRIMARY KEY USING INDEX customer_constraint1;

-  Rename a table constraint.

   ::

      ALTER TABLE myschema.customer RENAME CONSTRAINT customer_constraint2 TO customer_constraint;

-  Delete a table constraint.

   ::

      ALTER TABLE myschema.customer DROP CONSTRAINT customer_constraint;

-  Add a primary key constraint to a table.

   ::

      ALTER TABLE myschema.customer ADD PRIMARY KEY(C_CUSTKEY);

-  Add a partial cluster column to the column-store table **customer_address**.

   ::

      ALTER TABLE customer_address ADD CONSTRAINT customer_address_cluster PARTIAL CLUSTER KEY(ca_address_sk);

-  Delete the partial cluster column from the column-store table **customer_address**.

   ::

      ALTER TABLE customer_address DROP CONSTRAINT customer_address_cluster;

-  Modify the storage format of a column-store table. **Note**: The HStore tables in hybrid data warehouses do not support this example.

   ::

      ALTER TABLE customer_address SET (COLVERSION = 1.0);

-  Change the distribution mode of a table.

   ::

      ALTER TABLE customer_address DISTRIBUTE BY REPLICATION;

-  Rename a table.

   ::

      ALTER TABLE myschema.customer RENAME TO customer_t;

-  Modify the table cache policy (supported only in storage-compute decoupled 3.0 clusters).

   .. code-block::

      DROP TABLE IF EXISTS orders;
      CREATE TABLE orders(a int, b int);
      ALTER TABLE orders SET (cache_policy = 'NONE');

-  Disable the automatic partition deletion function of a table. For details about how to create a table, see :ref:`Example: Creating a Partitioned Table with Automatic Partition Management and Specified Partitions <en-us_topic_0000001764675414__section12898122016183>`.

   ::

      DROP TABLE IF EXISTS CPU;
      CREATE TABLE CPU(
          id integer,
          idle numeric,
          IO numeric,
          scope text,
          IP text,
          time timestamp
      ) with (TTL='7 days',PERIOD='1 day')
      partition by range(time)
      (
          PARTITION P1 VALUES LESS THAN('2022-01-05 16:32:45'),
          PARTITION P2 VALUES LESS THAN('2022-01-06 16:56:12')
      );

      ALTER TABLE CPU RESET (ttl);

Index Operation Example
-----------------------

-  Add an index to a table.

   ::

      ALTER TABLE myschema.customer_t ADD INDEX customer_index(C_CUSTKEY);

-  Delete a table index.

   ::

      ALTER TABLE myschema.customer_t DROP INDEX customer_index;
      ALTER TABLE myschema.customer_t ADD INDEX customer_index(C_CUSTKEY);
      ALTER TABLE myschema.customer_t DROP KEY customer_index;

-  Adds an index to a column in a table.

   ::

      ALTER TABLE myschema.customer_t ADD c_address_id varchar(20) CONSTRAINT ca_address_index CHECK (c_address_id > 0);

Hot and Cold Table Operation Example
------------------------------------

-  Do a cold or hot switchover for a single table.

   ::

      DROP TABLE IF EXISTS lifecycle_table;

      CREATE TABLE lifecycle_table(i int, val text) WITH (ORIENTATION = COLUMN, storage_policy = 'LMT:100')
      PARTITION BY RANGE (i)
      (
      PARTITION P1 VALUES LESS THAN(5),
      PARTITION P2 VALUES LESS THAN(10),
      PARTITION P3 VALUES LESS THAN(15),
      PARTITION P8 VALUES LESS THAN(MAXVALUE)
      )
      ENABLE ROW MOVEMENT;

      ALTER TABLE lifecycle_table REFRESH STORAGE;

-  Change a column-store partitioned table to a hot and cold table.

   ::

      DROP TABLE IF EXISTS test_1;
      CREATE TABLE test_1(id int,d_time date)
      WITH(ORIENTATION=COLUMN)
      DISTRIBUTE BY HASH (id)
      PARTITION BY RANGE (d_time)
      (PARTITION p1 START('2022-01-01') END('2022-01-31') EVERY(interval '1 day'));

      ALTER TABLE test_1 SET (storage_policy = 'LMT:100');

Column Operation Example
------------------------

Create the sample table **warehouse_t**.

::

   DROP TABLE IF EXISTS warehouse_t;
   CREATE TABLE warehouse_t
   (
       W_WAREHOUSE_SK            INTEGER                NOT NULL,
       W_WAREHOUSE_ID            CHAR(16)               NOT NULL,
       W_WAREHOUSE_NAME          VARCHAR(20)   UNIQUE DEFERRABLE,
       W_WAREHOUSE_SQ_FT         INTEGER                        ,
       W_COUNTY                  VARCHAR(30)                    ,
       W_STATE                   CHAR(2)            DEFAULT 'GA',
       W_ZIP                     CHAR(10)
   );

-  Adds a column.

   Adds a column to the **warehouse_t** table.

   ::

      ALTER TABLE warehouse_t ADD W_GOODS_CATEGORY int;

   Adds multiple columns to the **warehouse_t** table.

   ::

      ALTER TABLE warehouse_t ADD (W_COMMENT VARCHAR(117) NOT NULL, W_COUNT int);

-  Delete a column.

   Delete a column from the **warehouse_t** table.

   ::

      ALTER TABLE warehouse_t DROP COLUMN W_STATE;

-  Rename a column.

   Change the column name **W_ZIP** of the **warehouse_t** table to **new_W_ZIP**.

   ::

      ALTER TABLE warehouse_t RENAME W_ZIP TO new_W_ZIP;

-  Change the column name and column field information.

   Change the column name **W_GOODS_CATEGORY** of the **warehouse_t** table to **W_GOODS_CATEGORY2**, change the data type of the **W_GOODS_CATEGORY2** column to **DECIMAL**, and set the column constraint to **NOT NULL**.

   ::

      ALTER TABLE warehouse_t CHANGE W_GOODS_CATEGORY W_GOODS_CATEGORY2 DECIMAL NOT NULL COMMENT 'W_GOODS_CATEGORY';

-  Change the comment of a column.

   ::

      ALTER TABLE warehouse_t MODIFY W_GOODS_CATEGORY2 COMMENT 'W_GOODS_CATEGORY';

-  Change the data type of a column.

   Change the data type of the **warehouse_t** column and set the column constraint to **NOT NULL**.

   ::

      ALTER TABLE warehouse_t MODIFY W_WAREHOUSE_SQ_FT varchar(20) NOT NULL;

-  Add the **NOT NULL** constraint to a column.

   ::

      ALTER TABLE warehouse_t ALTER COLUMN W_COUNTY SET NOT NULL;

-  Add a primary key to the **warehouse_t** table.

   ::

      ALTER TABLE warehouse_t ADD PRIMARY KEY(w_warehouse_name);

-  Modify the **ON UPDATE** expression of a specified column in a table.

   Add a timestamp column with the **ON UPDATE** expression to the **warehouse_t** table. The timestamp of the column is automatically updated when an **UPDATE** operation is performed on the table.

   ::

      ALTER TABLE warehouse_t ADD COLUMN C_TIME timestamp on update current_timestamp;

   Delete the timestamp column with the **ON UPDATE** expression from the **warehouse_t** table.

   ::

      ALTER TABLE warehouse_t MODIFY COLUMN C_TIME timestamp on update NULL;

Helpful Links
-------------

:ref:`CREATE TABLE <dws_06_0177>`, :ref:`12.101-RENAME TABLE <dws_06_0276>`, and :ref:`DROP TABLE <dws_06_0208>`
