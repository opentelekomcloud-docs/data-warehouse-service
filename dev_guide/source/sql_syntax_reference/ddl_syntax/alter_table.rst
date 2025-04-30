:original_name: dws_06_0142.html

.. _dws_06_0142:

ALTER TABLE
===========

Function
--------

**ALTER TABLE** is used to modify tables, including modifying table definitions, renaming tables, renaming specified columns in tables, renaming table constraints, setting table schemas, enabling or disabling row-level access control, and adding or updating multiple columns.

Precautions
-----------

-  Only the owner of a table, a user granted with the ALTER permission for the table, or a system administrator has the permission to run the **ALTER TABLE** statement. To change the owner or schema of a table, you need to have ownership or system administrator privileges for the table and be a direct or indirect member of the new role.
-  The storage parameter **ORIENTATION** cannot be modified.
-  Currently, **SET SCHEMA** can only set schemas to user schemas. It cannot set a schema to a system internal schema.
-  Column-store tables support **PARTIAL CLUSTER KEY** but do not support table-level foreign key constraints. In 8.1.1 or later, column-store tables support the **PRIMARY KEY** constraint and table-level **UNIQUE** constraint in table creation.
-  A system column cannot be designated as a primary key in a row-store REPLICATION distributed table.
-  In a column-store table, you can perform **ADD COLUMN**, **ALTER TYPE**, **SET STATISTICS**, **DROP COLUMN** operations. The types of new and modified columns should be the :ref:`Data Types <dws_06_0008>` supported by column storage. The **USING** option of **ALTER TYPE** only supports constant expression and expression involved in the column.
-  The column constraints supported by column-store tables include **NULL**, **NOT NULL**, and **DEFAULT** constant values. Only the **DEFAULT** value can be modified (**SET DEFAULT** and **DROP DEFAULT**), and only the **NOT NULL** constraint can be deleted.
-  The **NOT NULL** constraint and **PRIMARY KEY** constraint can be added to existing column-store tables using **ALTER**. This constraint is supported by version 8.2.0 or later clusters.
-  When you modify the COLVERSION or **enable_delta** parameter of a column-store table, other ALTER operations cannot be performed.

-  Auto-increment columns cannot be added, or a column in which the **DEFAULT** value contains the nextval() expression cannot be added either.
-  Row-level access control cannot be enabled for HDFS tables, foreign tables, and temporary tables.
-  If you delete the PRIMARY KEY constraint by specifying the constraint name, the NOT NULL constraint is not deleted. You can manually delete the NOT NULL constraint as needed.
-  The **cold_tablespace** and **storage_policy** parameters of **ALTER RESET** cannot be used in OBS multi-temperature tables, and **COLVERSION** cannot be changed to **1.0** for such tables.
-  You can change a column-store table whose **COLVERSION** parameter is **2.0** to an OBS multi-temperature table. The **COLD_TABLESPACE** and **STORAGE_POLICY** parameters must be added.
-  You can use **ALTER TABLE** to change the values of **STORAGE_POLICY** for **RELOPTIONS**. After the cold/hot switchover policy is changed, the cold/hot attribute of the existing cold data will not change. The new policy takes effect for the next cold/hot switchover.
-  Distribution columns cannot be modified for global temporary tables.
-  Performing an **ALTER TABLE** operation on a table rebuilds it by dumping data into a new file and deleting the original file once the process is complete. It is important to remember that this can consume a significant amount of disk space for larger tables. When the disk space is insufficient, exercise caution when performing the **ALTER TABLE** operation on large tables to prevent the cluster from being read-only.

   -  Change the data type of a column.
   -  Add columns (including the oid column) to a row-store table.
   -  Modify **COLVERSION** for a column-store table.
   -  Specify the **DEFAULT** constant values for a column added to a column-store table. If the **DEFAULT** values contain volatile functions or are not **NULL** and do not belong to a specific data type, ensure they are correctly defined.

