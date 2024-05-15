:original_name: dws_06_0341.html

.. _dws_06_0341:

System Catalog Information Functions
====================================

format_type(type_oid, typemod)
------------------------------

Description: Gets SQL name of a data type.

Return type: text

Note:

**format_type** returns the SQL name of a data type that is identified by its type OID and possibly a type modifier. Pass NULL for the type modifier if no specific modifier is known. Certain type modifiers are passed for data types with length limitations. The SQL name returned from **format_type** contains the length of the data type, which can be calculated by taking sizeof(int32) from actual storage length [actual storage len - sizeof(int32)] in the unit of bytes. 32-bit space is required to store the customized length set by users. So the actual storage length contains 4 bytes more than the customized length. In the following example, the SQL name returned from **format_type** is character varying(6), indicating the length of varchar type is 6 bytes. So the actual storage length of varchar type is 10 bytes.

::

   SELECT format_type((SELECT oid FROM pg_type WHERE typname='varchar'), 10);
        format_type
   ----------------------
    character varying(6)
   (1 row)

pg_check_authid(role_oid)
-------------------------

Description: Check whether a role name with given OID exists.

Return type: Boolean

pg_describe_object(catalog_id, object_id, object_sub_id)
--------------------------------------------------------

Description: Gets description of a database object.

Return type: text

Note: **pg_describe_object** returns a description of a database object specified by catalog OID, object OID and a (possibly zero) sub-object ID. This is useful to determine the identity of an object as stored in the **pg_depend** catalog.

pg_get_constraintdef(constraint_oid)
------------------------------------

Description: Gets definition of a constraint.

Return type: text

pg_get_constraintdef(constraint_oid, pretty_bool)
-------------------------------------------------

Description: Gets definition of a constraint.

Return type: text

Note: **pg_get_constraintdef** and **pg_get_indexdef** respectively reconstruct the creating command for a constraint and an index.

pg_get_expr(pg_node_tree, relation_oid)
---------------------------------------

Description: Decompiles internal form of an expression, assuming that any Vars in it refer to the relationship indicated by the second parameter.

Return type: text

pg_get_expr(pg_node_tree, relation_oid, pretty_bool)
----------------------------------------------------

Description: Decompiles internal form of an expression, assuming that any Vars in it refer to the relationship indicated by the second parameter.

Return type: text

Note: **pg_get_expr** decompiles the internal form of an individual expression, such as the default value for a column. It can be useful when examining the contents of system catalogs. If the expression might contain Vars, specify the OID of the relationship they refer to as the second parameter; if no Vars are expected, zero is sufficient.

pg_get_indexdef(index_oid)
--------------------------

Description: Gets **CREATE INDEX** command for index.

Return type: text

**index_oid** indicates the index OID, which can be queried in the PG_STATIO_ALL_INDEXES system view.

Example: Query the OID and CREATE INDEX command of **index ds_ship_mode_t1_index1**.

::

   SELECT indexrelid FROM PG_STATIO_ALL_INDEXES WHERE indexrelname = 'ds_ship_mode_t1_index1';
    indexrelid
   ------------
        136035
   (1 row)
   SELECT * FROM pg_get_indexdef(136035);
                                                   pg_get_indexdef
   ---------------------------------------------------------------------------------------------------------------
    CREATE INDEX ds_ship_mode_t1_index1 ON tpcds.ship_mode_t1 USING psort (sm_ship_mode_sk) TABLESPACE pg_default
   (1 row)

pg_get_indexdef(index_oid, column_no, pretty_bool)
--------------------------------------------------

Description: Gets **CREATE INDEX** command for index, or definition of just one index column when **column_no** is not zero.

Return type: text

::

   SELECT * FROM pg_get_indexdef(136035,0,false);
                                                   pg_get_indexdef
   ---------------------------------------------------------------------------------------------------------------
    CREATE INDEX ds_ship_mode_t1_index1 ON tpcds.ship_mode_t1 USING psort (sm_ship_mode_sk) TABLESPACE pg_default
   (1 row)
   SELECT * FROM pg_get_indexdef(136035,1,false);
    pg_get_indexdef
   -----------------
    sm_ship_mode_sk
   (1 row)

