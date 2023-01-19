:original_name: dws_06_0051.html

.. _dws_06_0051:

System Information Functions
============================

Session Information Functions
-----------------------------

-  current_catalog

   Description: Name of the current database (called "catalog" in the SQL standard)

   Return type: name

   For example:

   ::

      SELECT current_catalog;
       current_database
      ------------------
       gaussdb
      (1 row)

-  current_database()

   Description: Name of the current database

   Return type: name

   For example:

   ::

      SELECT current_database();
       current_database
      ------------------
       gaussdb
      (1 row)

-  current_query()

   Description: Text of the currently executing query, as submitted by the client (might contain more than one statement)

   Return type: text

   For example:

   ::

      SELECT current_query();
            current_query
      -------------------------
       SELECT current_query();
      (1 row)

-  current_schema[()]

   Description: Name of current schema

   Return type: name

   For example:

   ::

      SELECT current_schema();
       current_schema
      ----------------
       public
      (1 row)

   Remarks: **current_schema** returns the first valid schema name in the search path. (If the search path is empty or contains no valid schema name, **NULL** is returned.) This is the schema that will be used for any tables or other named objects that are created without specifying a target schema.

-  current_schemas(boolean)

   Description: Names of schemas in search path

   Return type: name[]

   For example:

   ::

      SELECT current_schemas(true);
         current_schemas
      ---------------------
       {pg_catalog,public}
      (1 row)

   Note:

   **current_schemas(boolean)** returns an array of the names of all schemas presently in the search path. The Boolean option determines whether implicitly included system schemas such as **pg_catalog** are included in the returned search path.

   .. note::

      The search path can be altered at run time. The command is:

      ::

         SET search_path TO schema [, schema, ...]

-  current_user

   Description: User name of current execution context

   Return type: name

   For example:

   ::

      SELECT current_user;
       current_user
      --------------
       dbadmin
      (1 row)

   Note: **current_user** is the user identifier that is applicable for permission checking. Normally it is equal to the session user, but it can be changed with :ref:`SET ROLE <dws_06_0222>`. It also changes during the execution of functions with the attribute **SECURITY DEFINER**.

-  inet_client_addr()

   Description: Displays the IP address of the currently connected client.

   .. note::

      -  It is available only in remote connection mode.
      -  If the database is connected to the local PC, the value is empty.

   Return type: inet

   For example:

   ::

      SELECT inet_client_addr();
       inet_client_addr
      ------------------
       10.10.0.50
      (1 row)

-  inet_client_port()

   Description: Displays the port number of the currently connected client.

   .. note::

      It is available only in remote connection mode.

   Return type: int

   For example:

   ::

      SELECT inet_client_port();
       inet_client_port
      ------------------
                  33143
      (1 row)

-  inet_server_addr()

   Description: Displays the IP address of the current server.

   .. note::

      -  It is available only in remote connection mode.
      -  If the database is connected to the local PC, the value is empty.

   Return type: inet

   For example:

   ::

      SELECT inet_server_addr();
       inet_server_addr
      ------------------
       10.10.0.13
      (1 row)

-  inet_server_port()

   Description: Displays the port of the current server. All these functions return NULL if the current connection is via a Unix-domain socket.

   .. note::

      It is available only in remote connection mode.

   Return type: int

   For example:

   ::

      SELECT inet_server_port();
       inet_server_port
      ------------------
       8000
      (1 row)

-  pg_backend_pid()

   Description: Process ID of the server process attached to the current session

   Return type: int

   For example:

   ::

      SELECT pg_backend_pid();
       pg_backend_pid
      -----------------
       140229352617744
      (1 row)

-  pg_conf_load_time()

   Description: Configures load time. **pg_conf_load_time** returns the timestamp with time zone when the server configuration files were last loaded.

   Return type: timestamp with time zone

   For example:

   ::

      SELECT pg_conf_load_time();
            pg_conf_load_time
      ------------------------------
       2017-09-01 16:05:23.89868+08
      (1 row)