-  Do not specify a tablespace when running **ALTER TABLE** for an unlogged table. When **ALTER TABLE** is run for a non-unlogged table, the tablespace cannot be specified as **pg_unlogged**.
-  You cannot modify the **COLVERSION** of a V3 table (which is 3.0), and it is not possible to switch a non-V3 table to a V3 table (meaning that **COLVERSION** 2.0 cannot be changed to 3.0).

.. warning::

   -  Avoid performing **ALTER TABLE**, **ALTER TABLE PARTITION**, **DROP PARTITION**, and **TRUNCATE** operations during peak hours to prevent long SQL statements from blocking these operations or SQL services.
   -  For more information about development and design specifications, see "GaussDB(DWS) Development and Design Proposal" in the *GaussDB(DWS) Developer Guide*.

Syntax
------

-  **ALTER TABLE** modifies the definition of a table.

   ::

      ALTER TABLE [ IF EXISTS ] { table_name [*] | ONLY table_name | ONLY ( table_name ) }
          action [, ... ];

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

   .. note::

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

      -  **NO FORCE/FORCE ROW LEVEL SECURITY**

         Forcibly enables or disables row-level access control for a table.

         By default, the table owner is not affected by the row-level access control feature. However, if row-level access control is forcibly enabled, the table owner (excluding system administrators) will be affected. System administrators are not affected by any row-level access control policies.

      -  **REFRESH STORAGE**

         Changes the local hot partitions that meet the criteria specified in the **storage_policy** parameter of an OBS multi-temperature table to the cold partitions stored in the OBS.

         For example, if **storage_policy** is set to **'LMT:10'** for an OBS hot or cold table when it is created, the partitions that are not updated within the last 10 days are switched to cold partitions in the OBS.

   -  There are several clauses of **column_clause**:

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

      .. note::

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

            In this command, **anycol** indicates any column existing in the table and **anytype** indicates the type of the prototype of the column. **ALTER TYPE** does not change the table except that the table is forcibly rewritten. In this way, the data that is no longer used is deleted.

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

            The attribute option parameters are **n_distinct**, **n_distinct_inherited**, and **cstore_cu_sample_ratio**. **n_distinct** specifies and fixes the statistics of a table's distinct values. **n_distinct_inherited** specifies and inherits the distinct value statistics. **cstore_cu_sample_ratio** specifies the CU ratio for **ANALYZE** on a column-store table. Currently, the **n_distinct_inherited** parameter cannot be **SET** or **RESET**.

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

            Sets the storage mode for a column. This clause specifies whether this column is held inline or in a secondary TOAST table, and whether the data should be compressed. This statement can only be used for row-based tables. SET STORAGE only sets the strategy to be used for future table operations.

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

      **index_parameters** is as follows:

      ::

         [ WITH ( {storage_parameter = value} [, ... ] ) ]
             [ USING INDEX TABLESPACE tablespace_name ]

-  Changes the data type of an existing column in the table. Only the type conversion of the same category (between values, strings, and time) is allowed.

   .. code-block::

      ALTER TABLE [ IF EXISTS ] table_name
          MODIFY ( { column_name data_type | [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ] |
              [ CONSTRAINT constraint_name ] NULL | DEFAULT default_expr | COMMENT 'text' } [, ...] );

-  Rename the table. Changing the table name does not affect the stored data. The new table name can be prefixed with the schema name of the original table. The schema name cannot be changed at the same time.

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

      ALTER TABLE { table_name [*] | ONLY table_name | ONLY ( table_name ) }
          RENAME CONSTRAINT constraint_name TO new_constraint_name;

