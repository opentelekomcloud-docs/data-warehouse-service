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

**index_oid** indicates the index OID, which can be queried in the **PG_STATIO_ALL_INDEXES** system view.

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

pg_get_tabledef(table_name)
---------------------------

Description: Obtains a table definition based on **table_name**.

Return type: text

Remarks: **pg_get_tabledef** reconstructs the **CREATE** statement of the table definition, including the table definition, index information, and comments. Users need to create the dependent objects of the table, such as groups, schemas, tablespaces, and servers. The table definition does not include the statements for creating these dependent objects.

pg_get_tabledef(table_name/table_oid, forCreate boolean)
--------------------------------------------------------

Description: Obtains a table definition based on **table_name**. It is supported only by clusters of version 9.1.0.100 or later.

Return type: text

-  If the **forCreate** parameter is **true**, the table definition obtained will exclude the **TO GROUP** clause.
-  If the **forCreate** parameter is **false**, the table definition obtained will include the **TO GROUP** clause.

pg_get_tabledef(table_name/table_oid, forCreate boolean,withColComm boolean)
----------------------------------------------------------------------------

Description: Obtains a table definition based on **table_name**. This function is supported only by clusters of version 9.1.0.200 or later.

Return type: text

-  If **withColComment** is **true**, the comment for a column field is displayed next to it,
-  If **withColComment** is **false**, the comment for a column is shown at the end of the table definition.

Examples:

.. code-block::

   SELECT pg_get_tabledef('person',false,true);
                     pg_get_tabledef
   ----------------------------------------------------
    SET search_path = public;                         +
    CREATE  TABLE person (                            +
            id integer COMMENT 'Student ID',                +
            name character varying(25) COMMENT 'Name',+
            address text COMMENT 'Address'               +
    )                                                 +
    WITH (orientation=row, compression=no)            +
    DISTRIBUTE BY ROUNDROBIN                          +
    TO GROUP group_version1;
   (1 row)

   SELECT pg_get_tabledef('person',false,false);
                  pg_get_tabledef
   ---------------------------------------------
    SET search_path = public;                  +
    CREATE  TABLE person (                     +
            id integer,                        +
            name character varying(25),        +
            address text                       +
    )                                          +
    WITH (orientation=row, compression=no)     +
    DISTRIBUTE BY ROUNDROBIN                   +
    TO GROUP group_version1;                   +
    COMMENT ON COLUMN person.id IS 'Student ID';     +
    COMMENT ON COLUMN person.name IS 'Name';   +
    COMMENT ON COLUMN person.address IS 'Address';
   (1 row)

pg_options_to_table(reloptions)
-------------------------------

Description: Gets the set of storage option name/value pairs.

Return type: SETOF record

Note: **pg_options_to_table** returns the set of storage option name/value pairs (**option_name**/**option_value**) when passing **pg_class.reloptions** or **pg_attribute.attoptions**.

The following is an example:

::

   CREATE TABLE customer_test
   (
     state_ID   CHAR(2),
     state_NAME VARCHAR2(40),
     area_ID    NUMBER
   )
   WITH (ORIENTATION = COLUMN,COMPRESSION=middle);

::

   SELECT pg_options_to_table(reloptions) FROM pg_class WHERE relname='customer_test';
   pg_options_to_table
   ----------------------
    (orientation,column)
    (compression,middle)
    (bucketnums,16384)
    (colversion,2.0)
    (enable_delta,false)
   (5 rows)

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

The value might be quoted and schema-qualified. If no collation is derived for the argument expression, then a null value is returned. If the parameter is not of a collatable data type, then an error is thrown.

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