-  pg_my_temp_schema()

   Description: OID of the temporary schema of a session. The value is 0 if the OID does not exist.

   Return type: OID

   For example:

   ::

      SELECT pg_my_temp_schema();
       pg_my_temp_schema
      -------------------
                       0
      (1 row)

   Note: **pg_my_temp_schema** returns the OID of the current session's temporary schema, or zero if it has none (because it has not created any temporary tables). **pg_is_other_temp_schema** returns true if the given OID is the OID of another session's temporary schema.

-  pg_is_other_temp_schema(oid)

   Description: Whether the schema is the temporary schema of another session.

   Return type: boolean

   For example:

   ::

      SELECT pg_is_other_temp_schema(25356);
       pg_is_other_temp_schema
      -------------------------
       f
      (1 row)

-  pg_listening_channels()

   Description: Channel names that the session is currently listening on

   Return type: setof text

   For example:

   ::

      SELECT pg_listening_channels();
       pg_listening_channels
      -----------------------
      (0 rows)

   Note: **pg_listening_channels** returns a set of names of channels that the current session is listening to.

-  pg_postmaster_start_time()

   Description: Server start time **pg_postmaster_start_time** returns the **timestamp with time zone** when the server started.

   Return type: timestamp with time zone

   For example:

   ::

      SELECT pg_postmaster_start_time();
         pg_postmaster_start_time
      ------------------------------
       2017-08-30 16:02:54.99854+08
      (1 row)

-  pg_trigger_depth()

   Description: Current nesting level of triggers

   Return type: int

   For example:

   ::

      SELECT pg_trigger_depth();
       pg_trigger_depth
      ------------------
                      0
      (1 row)

-  pgxc_version()

   Description: Postgres-XC version information

   Return type: text

   For example:

   ::

      SELECT pgxc_version();
                                                      pgxc_version
      -------------------------------------------------------------------------------------------------------------
       Postgres-XC 1.1 on x86_64-unknown-linux-gnu, based on PostgreSQL 9.2.4, compiled by g++ (GCC) 5.4.0, 64-bit
      (1 row)

-  session_user

   Description: Session user name

   Return type: name

   For example:

   ::

      SELECT session_user;
       session_user
      --------------
       dbadmin
      (1 row)

   Note: **session_user** is usually the user who initiated the current database connection, but administrators can change this setting with :ref:`SET SESSION AUTHORIZATION <dws_06_0223>`.

-  user

   Description: Is equivalent to **current_user**.

   Return type: name

   For example:

   ::

      SELECT user;
       current_user
      --------------
       dbadmin
      (1 row)

-  version()

   Description: version information. **version** returns a string describing a server's version.

   Return type: text

   For example:

   ::

      SELECT version();
                                                                      version
      ---------------------------------------------------------------------------------------------------------------------------------------
       PostgreSQL 9.2.4 gsql ((GaussDB 8.1.1 build af002019) compiled at 2020-01-10 05:43:20 commit 6995 last mr 11566 ) on x86_64-unknown-linux-gnu, compiled by g++ (GCC) 5.4.0, 64-bit
      (1 row)

Access Privilege Inquiry Functions
----------------------------------

-  has_any_column_privilege(user, table, privilege)

   Description: Queries whether a specified user has permission for any column of table.

   Return type: boolean

-  has_any_column_privilege(table, privilege)

   Description: Queries whether the current user has permission for any column of table.

   Return type: boolean

   **has_any_column_privilege** checks whether a user can access any column of a table in a particular way. Its parameter possibilities are analogous to **has_table_privilege**, except that the desired access permission type must be some combination of SELECT, INSERT, UPDATE, or REFERENCES.

   .. note::

      Note that having any of these permissions at the table level implicitly grants it for each column of the table, so **has_any_column_privilege** will always return **true** if **has_table_privilege** does for the same parameters. But **has_any_column_privilege** also succeeds if there is a column-level grant of the permission for at least one column.