-  Set the schema of the table.

   ::

      ALTER TABLE [ IF EXISTS ] table_name
          SET SCHEMA new_schema;

   .. note::

      -  The schema setting moves the table into another schema. Associated indexes and constraints owned by table columns are migrated as well. Currently, the schema for sequences cannot be changed. If the table has sequences, delete the sequences, and create them again or delete the ownership between the table and sequences. In this way, the table schema can be changed.
      -  To change the schema of a table, you must also have CREATE privilege on the new schema. To add the table as a new child of a parent table, you must own the parent table as well. To alter the owner, you must also be a direct or indirect member of the new owning role, and that role must have CREATE permission on the table's schema. These restrictions enforce that altering the owner does not do anything you could not do by dropping and recreating the table. However, a system administrator can alter ownership of any table anyway.
      -  All the actions except for **RENAME** and **SET SCHEMA** can be combined into a list of multiple alterations to apply in parallel. For example, it is possible to add several columns or alter the type of several columns in a single command. This is useful with large tables, since only one pass over the table need be made.
      -  Adding a **CHECK** or **NOT NULL** constraint requires scanning the table to verify that existing rows meet the constraint.
      -  Adding a column with a non-null default or changing the type of an existing column will require the entire table to be rewritten. Table rebuilding may take a significant amount of time for a large table; and will temporarily require as much as double the disk space.

-  Add columns.

   ::

      ALTER TABLE [ IF EXISTS ] table_name
          ADD ( { column_name data_type [ compress_mode ] [ COLLATE collation ] [ column_constraint [ ... ] ]} [, ...] );

-  Update columns.

   ::

      ALTER TABLE [ IF EXISTS ] table_name
          MODIFY ( { column_name data_type | column_name [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ] | column_name [ CONSTRAINT constraint_name ] NULL } [, ...] );

.. _en-us_topic_0000001811634545__s3e87132692794964b56e3ba420e7b544:

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notification instead of an error if no tables have identical names. The notification prompts that the table you are querying does not exist.

-  **table_name [*] \| ONLY table_name \| ONLY ( table_name )**

   **table_name** is the name of table that you need to modify.

   If **ONLY** is specified, only the table is modified. If **ONLY** is not specified, the table and all subtables will be modified. You can add the asterisk (``*``) option following the table name to specify that all subtables are scanned, which is the default operation.

-  **constraint_name**

   Specifies the name of a constraint. The constraint name can contain a maximum of 63 characters.

-  **index_name**

   Specifies the name of this index.

-  **storage_parameter**

   Specifies the name of a storage parameter.

   The following options are added for partition management:

   -  **PERIOD** (interval type)

      Sets the period for automatically creating partitions in partition management.

      For details about the value range of **PERIOD** and the restrictions on enabling this function, see :ref:`▪PERIOD <en-us_topic_0000001764675414__li672910401685>`.

      .. note::

         -  If this parameter is not configured when you create a table, you can run the **set** statements to configure this parameter and enable automatic partition creation. If this parameter has been configured before, you can run the **set** statements to modify this parameter.
         -  You can run the **reset** command to disable the automatic partition creation. However, if the automatic partition deletion is enabled, the automatic partition creation cannot be disabled.

   -  **TTL** (interval type)

      Set the partition expiration time for automatically deleting partitions in partition management.

      For details about the TTL range and restrictions on enabling this function, see :ref:`▪TTL <en-us_topic_0000001764675414__li49277207810>`.

      .. note::

         -  If this parameter is not configured when you create a table, you can run the **set** statements to configure this parameter and enable automatic partition deletion. If this parameter has been configured before, you can run the **set** statements to modify this parameter.
         -  You can run the **reset** command to disable the automatic partition deletion.

   The following options are added to column-store tables in Turbo storage format:

   -  enable_turbo_store

      Specifies whether the column-store table is in the turbo storage format. This is supported only by 9.1.0.100 and later cluster versions.

      .. note::

         -  Common column-store tables in version 3.0 do not support the turbo storage format, while **hstore_opt** tables in version 3.0 only support the turbo storage format.
         -  In version 2.0, there is no restriction on column-store tables.