pg_get_keywords()
-----------------

Description: Gets list of SQL keywords and their categories.

Return type: SETOF record

Note: **pg_get_keywords** returns a set of records describing the SQL keywords recognized by the server. The **word** column contains the keyword. The **catcode** column contains a category code: **U** for unreserved, **C** for column name, **T** for type or function name, or **R** for reserved. The **catdesc** column contains a possibly-localized string describing the category.

pg_get_ruledef(rule_oid)
------------------------

Description: Gets **CREATE RULE** command for a rule.

Return type: text

pg_get_ruledef(rule_oid, pretty_bool)
-------------------------------------

Description: Gets **CREATE RULE** command for a rule.

Return type: text

pg_get_userbyid(role_oid)
-------------------------

Description: Gets role name with given OID.

Return type: name

Note: **pg_get_userbyid** extracts a role's name given its OID.

pg_get_viewdef(viewname text [, pretty bool [, fullflag bool]])
---------------------------------------------------------------

Description: gets underlying **SELECT** command for views.

Return type: text

Note:

-  **pg_get_viewdef** reconstructs the **SELECT** query that defines a view. If the value of **pretty bool** is set to **true**, the display format is suitable for printing and more readable. The default value of **pretty bool** is **false**, and the display format is not readable. Use the default format for dump purposes whenever possible. The **pretty bool** parameter can be applied only to valid views.
-  When **fullflag bool** is set to **true**, the complete definition of the view is displayed. The default value is **false**.

pg_get_viewdef(viewoid oid [, pretty bool [, fullflag bool]])
-------------------------------------------------------------

Description: gets underlying **SELECT** command for views.

Return type: text

pg_get_viewdef(view_oid, wrap_column_int)
-----------------------------------------

Description: Gets underlying SELECT command for view, wrapping lines with columns as specified, printing is implied.

Return type: text

pg_get_tabledef(table_oid)
--------------------------

Description: Obtains a table definition based on **table_oid**.

Return type: text

Example: Obtain the OID of the table **customer_t2** from the system catalog **pg_class**, and then use this function to query the definition of **customer_t2** to obtain the table columns, storage mode (row-store or column-store), and table distribution mode configured for **customer_t2** when it is created.

::

   SELECT oid FROM pg_class WHERE relname ='customer_t2';
     oid
   -------
    17353
   (1 row)

   SELECT * FROM pg_get_tabledef(17353);
                 pg_get_tabledef
   --------------------------------------------
    SET search_path = dbadmin;                +
    CREATE  TABLE customer_t2 (               +
            state_id character(2),            +
            state_name character varying(40), +
            area_id numeric                   +
    )                                         +
    WITH (orientation=column, compression=low)+
    DISTRIBUTE BY HASH(state_id)              +
    TO GROUP group_version1;
   (1 row)

pg_get_tabledef(table_name)
---------------------------

Description: Obtains a table definition based on **table_name**.

Return type: text

Remarks: **pg_get_tabledef** reconstructs the **CREATE** statement of the table definition, including the table definition, index information, and comments. Users need to create the dependent objects of the table, such as groups, schemas, tablespaces, and servers. The table definition does not include the statements for creating these dependent objects.

pg_options_to_table(reloptions)
-------------------------------

Description: Gets the set of storage option name/value pairs.

Return type: SETOF record