-  has_column_privilege(user, table, column, privilege)

   Description: Queries whether a specified user has permission for column.

   Return type: boolean

-  has_column_privilege(table, column, privilege)

   Description: Queries whether the current user has permission for column.

   Return type: boolean

   **has_column_privilege** checks whether a user can access a column in a particular way. Its argument possibilities are analogous to **has_table_privilege**, with the addition that the column can be specified either by name or attribute number. The desired access permission type must evaluate to some combination of **SELECT**, **INSERT**, **UPDATE**, or **REFERENCES**.

   .. note::

      Note that having any of these permissions at the table level implicitly grants it for each column of the table.

-  has_database_privilege(user, database, privilege)

   Description: Queries whether a specified user has permission for database.

   Return type: boolean

-  has_database_privilege(database, privilege)

   Description: Queries whether the current user has permission for database.

   Return type: boolean

   Note: **has_database_privilege** checks whether a user can access a database in a particular way. Its argument possibilities are analogous to **has_table_privilege**. The desired access permission type must evaluate to some combination of **CREATE**, **CONNECT**, **TEMPORARY**, or **TEMP** (which is equivalent to **TEMPORARY**).

-  has_foreign_data_wrapper_privilege(user, fdw, privilege)

   Description: Queries whether a specified user has permission for foreign-data wrapper.

   The **fdw** parameter indicates the name or ID of the foreign data wrapper.

   Return type: boolean

-  has_foreign_data_wrapper_privilege(fdw, privilege)

   Description: Queries whether the current user has permission for foreign-data wrapper.

   Return type: boolean

   Note: **has_foreign_data_wrapper_privilege** checks whether a user can access a foreign-data wrapper in a particular way. Its argument possibilities are analogous to **has_table_privilege**. The desired access permission type must evaluate to **USAGE**.

-  has_function_privilege(user, function, privilege)

   Description: Queries whether a specified user has permission for function.

   Return type: boolean

-  has_function_privilege(function, privilege)

   Description: Queries whether the current user has permission for function.

   Return type: boolean

   Note: **has_function_privilege** checks whether a user can access a function in a particular way. Its argument possibilities are analogous to **has_table_privilege**. When a function is specified by a text string rather than by OID, the allowed input is the same as that for the **regprocedure** data type (see :ref:`Object Identifier Types <dws_06_0022>`). The desired access permission type must evaluate to **EXECUTE**.

-  has_language_privilege(user, language, privilege)

   Description: Queries whether a specified user has permission for language.

   Return type: boolean

-  has_language_privilege(language, privilege)

   Description: Queries whether the current user has permission for language.

   Return type: boolean

   Note: **has_language_privilege** checks whether a user can access a procedural language in a particular way. Its argument possibilities are analogous to **has_table_privilege**. The desired access permission type must evaluate to **USAGE**.

-  has_schema_privilege(user, schema, privilege)

   Description: Queries whether a specified user has permission for schema.

   Return type: boolean

-  has_schema_privilege(schema, privilege)

   Description: Queries whether the current user has permission for schema.

   Return type: boolean

   Note: **has_schema_privilege** checks whether a user can access a schema in a particular way. Its argument possibilities are analogous to **has_table_privilege**. The desired access permission type must evaluate to some combination of **CREATE** or **USAGE**.

-  has_server_privilege(user, server, privilege)

   Description: Queries whether a specified user has permission for foreign server.

   Return type: boolean

-  has_server_privilege(server, privilege)

   Description: Queries whether the current user has permission for foreign server.

   Return type: boolean

   Note: **has_server_privilege** checks whether a user can access a foreign server in a particular way. Its argument possibilities are analogous to **has_table_privilege**. The desired access permission type must evaluate to **USAGE**.

-  has_table_privilege(user, table, privilege)

   Description: Queries whether a specified user has permission for table.

   Return type: boolean