-  **new_owner**

   Specifies the name of the new table owner.

-  **new_tablespace**

   Specifies the new name of the tablespace to which the table belongs.

-  **column_name**, **column_1_name**, **column_2_name**

   Specifies the name of a new or an existing column.

-  **data_type**

   Specifies the type of a new column or a new type of an existing column.

-  **compress_mode**

   Specifies the compress options of the table, only available for row-based tables. The clause specifies the algorithm preferentially used by the column.

-  **collation**

   Specifies the collation rule name of a column. The optional **COLLATE** clause specifies a collation for the new column; if omitted, the collation is the default for the new column.

-  **USING expression**

   A **USING** clause specifies how to compute the new column value from the old; if omitted, the default conversion is an assignment cast from old data type to new. A **USING** clause must be provided if there is no implicit or assignment cast from the old to new type.

   .. note::

      **USING** in **ALTER TYPE** can specify any expression involving the old values of the row; that is, it can refer to any columns other than the one being converted. This allows very general conversions to be done with the **ALTER TYPE** syntax. Because of this flexibility, the **USING** expression is not applied to the column's default value (if any); the result might not be a constant expression as required for a default. This means that when there is no implicit or assignment cast from old to new type, **ALTER TYPE** might fail to convert the default even though a **USING** clause is supplied. In such cases, drop the default with **DROP DEFAULT**, perform the **ALTER TYPE**, and then use **SET DEFAULT** to add a suitable new default. Similar considerations apply to indexes and constraints involving the column.

-  **NOT NULL \| NULL**

   Sets whether the column allows null values.

-  **integer**

   Specifies the constant value of an integer with a sign. If **PERCENT** is used, the range of **integer** is from 0 to 100.

-  **attribute_option**

   Specifies an attribute option.

-  **PLAIN \| EXTERNAL \| EXTENDED \| MAIN**

   Specifies a column storage mode.

   -  **PLAIN** must be used for fixed-length values (such as integers). It must be inline and uncompressed.
   -  **MAIN** is for inline, compressible data.
   -  **EXTERNAL** is for external, uncompressed data. Use of **EXTERNAL** will make substring operations on **text** and **bytea** values run faster, at the penalty of increased storage space.
   -  **EXTENDED** is for external, compressed data. **EXTENDED** is the default for most data types that support non-**PLAIN** storage.

-  **CHECK ( expression )**

   New or updated rows must satisfy for an insert or update operation to succeed. Expressions evaluating to TRUE succeed. If any row of an insert or update operation produces a FALSE result, an error exception is raised and the insert or update does not alter the database.

   A check constraint specified as a column constraint should reference only the column's values, while an expression appearing in a table constraint can reference multiple columns.

   Currently, **CHECK** expression does not include subqueries and cannot use variables apart from the current column.

-  **DEFAULT default_expr**

   Assigns a default data value for a column.

   The data type of the default expression must match the data type of the column.

   The default expression will be used in any insert operation that does not specify a value for the column. If there is no default value for a column, then the default value is **NULL**.

   If a suffix operator, such as (!). is used in **default_expr**, enclose the operator in parentheses.

