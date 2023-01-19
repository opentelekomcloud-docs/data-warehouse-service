:original_name: dws_06_0142.html

.. _dws_06_0142:

ALTER TABLE
===========

Function
--------

**ALTER TABLE** is used to modify tables, including modifying table definitions, renaming tables, renaming specified columns in tables, renaming table constraints, setting table schemas, enabling or disabling row-level access control, and adding or updating multiple columns.

Important Notes
---------------

-  You must own the table to use **ALTER TABLE**. A system administrator has the permission by default.
-  The storage parameter **ORIENTATION** cannot be modified.
-  Currently, **SET SCHEMA** can only be used to set a schema to a user schema, not to a system internal schema.
-  Column-store tables support **PARTIAL CLUSTER KEY** but do not support table-level foreign key constraints. In 8.1.1 or later, column-store tables support the **PRIMARY KEY** constraint and table-level **UNIQUE** constraint.
-  In a column-store table, you can perform **ADD COLUMN**, **ALTER TYPE**, **SET STATISTICS**, **DROP COLUMN** operations. The types of new and modified columns should be the :ref:`Data Types <dws_06_0008>` supported by column storage. The **USING** option of **ALTER TYPE** only supports constant expression and expression involved in the column.
-  The column constraints supported by column-store tables include **NULL**, **NOT NULL**, and **DEFAULT** constant values. Only the **DEFAULT** value can be modified (**SET DEFAULT** and **DROP DEFAULT**), and only the **NOT NULL** constraint can be deleted. Currently, **NULL** and **NOT NULL** constraints cannot be modified.
-  When you modify the COLVERSION or **enable_delta** parameter of a column-store table, other ALTER operations cannot be performed.