-  has_table_privilege(table, privilege)

   Description: Queries whether the current user has permission for table.

   Return type: boolean

   **has_table_privilege** checks whether a user can access a table in a particular way. The user can be specified by name, by OID (**pg_authid.oid**), **public** to indicate the PUBLIC pseudo-role, or if the argument is omitted **current_user** is assumed. The table can be specified by name or by OID. When specifying by name, the name can be schema-qualified if necessary. The desired access permission type is specified by a text string, which must be one of the values **SELECT**, **INSERT**, **UPDATE**, **DELETE**, **TRUNCATE**, **REFERENCES**, or **TRIGGER**. Optionally, **WITH GRANT OPTION** can be added to a permission type to test whether the permission is held with grant option. Also, multiple permission types can be listed separated by commas, in which case the result will be **true** if any of the listed permissions is held.

   For example:

   ::

      SELECT has_table_privilege('tpcds.web_site', 'select');
       has_table_privilege
      ---------------------
       t
      (1 row)

      SELECT has_table_privilege('dbadmin', 'tpcds.web_site', 'select,INSERT WITH GRANT OPTION ');
       has_table_privilege
      ---------------------
       t
      (1 row)

-  pg_has_role(user, role, privilege)

   Description: Queries whether a specified user has permission for role.

   Return type: boolean

-  pg_has_role(role, privilege)

   Description: Specifies whether the current user has permission for role.

   Return type: boolean

   Note: **pg_has_role** checks whether a user can access a role in a particular way. Its argument possibilities are analogous to **has_table_privilege**, except that **public** is not allowed as a user name. The desired access permission type must evaluate to some combination of **MEMBER** or **USAGE**. **MEMBER** denotes direct or indirect membership in the role (that is, the right to do **SET ROLE**), while **USAGE** denotes the permissions of the role are available without doing **SET ROLE**.

Schema Visibility Inquiry Functions
-----------------------------------

Each function performs the visibility check for one type of database object. For functions and operators, an object in the search path is visible if there is no object of the same name and argument data type(s) earlier in the path. For operator classes, both name and associated index access method are considered.

All these functions require OIDs to identify the objects to be checked. If you want to test an object by name, it is convenient to use the OID alias types (**regclass**, **regtype**, **regprocedure**, **regoperator**, **regconfig**, or **regdictionary**).

For example, a table is said to be visible if its containing schema is in the search path and no table of the same name appears earlier in the search path. This is equivalent to the statement that the table can be referenced by name without explicit schema qualification. For example, to list the names of all visible tables:

::

   SELECT relname FROM pg_class WHERE pg_table_is_visible(oid);

-  pg_collation_is_visible(collation_oid)

   Description: Queries whether the collation is visible in search path.

   Return type: boolean

-  pg_conversion_is_visible(conversion_oid)

   Description: Queries whether the conversion is visible in search path.

   Return type: boolean

-  pg_function_is_visible(function_oid)

   Description: Queries whether the function is visible in search path.

   Return type: boolean

-  pg_opclass_is_visible(opclass_oid)

   Description: Queries whether the operator class is visible in search path.

   Return type: boolean

-  pg_operator_is_visible(operator_oid)

   Description: Queries whether the operator is visible in search path.

   Return type: boolean

-  pg_opfamily_is_visible(opclass_oid)

   Description: Queries whether the operator family is visible in search path.

   Return type: boolean

-  pg_table_is_visible(table_oid)

   Description: Queries whether the table is visible in search path.

   Return type: boolean

-  pg_ts_config_is_visible(config_oid)

   Description: Queries whether the text search configuration is visible in search path.

   Return type: boolean

-  pg_ts_dict_is_visible(dict_oid)

   Description: Queries whether the text search dictionary is visible in search path.

   Return type: boolean

-  pg_ts_parser_is_visible(parser_oid)

   Description: Queries whether the text search parser is visible in search path.

   Return type: boolean

-  pg_ts_template_is_visible(template_oid)

   Description: Queries whether the text search template is visible in search path.

   Return type: boolean

