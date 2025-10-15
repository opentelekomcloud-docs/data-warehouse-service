:original_name: dws_06_0154.html

.. _dws_06_0154:

COMMENT
=======

Function
--------

**COMMENT** defines or changes the comment of an object.

Precautions
-----------

-  Only one comment string is stored for each object. To modify a comment, issue a new **COMMENT** command for the same object. To remove a comment, write **NULL** in place of the text string. Comments are automatically deleted when their objects are deleted.
-  Currently, there is no security protection for viewing comments. Any user connected to a database can view all the comments for objects in the database. For shared objects such as databases, roles, and tablespaces, comments are stored globally so any user connected to any database in the cluster can see all the comments for shared objects. Therefore, do not put security-critical information in comments.
-  For most kinds of objects, only the owner of objects can set the comment. Roles do not have owners, so the rule for **COMMENT ON ROLE** is that you must be administrator to comment on an administrator role, or have the CREATEROLE permission to comment on non-administrator roles. An administrator can comment on anything.

Syntax
------

::

   COMMENT ON
   {
     AGGREGATE agg_name (agg_type [, ...] ) |
     CAST (source_type AS target_type) |
     COLLATION object_name |
     COLUMN { table_name.column_name | view_name.column_name } |
     CONSTRAINT constraint_name ON table_name |
     CONVERSION object_name |
     DATABASE object_name |
     DOMAIN object_name |
     EXTENSION object_name |
     FOREIGN DATA WRAPPER object_name |
     FOREIGN TABLE object_name |
     FUNCTION function_name ( [ {[ argmode ] [ argname ] argtype} [, ...] ] ) |
     INDEX object_name |
     LARGE OBJECT large_object_oid |
     OPERATOR operator_name (left_type, right_type) |
     OPERATOR CLASS object_name USING index_method |
     OPERATOR FAMILY object_name USING index_method |
     [ PROCEDURAL ] LANGUAGE object_name |
     ROLE object_name |
     RULE rule_name ON table_name |
     SCHEMA object_name |
     SERVER object_name |
     TABLE object_name |
     TABLESPACE object_name |
     TEXT SEARCH CONFIGURATION object_name |
     TEXT SEARCH DICTIONARY object_name |
     TEXT SEARCH PARSER object_name |
     TEXT SEARCH TEMPLATE object_name |
     TYPE object_name |
     VIEW object_name
   }
      IS 'text';

Parameter Description
---------------------

-  **agg_name**

   Specifies the new name of an aggregation function.

-  **agg_type**

   Specifies the data types of the aggregation function parameters.

-  **source_type**

   Specifies the name of the source data type of the cast.

-  **target_type**

   Specifies the name of the target data type of the cast.

-  **object_name**

   Specifies the name of the object to be commented.

-  **table_name.column_name**/**view_name.column_name**

   Specifies the column whose comment is defined or modified. You can add the table name or view name as the prefix.

-  **constraint_name**

   Specifies the table constraints whose comment is defined or modified.

-  **table_name**

   Specifies the table name.

-  **function_name**

   Specifies the function whose comment is defined or modified.

-  **argmode,argname,argtype**

   Specifies the schema, name, and type of the function parameters.

-  **large_object_oid**

   Specifies the OID of the large object whose comment is defined or modified.

-  **operator_name**

   Specifies the name of the operator.

-  **left_type,right_type**

   The data type(s) of the operator's arguments (optionally schema-qualified). Write NONE for the missing argument of a prefix or postfix operator.

-  **text**

   Specifies the new comment, written as a string literal; or **NULL** to drop the comment.

Examples
--------

Add a comment to the **customer.c_customer_sk** column.

::

   COMMENT ON COLUMN customer.c_customer_sk IS 'Primary key of customer demographics table.';

Add a comment to the **tpcds.customer_details_view_v2** view.

::

   COMMENT ON VIEW tpcds.customer_details_view_v2 IS 'View of customer detail';

Add a comment to the **customer** table.

::

   COMMENT ON TABLE customer IS 'This is my table';