-  Auto-increment columns cannot be added, or a column in which the **DEFAULT** value contains the nextval() expression cannot be added either.
-  Row-level access control cannot be enabled for HDFS tables, foreign tables, and temporary tables.
-  If you delete the PRIMARY KEY constraint by specifying the constraint name, the NOT NULL constraint is not deleted. You can manually delete the NOT NULL constraint as needed.
-  The **cold_tablespace** and **storage_policy** parameters of **ALTER RESET** cannot be used in OBS hot or cold tables, and **COLVERSION** cannot be changed to **1.0** for such tables.
-  You can change a column-store table whose **COLVERSION** parameter is **2.0** to an OBS hot or cold table. The **COLD_TABLESPACE** and **STORAGE_POLICY** parameters must be added.
-  You can use **ALTER TABLE** to change the values of **STORAGE_POLICY** for **RELOPTIONS**. After the cold/hot switchover policy is changed, the cold/hot attribute of the existing cold data will not change. The new policy takes effect for the next cold/hot switchover.

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
          | DISTRIBUTE BY { REPLICATION | { HASH ( column_name [,...] ) } }
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

         Adds a new table constraint.

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

      -  **DISTRIBUTE BY { REPLICATION \| { HASH ( column_name [,...] ) } }**

         Changing a table's distribution mode will physically redistribute the table data based on the new distribution mode. After the distribution mode is changed, you are advised to manually run the **ANALYZE** statement to collect new statistics about the table.

         .. note::

            -  This operation is a major change operation, involving table distribution information modification and physical data redistribution. During the modification, services are blocked. After the modification, the original execution plan of services will change. Perform this operation according to the standard change process.
            -  This operation is a resource-intensive operation. If you need to modify the distribution mode of large tables, perform the operation when the computing and storage resources are sufficient. Ensure that the remaining space of the entire cluster and the tablespace where the original table is located is sufficient to store a table that has the same size as the original table and is distributed in the new distribution mode.

      -  **TO { GROUP groupname \| NODE ( nodename [, ... ] ) }**

         The syntax is only available in extended mode (when GUC parameter **support_extended_features** is **on**). Exercise caution when enabling the mode. It is used for tools like internal dilatation tools. Common users should not use the mode.

      -  **ADD NODE ( nodename [, ... ] )**

         It is only available for tools like internal dilatation. General users should not use the mode.

      -  **DELETE NODE ( nodename [, ... ] )**

         It is only available for internal scale-in tools. Common users should not use the syntax.

      -  **DISABLE TRIGGER [ trigger_name \| ALL \| USER ]**

         Disables a single trigger specified by **trigger_name**, disables all triggers, or disables only user triggers (excluding internally generated constraint triggers, for example, deferrable unique constraint triggers and exclusion constraints triggers).

         .. note::

            Exercise caution when using this function because data integrity cannot be ensured as expected if the triggers are not executed.

      -  **\| ENABLE TRIGGER [ trigger_name \| ALL \| USER ]**

         Enables a single trigger specified by **trigger_name**, enables all triggers, or enables only user triggers.

      -  **\| ENABLE REPLICA TRIGGER trigger_name**

         Determines that the trigger firing mechanism is affected by the configuration variable **session_replication_role**. When the replication role is **origin** (default value) or **local**, a simple trigger is fired.

         When **ENABLE REPLICA** is configured for a trigger, it is fired only when the session is in **replica** mode.

      -  **\| ENABLE ALWAYS TRIGGER trigger_name**

         Determines that all triggers are fired regardless of the current replication mode.

      -  **\| DISABLE/ENABLE ROW LEVEL SECURITY**

         Enables or disables row-level access control for a table.

         If row-level access control is enabled for a data table but no row-level access control policy is defined, the row-level access to the data table is not affected. If row-level access control for a table is disabled, the row-level access to the table is not affected even if a row-level access control policy has been defined. For details, see :ref:`CREATE ROW LEVEL SECURITY POLICY <dws_06_0169>`.

      -  **\|** **NO FORCE/FORCE ROW LEVEL SECURITY**

         Forcibly enables or disables row-level access control for a table.

         By default, the table owner is not affected by the row-level access control feature. However, if row-level access control is forcibly enabled, the table owner (excluding system administrators) will be affected. System administrators are not affected by any row-level access control policies.

      -  **\|** **REFRESH STORAGE**

         Changes the local hot partitions that meet the criteria defined by the rules specified in the **storage_policy** parameter of an OBS hot or cold table to the cold partitions stored in the OBS.

         For example, if **storage_policy** is set to **'LMT:10'** for an OBS hot or cold table when it is created, the partitions that are not updated within the last 10 days are switched to cold partitions in the OBS.

   -  There are several clauses of **column_clause**:

      ::

         ADD [ COLUMN ] column_name data_type [ compress_mode ] [ COLLATE collation ] [ column_constraint [ ... ] ]
         | MODIFY column_name data_type
         | MODIFY column_name [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ]
         | MODIFY column_name [ CONSTRAINT constraint_name ] NULL
         | DROP [ COLUMN ] [ IF EXISTS ] column_name [ RESTRICT | CASCADE ]
         | ALTER [ COLUMN ] column_name [ SET DATA ] TYPE data_type [ COLLATE collation ] [ USING expression ]
         | ALTER [ COLUMN ] column_name { SET DEFAULT expression | DROP DEFAULT }
         | ALTER [ COLUMN ] column_name { SET | DROP } NOT NULL
         | ALTER [ COLUMN ] column_name SET STATISTICS [PERCENT] integer
         | ADD STATISTICS (( column_1_name, column_2_name [, ...] ))
         | DELETE STATISTICS (( column_1_name, column_2_name [, ...] ))
         | ALTER [ COLUMN ] column_name SET ( {attribute_option = value} [, ... ] )
         | ALTER [ COLUMN ] column_name RESET ( attribute_option [, ... ] )
         | ALTER [ COLUMN ] column_name SET STORAGE { PLAIN | EXTERNAL | EXTENDED | MAIN }

      .. note::

         -  **ADD [ COLUMN ] column_name data_type [ compress_mode ] [ COLLATE collation ] [ column_constraint [ ... ] ]**

            Adds a column to a table. If a column is added with **ADD COLUMN**, all existing rows in the table are initialized with the column's default value (**NULL** if no **DEFAULT** clause is specified).

         -  **ADD ( { column_name data_type [ compress_mode ] } [, ...] )**

            Adds columns in the table.

         -  **MODIFY column_name data_type**

            Change the data type of an existing field in the table. Only the type conversion of the same category (between values, character strings, and time) is allowed.

         -  **MODIFY column_name [ CONSTRAINT constraint_name ] NOT NULL [ ENABLE ]**

            Adds a NOT NULL constraint to a column of a table. Currently, this clause is unavailable to column-store tables.

         -  **MODIFY column_name [ CONSTRAINT constraint_name ] NULL**

            Deletes the NOT NULL constraint to a certain column in the table.

         -  **DROP [ COLUMN ] [ IF EXISTS ] column_name [ RESTRICT \| CASCADE ]**

            Drops a column from a table. Index and constraint related to the column are automatically dropped. If an object not belonging to the table depends on the column, **CASCADE** must be specified, such as foreign key reference and view.

            The **DROP COLUMN** form does not physically remove the column, but simply makes it invisible to SQL operations. Subsequent insert and update operations in the table will store a **NULL** value for the column. Therefore, column deletion takes a short period of time but does not immediately release the table space on the disks, because the space occupied by the deleted column is not reclaimed. The space will be reclaimed when **VACUUM** is executed.

         -  **ALTER [ COLUMN ] column_name [ SET DATA ] TYPE data_type [ COLLATE collation ] [ USING expression ]**

            To change the data type of a table column (data in the distribution column is not allowed to change types), only the type conversion of the same category (between values, strings, and time) is allowed. Indexes and simple table constraints on the column will automatically use the new data type by reparsing the originally supplied expression.

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

         -  **{ADD \| DELETE} STATISTICS ((column_1_name, column_2_name [, ...]))**

            Adds or deletes the declaration of collecting multi-column statistics to collect multi-column statistics as needed when **ANALYZE** is performed for a table or a database. The statistics about a maximum of 32 columns can be collected at a time. You are not allowed to add or delete the declaration for system tables or foreign tables

         -  **ALTER [ COLUMN ] column_name SET ( {attribute_option = value} [, ... ] )**

            **ALTER [ COLUMN ] column_name RESET ( attribute_option [, ... ] )**

            Sets or resets per-attribute options.

            Currently, the only defined per-attribute options are **n_distinct** and **n_distinct_inherited**. **n_distinct** affects statistics of table, while **n_distinct_inherited** affects the statistics of table and its subtables. Currently, only **SET/RESET n_distinct** is supported, and **SET/RESET n_distinct_inherited** is forbidden.

         -  **ALTER [ COLUMN ] column_name SET STORAGE { PLAIN \| EXTERNAL \| EXTENDED \| MAIN }**

            Sets the storage mode for a column. This clause specifies whether this column is held inline or in a secondary TOAST table, and whether the data should be compressed. This statement can only be used for row-based tables. SET STORAGE only sets the strategy to be used for future table operations.

      -  **column_constraint** is as follows:

         ::

            [ CONSTRAINT constraint_name ]
                { NOT NULL |
                  NULL |
                  CHECK ( expression ) |
                  DEFAULT default_expr  |
                  UNIQUE index_parameters |
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

   -  **table_constraint** is as follows:

      ::

         [ CONSTRAINT constraint_name ]
             { CHECK ( expression ) |
               UNIQUE ( column_name [, ... ] ) index_parameters |
               PRIMARY KEY ( column_name [, ... ] ) index_parameters }

             [ DEFERRABLE | NOT DEFERRABLE | INITIALLY DEFERRED | INITIALLY IMMEDIATE ]

      **index_parameters** is as follows:

      ::

         [ WITH ( {storage_parameter = value} [, ... ] ) ]
             [ USING INDEX TABLESPACE tablespace_name ]

-  Rename the table. The renaming does not affect stored data. The new table name cannot be prefixed with the schema name of the original table.

   ::

      ALTER TABLE [ IF EXISTS ] table_name
          RENAME TO new_table_name;

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

.. _en-us_topic_0000001145910731__s3e87132692794964b56e3ba420e7b544:

Parameter Description
---------------------

-  **IF EXISTS**

   Sends a notification instead of an error if no tables have identical names. The notification prompts that the table you are querying does not exist.

-  **table_name [*] \| ONLY table_name \| ONLY ( table_name )**

   **table_name** is the name of table that you need to modify.

   If **ONLY** is specified, only the table is modified. If **ONLY** is not specified, the table and all subtables will be modified. You can add the asterisk (``*``) option following the table name to specify that all subtables are scanned, which is the default operation.

-  **constraint_name**

   Specifies the name of an existing constraint to drop.

-  **index_name**

   Specifies the name of this index.

-  **storage_parameter**

   Specifies the name of a storage parameter.

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

-  **UNIQUE index_parameters**

   **UNIQUE ( column_name [, ... ] ) index_parameters**

   The **UNIQUE** constraint specifies that a group of one or more columns of a table can contain only unique values.

-  **PRIMARY KEY index_parameters**

   **PRIMARY KEY ( column_name [, ... ] ) index_parameters**

   The primary key constraint specifies that one or more columns of a table must contain unique (non-duplicate) and non-null values. This parameter is valid only for columns with the **NOT NULL** constraint.

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

Example 1: Operations on Tables
-------------------------------

Move a table to another schema.

::

   ALTER TABLE tpcds.warehouse_t19 SET SCHEMA joe;

When renaming an existing table, the new table name cannot be prefixed with the schema name of the original table.

::

   ALTER TABLE joe.warehouse_t19 RENAME TO warehouse_t23;

Change the distribution mode of the **tpcds.warehouse_t22** table to **REPLICATION**.

::

   ALTER TABLE tpcds.warehouse_t22 DISTRIBUTE BY REPLICATION;

Change the distribution column of the **tpcds.warehouse_t22** table to **W_WAREHOUSE_SK**.

::

   ALTER TABLE tpcds.warehouse_t22 DISTRIBUTE BY HASH(W_WAREHOUSE_SK);

Switch the storage format of a column-store table.

::

   ALTER TABLE tpcds.warehouse_t18 SET (COLVERSION = 1.0);

Disable the delta table function of the column-store table.

::

   ALTER TABLE tpcds.warehouse_t21 SET (ENABLE_DELTA = OFF);

Disable the **SKIP_FPI_HINT** function of the table.

::

   ALTER TABLE tpcds.warehouse_t22 SET (SKIP_FPI_HINT = FALSE);

Change the data temperature for a single table.

::

   ALTER TABLE tpcds.warehouse_t23 REFRESH STORAGE;

Change the data temperature for multiple tables in batches.

.. code-block::

   SELECT pg_refresh_storage();

Example 2: Operations on Table Constraints
------------------------------------------

Create an index **ds_warehouse_t1_index1** for the table **tpcds.warehouse_t1**. Then add primary key constraints, and rename the created index.

::

   CREATE UNIQUE INDEX ds_warehouse_t1_index1 ON tpcds.warehouse_t1(W_WAREHOUSE_SK);
   ALTER TABLE tpcds.warehouse_t1 ADD CONSTRAINT ds_warehouse_t1_index2 PRIMARY KEY USING INDEX ds_warehouse_t1_index1;

Delete the primary key **ds_warehouse_t1_index2** from the table **tpcds.warehouse_t1**.

::

   ALTER TABLE tpcds.warehouse_t1 DROP CONSTRAINT ds_warehouse_t1_index2;

If no partial clusters have been specified in a column-store table, add a partial cluster to the table.

::

   ALTER TABLE tpcds.warehouse_t17 ADD PARTIAL CLUSTER KEY(W_WAREHOUSE_SK);

Delete a partial cluster column from the column-store table.

::

   ALTER TABLE tpcds.warehouse_t17 DROP CONSTRAINT warehouse_t17_cluster;

Add a Not-Null constraint to an existing column.

::

   ALTER TABLE tpcds.warehouse_t19 ALTER COLUMN W_GOODS_CATEGORY SET NOT NULL;

Remove Not-Null constraints from an existing column.

::

   ALTER TABLE tpcds.warehouse_t19 ALTER COLUMN W_GOODS_CATEGORY DROP NOT NULL;

Add a check constraint to the **tpcds.warehouse_t19** table.

::

   ALTER TABLE tpcds.warehouse_t19 ADD CONSTRAINT W_CONSTR_KEY4 CHECK (W_STATE <> '');

Example 3: Operations on Columns
--------------------------------

Add a primary key to the **tpcds.warehouse_t1 table**.

::

   ALTER TABLE tpcds.warehouse_t1 ADD PRIMARY KEY(W_WAREHOUSE_SK);

Add a varchar column to the **tpcds.warehouse_t19** table.

::

   ALTER TABLE tpcds.warehouse_t19 ADD W_GOODS_CATEGORY varchar(30);

Use one statement to alter the types of two existing columns.

::

   ALTER TABLE tpcds.warehouse_t19
   ALTER COLUMN W_GOODS_CATEGORY TYPE varchar(80),
   ALTER COLUMN W_STREET_NAME TYPE varchar(100);

This statement is equivalent to the preceding statement.

::

   ALTER TABLE tpcds.warehouse_t19 MODIFY (W_GOODS_CATEGORY varchar(30), W_STREET_NAME varchar(60));

Delete a column from the **tpcds.warehouse_t23** table.

::

   ALTER TABLE tpcds.warehouse_t23 DROP COLUMN W_STREET_NAME;

Links
-----

:ref:`CREATE TABLE <dws_06_0177>`, :ref:`DROP TABLE <dws_06_0208>`