-  pg_type_is_visible(type_oid)

   Description: Queries whether the type (or domain) is visible in search path.

   Return type: boolean

System Catalog Information Functions
------------------------------------

-  format_type(type_oid, typemod)

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

-  pg_check_authid(role_oid)

   Description: Checks whether a role name with given OID exists.

   Return type: bool

-  pg_describe_object(catalog_id, object_id, object_sub_id)

   Description: Gets description of a database object.

   Return type: text

   Note: **pg_describe_object** returns a description of a database object specified by catalog OID, object OID and a (possibly zero) sub-object ID. This is useful to determine the identity of an object as stored in the **pg_depend** catalog.

-  pg_get_constraintdef(constraint_oid)

   Description: Gets definition of a constraint.

   Return type: text

-  pg_get_constraintdef(constraint_oid, pretty_bool)

   Description: Gets definition of a constraint.

   Return type: text

   Note: **pg_get_constraintdef** and **pg_get_indexdef** respectively reconstruct the creating command for a constraint and an index.

-  pg_get_expr(pg_node_tree, relation_oid)

   Description: Decompiles internal form of an expression, assuming that any Vars in it refer to the relationship indicated by the second parameter.

   Return type: text

-  pg_get_expr(pg_node_tree, relation_oid, pretty_bool)

   Description: Decompiles internal form of an expression, assuming that any Vars in it refer to the relationship indicated by the second parameter.

   Return type: text

   Note: **pg_get_expr** decompiles the internal form of an individual expression, such as the default value for a column. It can be useful when examining the contents of system catalogs. If the expression might contain Vars, specify the OID of the relationship they refer to as the second parameter; if no Vars are expected, zero is sufficient.

-  pg_get_functiondef(func_oid)

   Description: Gets definition of a function.

   Return type: text

   **func_oid** is the OID of the function, which can be queried in the PG_PROC system catalog.

   Example: Query the OID and definition of the justify_days function.

   ::

      SELECT oid FROM pg_proc WHERE proname ='justify_days';
       oid
      ------
       1295
      (1 row)

      SELECT * FROM pg_get_functiondef(1295);
       headerlines |                          definition
      -------------+--------------------------------------------------------------
                 4 | CREATE OR REPLACE FUNCTION pg_catalog.justify_days(interval)+
                   |  RETURNS interval                                           +
                   |  LANGUAGE internal                                          +
                   |  IMMUTABLE STRICT NOT FENCED NOT SHIPPABLE                  +
                   | AS $function$interval_justify_days$function$                +
                   |
      (1 row)

-  pg_get_function_arguments(func_oid)

   Description: Gets argument list of function's definition (with default values).

   Return type: text

   Note: **pg_get_function_arguments** returns the argument list of a function, in the form it would need to appear in within **CREATE FUNCTION**.

-  pg_get_function_identity_arguments(func_oid)

   Description: Gets argument list to identify a function (without default values).

   Return type: text

   Note: **pg_get_function_identity_arguments** returns the argument list necessary to identify a function, in the form it would need to appear in within **ALTER FUNCTION**. This form omits default values.

-  pg_get_function_result(func_oid)

   Description: Gets **RETURNS** clause for function.

   Return type: text

   Note: **pg_get_function_result** returns the appropriate **RETURNS** clause for the function.

-  pg_get_indexdef(index_oid)

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

-  pg_get_indexdef(index_oid, column_no, pretty_bool)

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

-  pg_get_keywords()

   Description: Gets list of SQL keywords and their categories.

   Return type: setof record

   Note: **pg_get_keywords** returns a set of records describing the SQL keywords recognized by the server. The **word** column contains the keyword. The **catcode** column contains a category code: **U** for unreserved, **C** for column name, **T** for type or function name, or **R** for reserved. The **catdesc** column contains a possibly-localized string describing the category.

-  pg_get_ruledef(rule_oid)

   Description: Gets **CREATE RULE** command for a rule.

   Return type: text