-  **UNIQUE [ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ] index_parameters**

   **UNIQUE ( column_name [, ... ] ) [ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ] index_parameters**

   The **UNIQUE** constraint specifies that a group of one or more columns of a table can contain only unique values.

   The **[ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ]** field is used to specify how to process null values in the index column of the Unique index.

   Default value: This parameter is left empty by default. NULL values can be inserted repeatedly.

   When the inserted data is compared with the original data in the table, the NULL value can be processed in any of the following ways:

   -  NULLS DISTINCT: NULL values are unequal and can be inserted repeatedly.
   -  NULLS NOT DISTINCT: NULL values are equal. If all index columns are NULL, NULL values cannot be inserted repeatedly. If some index columns are NULL, data can be inserted only when non-null values are different.
   -  NULLS IGNORE: NULL values are skipped during the equivalent comparison. If all index columns are NULL, NULL values can be inserted repeatedly. If some index columns are NULL, data can be inserted only when non-null values are different.

   The following table lists the behaviors of the three processing modes.

   .. table:: **Table 1** Processing of NULL values in index columns in unique indexes

      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | Constraint         | All Index Columns Are NULL     | Some Index Columns Are NULL.                                                                               |
      +====================+================================+============================================================================================================+
      | NULLS DISTINCT     | Can be inserted repeatedly.    | Can be inserted repeatedly.                                                                                |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | NULLS NOT DISTINCT | Cannot be inserted repeatedly. | Cannot be inserted if the non-null values are equal. Can be inserted if the non-null values are not equal. |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | NULLS IGNORE       | Can be inserted repeatedly.    | Cannot be inserted if the non-null values are equal. Can be inserted if the non-null values are not equal. |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+

-  **PRIMARY KEY index_parameters**

   **PRIMARY KEY ( column_name [, ... ] ) index_parameters**

   The primary key constraint specifies that a column or columns of a table can contain only unique (non-duplicate) and non-null values.

-  **DEFERRABLE \| NOT DEFERRABLE \| INITIALLY DEFERRED \| INITIALLY IMMEDIATE**

   Sets whether the constraint is deferrable. This option is unavailable to column-store tables.

   -  **DEFERRABLE**: deferrable can be postponed until the end of the transaction using the **SET CONSTRAINTS** command.
   -  **NOT DEFERRABLE**: checks immediately after the execution of each command.
   -  **INITIALLY IMMEDIATE**: checks immediately after the execution of each statement.
   -  **INITIALLY DEFERRED**: checks when the transaction ends.

-  **WITH ( {storage_parameter = value} [, ... ] )**

   Specifies an optional storage parameter for a table or an index.

-  **COMPRESS|NOCOMPRESS**

   -  **NOCOMPRESS**: If the **NOCOMPRESS** keyword is specified, the existing compression feature of the table is not changed.
   -  **COMPRESS**: If the **COMPRESS** keyword is specified, the table compression feature is triggered if tuples are inserted in a batch.

-  **new_table_name**

   Specifies the new table name.

-  **new_column_name**

   Specifies the new name of a specific column in a table.

-  **new_constraint_name**

   Specifies the new name of a table constraint.

-  **new_schema**

   Specifies the new schema name.

-  **CASCADE**

   Automatically drops objects that depend on the dropped column or constraint (for example, views referencing the column).

-  **RESTRICT**

   Refuses to drop the column or constraint if there are any dependent objects. This is the default behavior.

-  **schema_name**

   Specifies the schema name of a table.

-  **cache_policy**

   Table cache policy. This parameter is supported only in the storage-compute decoupling 3.0 version. For details about the value, see :ref:`cache_policy <en-us_topic_0000001764675138__en-us_topic_0000001342465185_li14679114513562>`.

Table Operation Examples
------------------------

Rename a table:

::

   ALTER TABLE CUSTOMER RENAME TO CUSTOMER_t;

Add a new table constraint:

::

   ALTER TABLE customer_address ADD PRIMARY KEY(ca_address_sk);

Adds primary key constraint or unique constraint based on the unique index.

Create an index **CUSTOMER_constraint1** for the table **CUSTOMER**. Then add primary key constraints, and rename the created index.

::

   CREATE UNIQUE INDEX CUSTOMER_constraint1 ON CUSTOMER(C_CUSTKEY);
   ALTER TABLE CUSTOMER ADD CONSTRAINT CUSTOMER_constraint2 PRIMARY KEY USING INDEX CUSTOMER_constraint1;

Rename a table constraint:

::

   ALTER TABLE CUSTOMER RENAME CONSTRAINT CUSTOMER_constraint2 TO CUSTOMER_constraint;

Delete a table constraint:

::

   ALTER TABLE CUSTOMER DROP CONSTRAINT CUSTOMER_constraint;

Add a table index:

::

   ALTER TABLE CUSTOMER ADD INDEX CUSTOMER_index(C_CUSTKEY);

Delete a table index:

::

   ALTER TABLE CUSTOMER DROP INDEX CUSTOMER_index;
   ALTER TABLE CUSTOMER DROP KEY CUSTOMER_index;

Add a partial cluster key to a column-store table:

::

   ALTER TABLE customer_address ADD CONSTRAINT customer_address_cluster PARTIAL CLUSTER KEY(ca_address_sk);

Delete a partial cluster column from the column-store table.

::

   ALTER TABLE customer_address DROP CONSTRAINT customer_address_cluster;

Switch the storage format of a column-store table:

::

   ALTER TABLE customer_address SET (COLVERSION = 1.0);

Change the distribution mode of a table:

::

   ALTER TABLE customer_address DISTRIBUTE BY REPLICATION;

Change the schema of a table:

::

   ALTER TABLE customer_address SET SCHEMA tpcds;

Change the data temperature for a single table:

::

   ALTER TABLE cold_hot_table REFRESH STORAGE;

Change a column-store partitioned table to a table that supports hot and cold data separation.

::

   CREATE table test_1(id int,d_time date)
   WITH(ORIENTATION=COLUMN)
   DISTRIBUTE BY HASH (id)
   PARTITION BY RANGE (d_time)
   (PARTITION p1 START('2022-01-01') END('2022-01-31') EVERY(interval '1 day'))

   ALTER TABLE test_1 SET (storage_policy = 'LMT:100');

Modify the table cache policy (supported only in clusters of the storage-compute decoupling 3.0 version).

.. code-block::

   ALTER TABLE orders SET (cache_policy = 'NONE');

Column Operation Examples
-------------------------

Add a column to a table.

::

   ALTER TABLE warehouse_t ADD W_GOODS_CATEGORY int;

Modify the column name and column field information in the table.

::

   ALTER TABLE warehouse_t CHANGE W_GOODS_CATEGORY W_GOODS_CATEGORY2 DECIMAL NOT NULL COMMENT 'W_GOODS_CATEGORY';

Add a primary key to a table.

::

   ALTER TABLE warehouse_t ADD PRIMARY KEY(w_warehouse_name);

Rename a column.

::

   ALTER TABLE CUSTOMER RENAME C_PHONE TO new_C_PHONE;

Add columns to a table.

::

   ALTER TABLE CUSTOMER ADD (C_COMMENT VARCHAR(117) NOT NULL, C_COUNT int);

Change the data type of a column in the table and set the column constraint to **NOT NULL**.

::

   ALTER TABLE CUSTOMER MODIFY C_MKTSEGMENT varchar(20) NOT NULL;

Add the **NOT NULL** constraint to a column in the table.

::

   ALTER TABLE CUSTOMER ALTER COLUMN C_PHONE SET NOT NULL;

Delete a column from a table.

::

   ALTER TABLE CUSTOMER DROP COLUMN C_COUNT;

Add an index to a column in the table.

::

   ALTER TABLE customer_address MODIFY ca_address_id varchar(20) CONSTRAINT ca_address_index CHECK (ca_address_id > 0);

Add a timestamp column with the **ON UPDATE** expression to the **customer_address** table.

::

   ALTER TABLE customer_address ADD COLUMN C_TIME timestamp on update current_timestamp;

Delete the timestamp column with the **ON UPDATE** expression from the **customer_address**.

::

   ALTER TABLE customer_address MODIFY COLUMN C_TIME timestamp on update NULL;

Helpful Links
-------------

:ref:`CREATE TABLE <dws_06_0177>`, :ref:`12.101-RENAME TABLE <dws_06_0276>`, and :ref:`DROP TABLE <dws_06_0208>`