Note: **pg_options_to_table** returns the set of storage option name/value pairs (**option_name**/**option_value**) when passing **pg_class.reloptions** or **pg_attribute.attoptions**.

pg_typeof(any)
--------------

Description: Gets the data type of any value.

Return type: regtype

Note:

**pg_typeof** returns the OID of the data type of the value that is passed to it. This can be helpful for troubleshooting or dynamically constructing SQL queries. The function is declared as returning **regtype**, which is an OID alias type (see :ref:`Object Identifier Types <dws_06_0022>`). This means that it is the same as an OID for comparison purposes but displays as a type name.

Example:

::

   SELECT pg_typeof(33);
    pg_typeof
   -----------
    integer
   (1 row)

   SELECT typlen FROM pg_type WHERE oid = pg_typeof(33);
    typlen
   --------
         4
   (1 row)

collation for (any)
-------------------

Description: Gets the collation of the parameter.

Return type: text

Note:

The expression **collation for** returns the collation of the value that is passed to it. Example:

::

   SELECT collation for (description) FROM pg_description LIMIT 1;
    pg_collation_for
   ------------------
    "default"
   (1 row)

The value might be quoted and schema-qualified. If no collation is derived for the argument expression, then a null value is returned. If the parameter is not of a collectable data type, then an error is thrown.

getdistributekey(table_name)
----------------------------

Description: Gets a distribution column for a hash table.

Return type: text

Example:

::

   SELECT getdistributekey('item');
    getdistributekey
   ------------------
    i_item_sk
   (1 row)

table_skewness(text)
--------------------

Description: Queries the percentage of table data among all nodes.

Parameter: Indicates that the type of the name of the to-be-queried table is text.

Return type: record

table_skewness(table_name text, column_name text[, row_num text])
-----------------------------------------------------------------

Description: Queries the proportion of column data distributed on each node based on the hash distribution rule. The results are sorted based on the data volumes of the nodes.

Parameters: **table_name** indicates a table name, **column_name** indicates a column name, and **row_num** indicates that all data in the current column is returned. The default value is **0**. A value other than **0** indicates the number of data records whose statistics are sampled. (Records are randomly sampled.)

Return type: record

Example:

Distribute data by hash based on the column **a** in the **tx** table. Seven records are distributed on DN 1, two records on DN 2, and one record on DN 0.

::

   SELECT * FROM table_skewness('tx','a');
    seqnum | num |  ratio
   --------+-----+----------
    1      | 7   | 70.000%
    2      | 2   | 20.000%
    0      | 1   | 10.000%
   (3 row)

table_data_skewness(data_row record, locatorType "char")
--------------------------------------------------------

Description: Calculates the bucket distribution index for the records concatenated using the columns in a specified table.

Parameters: **data_row** indicates the record concatenated using columns in the specified table. **locatorType** indicates the distribution rule. You are advised to set **locatorType** to **H**, indicating hash distribution.

Return type: smallint

Example:

Calculates the bucket distribution index based on the hash distribution rule for the records combined concatenated using the columns in the **tx** table.

::

   SELECT a, table_data_skewness(row(a), 'H') FROM tx;
    a | table_data_skewness
   ---+---------------------
    3 |                   0
    6 |                   2
    7 |                   2
    4 |                   1
    5 |                   1
   (5 rows)

table_distribution(schemaname text, tablename text)
---------------------------------------------------

Description: Queries the storage space occupied by a specified table on each node.

Parameter: Indicates that the types of the schema name and table name for the table to be queried are both text.

Return type: record

.. note::

   -  To query for the storage distribution of a specified table by using this function, you must have the **SELECT** permission for the table.
   -  The performance of **table_distribution** is better than that of **table_skewness**. Especially in a large cluster with a large amount of data, **table_distribution** is recommended.
   -  When you use **table_distribution** and want to view the space usage, you can use **dnsize** or **(sum(dnsize) over ())** to view the percentage.

table_distribution(regclass)
----------------------------

Description: Queries the storage space occupied by a specified table on each node.

Parameter: Indicates the name or OID of the table to be queried. The table name can be defined by the schema name. Parameter type: regclass

Return type: record

.. note::

   -  To query for the storage distribution of a specified table by using this function, you must have the **SELECT** permission for the table.
   -  The performance of **table_distribution** is better than that of **table_skewness**. Especially in a large cluster with a large amount of data, **table_distribution** is recommended.
   -  When you use **table_distribution** and want to view the space usage, you can use **dnsize** or **(sum(dnsize) over ())** to view the percentage.

table_distribution()
--------------------

Description: Queries the storage distribution of all tables in the current database.

Return type: record

.. note::

   -  This function involves the query for information about all tables in the database. To execute this function, you must have the administrator rights or rights of the preset role **gs_role_read_all_stats**.

   Based on the table_distribution() function, GaussDB(DWS) provides the PGXC_GET_TABLE_SKEWNESS view as an alternative way to query for data skew. You are advised to use this view when the number of tables in the database is less than 10000.

Example:

::

   SELECT * FROM table_distribution();
        schemaname     |        tablename        |   nodename   | dnsize
   --------------------+-------------------------+--------------+--------
    scheduler          | pg_task                 | dn_6005_6006 |   8192
    public             | ocr_group               | dn_6005_6006 |   8192
    public             | ocr_item                | dn_6005_6006 |   8192
    sea                | ocr_group               | dn_6005_6006 |  16384
    sea                | ocr_item                | dn_6005_6006 |  16384
    public             | customer_t1             | dn_6005_6006 |  16384
    dbms_om            | gs_wlm_session_info     | dn_6005_6006 |   8192
    dbms_om            | gs_wlm_operator_info    | dn_6005_6006 |   8192
    dbms_om            | gs_wlm_ec_operator_info | dn_6005_6006 |   8192
    public             | pgxc_copy_error_log     | dn_6005_6006 |   8192
    information_schema | sql_features            | dn_6005_6006 |  98304
    information_schema | sql_implementation_info | dn_6005_6006 |  49152
    information_schema | sql_languages           | dn_6005_6006 |  49152
    information_schema | sql_packages            | dn_6005_6006 |  49152
    information_schema | sql_parts               | dn_6005_6006 |  49152
    information_schema | sql_sizing              | dn_6005_6006 |  49152
    information_schema | sql_sizing_profiles     | dn_6005_6006 |   8192
    scheduler          | pg_task                 | dn_6003_6004 |   8192
    public             | ocr_group               | dn_6003_6004 |   8192
    public             | ocr_item                | dn_6003_6004 |  16384
    sea                | ocr_group               | dn_6003_6004 |   8192
    sea                | ocr_item                | dn_6003_6004 |  16384
    public             | customer_t1             | dn_6003_6004 |  16384
    dbms_om            | gs_wlm_session_info     | dn_6003_6004 |   8192
    dbms_om            | gs_wlm_operator_info    | dn_6003_6004 |   8192
    dbms_om            | gs_wlm_ec_operator_info | dn_6003_6004 |   8192
    public             | pgxc_copy_error_log     | dn_6003_6004 |   8192
    information_schema | sql_features            | dn_6003_6004 |  98304
    information_schema | sql_implementation_info | dn_6003_6004 |  49152
    information_schema | sql_languages           | dn_6003_6004 |  49152
    information_schema | sql_packages            | dn_6003_6004 |  49152
    information_schema | sql_parts               | dn_6003_6004 |  49152
    information_schema | sql_sizing              | dn_6003_6004 |  49152
    information_schema | sql_sizing_profiles     | dn_6003_6004 |   8192
    scheduler          | pg_task                 | dn_6001_6002 |   8192
    public             | ocr_group               | dn_6001_6002 |  16384
    public             | ocr_item                | dn_6001_6002 |   8192
    sea                | ocr_group               | dn_6001_6002 |   8192
    sea                | ocr_item                | dn_6001_6002 |  16384
    public             | customer_t1             | dn_6001_6002 |  16384
    dbms_om            | gs_wlm_session_info     | dn_6001_6002 |   8192
    dbms_om            | gs_wlm_operator_info    | dn_6001_6002 |   8192
    dbms_om            | gs_wlm_ec_operator_info | dn_6001_6002 |   8192
    public             | pgxc_copy_error_log     | dn_6001_6002 |   8192
    information_schema | sql_features            | dn_6001_6002 |  98304
    information_schema | sql_implementation_info | dn_6001_6002 |  49152
    information_schema | sql_languages           | dn_6001_6002 |  49152
    information_schema | sql_packages            | dn_6001_6002 |  49152
    information_schema | sql_parts               | dn_6001_6002 |  49152
    information_schema | sql_sizing              | dn_6001_6002 |  49152
    information_schema | sql_sizing_profiles     | dn_6001_6002 |   8192
   (51 rows)

gs_table_distribution(schemaname text, tablename text)
------------------------------------------------------

Description: Queries the storage space occupied by a specified table on each node.

Return type: record

.. table:: **Table 1** gs_table_distribution(schemaname text, tablename text)

   +-----------------------+-----------------------+---------------------------------------------------+
   | Column                | Type                  | Description                                       |
   +=======================+=======================+===================================================+
   | schemaname            | name                  | Specifies the schema name.                        |
   +-----------------------+-----------------------+---------------------------------------------------+
   | tablename             | name                  | Table name                                        |
   +-----------------------+-----------------------+---------------------------------------------------+
   | relkind               | character             | Type.                                             |
   |                       |                       |                                                   |
   |                       |                       | -  **i**: index                                   |
   |                       |                       | -  **r**: table                                   |
   +-----------------------+-----------------------+---------------------------------------------------+
   | nodename              | name                  | Node name                                         |
   +-----------------------+-----------------------+---------------------------------------------------+
   | dnsize                | bigint                | Storage space of the table on the node, in bytes. |
   +-----------------------+-----------------------+---------------------------------------------------+

.. note::

   -  To query for the storage distribution of a specified table by using this function, you must have the **SELECT** permission for the table.
   -  This function is based on the physical file storage space records in the **PG_RELFILENODE_SIZE** system catalog. Ensure that the GUC parameters **use_workload_manager** and **enable_perm_space** are enabled.
   -  The performance of the **gs_table_distribution** function is lower than that of the **table_distribution** function when a single table is queried. But when the entire database is queried, the performance of the **gs_table_distribution** function is much better. In a large cluster with a large amount of data, you are advised to use the **gs_table_distribution** function to query all tables in the database.

gs_table_distribution()
-----------------------

Description: Quickly queries the storage distribution of all tables in the current database.

Return type: record

.. table:: **Table 2** gs_table_distribution()

   ========== ========= =================================================
   Column     Type      Description
   ========== ========= =================================================
   schemaname name      Schema name
   tablename  name      Table name
   relkind    character Type of the table. **i**: index; **r**: table.
   nodename   name      Node name
   dnsize     bigint    Storage space of the table on the node, in bytes.
   ========== ========= =================================================

.. note::

   -  To query for the storage distribution of a specified table by using this function, you must have the **SELECT** permission for the table.
   -  This function is based on the physical file storage space records in the **PG_RELFILENODE_SIZE** system catalog. Ensure that the GUC parameters **use_workload_manager** and **enable_perm_space** are enabled.
   -  The performance of the **gs_table_distribution** function is lower than that of the **table_distribution** function when a single table is queried. But when the entire database is queried, the performance of the **gs_table_distribution** function is much better. In a large cluster with a large amount of data, you are advised to use the **gs_table_distribution** function to query all tables in the database.

pgxc_get_stat_dirty_tables(int dirty_percent, int n_tuples)
-----------------------------------------------------------

Description: Obtains information about insertion, update, and deletion operations on tables and the dirty page rate of tables. This function optimizes the performance of the **PGXC_GET_STAT_ALL_TABLES** view. It can quickly filter out tables whose dirty page rate is greater than **dirty_percent** and number of dead tuples is greater than **n_tuples** on each DN, and return the filtering results to the CN for summary and output.

Return type: SETOF record

The following table describes return columns.

=============== ============ ==============================
Column          Type         Description
=============== ============ ==============================
relid           oid          Table OID
relname         name         Table name
schemaname      name         Schema name of the table
n_tup_ins       bigint       Number of inserted tuples
n_tup_upd       bigint       Number of updated tuples
n_tup_del       bigint       Number of deleted tuples
n_live_tup      bigint       Number of live tuples
n_dead_tup      bigint       Number of dead tuples
dirty_page_rate numeric(5,2) Dirty page rate (%) of a table
=============== ============ ==============================

Example:

Query the tables whose dirty page rate is greater than 10% and number of dirty data rows is greater than 1000 in the database:

::

   SELECT * FROM pgxc_get_stat_dirty_tables(0,0) where dirty_page_rate > 10 and n_dead_tup > 1000;
    relid |         relname         | schemaname | n_tup_ins | n_tup_upd | n_tup_del | n_live_tup | n_dead_tup | dirty_page_rate
   -------+-------------------------+------------+-----------+-----------+-----------+------------+------------+-----------------
    16782 | bandwidth_history_table | scheduler  |    356355 |         0 |    202068 |     154287 |      29031 |           15.84
    12050 | gs_wlm_instance_history | pg_catalog |    406464 |         0 |    234835 |     171647 |      27721 |           13.90
   (2 rows)

pgxc_get_stat_dirty_tables(int dirty_percent, int n_tuples, text schema)
------------------------------------------------------------------------

Description: Obtains information about insertion, update, and deletion operations on tables and the dirty page rate of tables. This function optimizes the performance of the **PGXC_GET_STAT_ALL_TABLES** view. It can quickly filter out tables whose dirty page rate is greater than **dirty_percent** and number of dead tuples is greater than **n_tuples** on each DN, and return the filtering results to the CN for summary and output. **text schema** indicates tables whose schema name is **schema**.

Return type: SETOF record

The return columns of the function are the same as those of the **pgxc_get_stat_dirty_tables(int dirty_percent, int n_tuples)** function.

Example:

Query the dirty page rate of the **pg_catalog.pg_class** system catalog.

::

   SELECT relname AS table_name,dirty_page_rate FROM pgxc_get_stat_dirty_tables(0,0,'pg_catalog') WHERE relname = 'pg_class';
    table_name | dirty_page_rate
   ------------+-----------------
    pg_class   |           16.46
   (1 row)

gs_switch_relfilenode()
-----------------------

Description: Exchanges meta information of two tables or partitions. (This is only used for the redistribution tool. An error message is displayed when the function is directly used by users).

Return type: integer

copy_error_log_create()
-----------------------

Description: Creates the error table (**public.pgxc_copy_error_log**) required for creating the **COPY FROM** error tolerance mechanism.

Return type: boolean

.. note::

   -  This function attempts to create the **public.pgxc_copy_error_log** table. For details about the table, see :ref:`Table 3 <en-us_topic_0000001495991693__table2822639715>`.
   -  Create the B-tree index on the **relname** column and execute **REVOKE ALL on public.pgxc_copy_error_log FROM public** to manage permissions for the error table (the permissions are the same as those of the **COPY** statement).
   -  **public.pgxc_copy_error_log** is a row-store table. Therefore, this function can be executed and **COPY FROM** error tolerance is available only when row-store tables can be created in the cluster. After the GUC parameter **enable_hadoop_env** is enabled, row-based tables cannot be created in the cluster. The default value is **off**.
   -  Same as the error table and the **COPY** statement, the function requires **sysadmin** or higher permissions.
   -  If the **public.pgxc_copy_error_log** table or the **copy_error_log_relname_idx** index already exists before the function creates it, the function will report an error and roll back.

.. _en-us_topic_0000001495991693__table2822639715:

.. table:: **Table 3** Error table public.pgxc_copy_error_log

   +-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column    | Type                     | Description                                                                                                                                          |
   +===========+==========================+======================================================================================================================================================+
   | relname   | varchar                  | Name of the table, which is in the form of *Schema name*\ **.**\ *Table name*                                                                        |
   +-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | begintime | timestamp with time zone | Time when a data format error was reported                                                                                                           |
   +-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | filename  | character varying        | Name of the source data file where a data format error occurs                                                                                        |
   +-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rownum    | bigint                   | Number of the row where a data format error occurs in a source data file                                                                             |
   +-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rawrecord | text                     | Raw record of a data format error in the source data file. To prevent a field from being too long, the length of the field cannot exceed 1024 bytes. |
   +-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | detail    | text                     | Error details                                                                                                                                        |
   +-----------+--------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Example:

::

   SELECT copy_error_log_create();
    copy_error_log_create
   -----------------------
    t
   (1 row)