-  pg_get_ruledef(rule_oid, pretty_bool)

   Description: Gets **CREATE RULE** command for a rule.

   Return type: text

-  pg_get_userbyid(role_oid)

   Description: Gets role name with given OID.

   Return type: name

   Note: **pg_get_userbyid** extracts a role's name given its OID.

-  pg_get_viewdef(viewname text [, pretty bool [, fullflag bool]])

   Description: gets underlying **SELECT** command for views.

   Return type: text

   Note:

   -  **pg_get_viewdef** reconstructs the **SELECT** query that defines a view. If the value of **pretty bool** is set to **true**, the display format is suitable for printing and more readable. The default value of **pretty bool** is **false**, and the display format is not readable. Use the default format for dump purposes whenever possible. The **pretty bool** parameter can be applied only to valid views.
   -  When **fullflag bool** is set to **true**, the complete definition of the view is displayed. The default value is **false**.

-  pg_get_viewdef(viewoid oid [, pretty bool [, fullflag bool]])

   Description: gets underlying **SELECT** command for views.

   Return type: text

-  pg_get_viewdef(view_oid, wrap_column_int)

   Description: Gets underlying SELECT command for view, wrapping lines with columns as specified, printing is implied.

   Return type: text

-  pg_get_tabledef(table_oid)

   Description: Obtains a table definition based on **table_oid**.

   Return type: text

   Example: Obtain the OID of the table **customer_t2** from the system catalog **pg_class**, and then use this function to query the definition of **customer_t2** to obtain the table columns, storage mode (row-store or column-store), and table distribution mode configured for **customer_t2** when it is created.

   ::

      select oid from pg_class where relname ='customer_t2';
        oid
      -------
       17353
      (1 row)

      select * from pg_get_tabledef(17353);
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

-  pg_get_tabledef(table_name)

   Description: Obtains a table definition based on **table_name**.

   Return type: text

   Remarks: **pg_get_tabledef** reconstructs the **CREATE** statement of the table definition, including the table definition, index information, and comments. Users need to create the dependent objects of the table, such as groups, schemas, tablespaces, and servers. The table definition does not include the statements for creating these dependent objects.

-  pg_options_to_table(reloptions)

   Description: Gets the set of storage option name/value pairs.

   Return type: setof record

   Note: **pg_options_to_table** returns the set of storage option name/value pairs (**option_name**/**option_value**) when passing **pg_class.reloptions** or **pg_attribute.attoptions**.

-  pg_typeof(any)

   Description: Gets the data type of any value.

   Return type: regtype

   Note:

   **pg_typeof** returns the OID of the data type of the value that is passed to it. This can be helpful for troubleshooting or dynamically constructing SQL queries. The function is declared as returning **regtype**, which is an OID alias type (see :ref:`Object Identifier Types <dws_06_0022>`). This means that it is the same as an OID for comparison purposes but displays as a type name.

   For example:

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

-  collation for (any)

   Description: Gets the collation of the parameter.

   Return type: text

   Note:

   The expression **collation for** returns the collation of the value that is passed to it. For example:

   ::

      SELECT collation for (description) FROM pg_description LIMIT 1;
       pg_collation_for
      ------------------
       "default"
      (1 row)

   The value might be quoted and schema-qualified. If no collation is derived for the argument expression, then a null value is returned. If the parameter is not of a collectable data type, then an error is thrown.

-  getdistributekey(table_name)

   Description: Gets a distribution column for a hash table.

   Return type: text

   For example:

   ::

      SELECT getdistributekey('item');
       getdistributekey
      ------------------
       i_item_sk
      (1 row)

Comment Information Functions
-----------------------------

-  col_description(table_oid, column_number)

   Description: Gets comment for a table column.

   Return type: text

   Note: **col_description** returns the comment for a table column, which is specified by the OID of its table and its column number.

-  obj_description(object_oid, catalog_name)

   Description: Gets comment for a database object.

   Return type: text

   Note: The two-parameter form of **obj_description** returns the comment for a database object specified by its OID and the name of the containing system catalog. For example, **obj_description(123456,'pg_class')** would retrieve the comment for the table with OID 123456. The one-parameter form of **obj_description** requires only the object OID.

   **obj_description** cannot be used for table columns since columns do not have OIDs of their own.

-  obj_description(object_oid)

   Description: Gets comment for a database object.

   Return type: text

-  shobj_description(object_oid, catalog_name)

   Description: Gets comment for a shared database object.

   Return type: text

   Note: **shobj_description** is used just like **obj_description** except the former is used for retrieving comments on shared objects. Some system catalogs are global to all databases within each cluster, and the comments for objects in them are stored globally as well.

Transaction IDs and Snapshots
-----------------------------

The following functions provide server transaction information in an exportable form. The main use of these functions is to determine which transactions were committed between two snapshots.

-  pgxc_is_committed(transaction_id)

   Description: Determines whether the given XID is committed or ignored. NULL indicates the unknown status (such as running, preparing, and freezing).

   Return type: bool

-  txid_current()

   Description: Gets current transaction ID.

   Return type: bigint

-  txid_current_snapshot()

   Description: Gets current snapshot.

   Return type: txid_snapshot

-  txid_snapshot_xip(txid_snapshot)

   Description: Gets in-progress transaction IDs in snapshot.

   Return type: setof bigint

-  txid_snapshot_xmax(txid_snapshot)

   Description: Gets **xmax** of snapshot.

   Return type: bigint

-  txid_snapshot_xmin(txid_snapshot)

   Description: Gets **xmin** of snapshot.

   Return type: bigint

-  txid_visible_in_snapshot(bigint, txid_snapshot)

   Description: Queries whether the transaction ID is visible in snapshot. (do not use with subtransaction ids)

   Return type: boolean

The internal transaction ID type (**xid**) is 32 bits wide and wraps around every 4 billion transactions. **txid_snapshot**, the data type used by these functions, stores information about transaction ID visibility at a particular moment in time. :ref:`Table 1 <en-us_topic_0000001098990948__t6c9802fc233b4ac6889195c97de3f1e0>` describes its components.

.. _en-us_topic_0000001098990948__t6c9802fc233b4ac6889195c97de3f1e0:

.. table:: **Table 1** Snapshot components

   +----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name     | Description                                                                                                                                                                                                                                                                                                                                                                                           |
   +==========+=======================================================================================================================================================================================================================================================================================================================================================================================================+
   | xmin     | Earliest transaction ID (txid) that is still active. All earlier transactions will either be committed and visible, or rolled back.                                                                                                                                                                                                                                                                   |
   +----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | xmax     | First as-yet-unassigned txid. All txids greater than or equal to this are not yet started as of the time of the snapshot, so they are invisible.                                                                                                                                                                                                                                                      |
   +----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | xip_list | Active txids at the time of the snapshot. The list includes only those active txids between **xmin** and **xmax**; there might be active txids higher than **xmax**. A txid that is **xmin <= txid < xmax** and not in this list was already completed at the time of the snapshot, and is either visible or dead according to its commit status. The list does not include txids of subtransactions. |
   +----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**txid_snapshot**'s textual representation is **xmin:xmax:xip_list**.

For example: **10:20:10,14,15** means **xmin=10, xmax=20, xip_list=10, 14, 15**.

Computing Node Group Function
-----------------------------

pv_compute_pool_workload()

Description: Load status of a computing Node Group.

Return type: void

For example:

::

   SELECT * from pv_compute_pool_workload();
    nodename  | rpinuse | maxrp | nodestate
   -----------+---------+-------+-----------
    datanode1 |       0 |  1000 | normal
    datanode2 |       0 |  1000 | normal
   (2 rows)

Lock Information Function
-------------------------

pgxc_get_lock_conflicts()

Description: Obtains information about conflicting locks in the cluster. When a lock is waiting for another lock or another lock is waiting for it, a lock conflict occurs.

Return type: setof record
