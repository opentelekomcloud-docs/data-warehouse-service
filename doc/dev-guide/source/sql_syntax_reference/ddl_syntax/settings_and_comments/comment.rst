:original_name: dws_06_0154.html

.. _dws_06_0154:

COMMENT
=======

Function
--------

**COMMENT** defines or changes the comment of an object.

Precautions
-----------

-  Only one comment string is stored for each object. To modify a comment, issue a new **COMMENT** command for the same object. To remove a comment, write **NULL** at the location of the text string. Comments are automatically deleted when their objects are deleted.
-  There is no security for viewing comments. Any user with database access can see all object comments. Comments on shared items like databases, roles, and tablespaces are visible to any user connected to any database in the cluster. Avoid adding sensitive security information to comments.
-  For most objects, only the owner of the object can set comments. A role has no owner. So, only a system admin can use the **COMMENT ON ROLE** command for system admin roles. Roles with the **CREATE ROLE** permission can add comments to non-admin roles. Administrators can comment on anything.

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

.. table:: **Table 1** COMMENT parameters

   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | Parameter                                    | Description                                                                                                                                                   | Value Range                           |
   +==============================================+===============================================================================================================================================================+=======================================+
   | agg_name                                     | Specifies the name of the aggregate function to be commented out.                                                                                             | ``-``                                 |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | agg_type                                     | Specifies the data types of the aggregation function parameters.                                                                                              | ``-``                                 |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | source_type                                  | Specifies the name of the source data type of the cast.                                                                                                       | ``-``                                 |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | target_type                                  | Specifies the name of the target data type of the cast.                                                                                                       | ``-``                                 |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | object_name                                  | Specifies the name of an object.                                                                                                                              | ``-``                                 |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | table_name.column_name/view_name.column_name | Specifies the column whose comment is defined or modified. You can add the table name or view name as the prefix.                                             | ``-``                                 |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | constraint_name                              | Specifies the table constraints whose comment is defined or modified.                                                                                         | Name of an existing table constraint. |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | table_name                                   | Specifies the table name.                                                                                                                                     | Name of an existing table.            |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | function_name                                | Specifies the function whose comment is defined or modified.                                                                                                  | Use an existing function name.        |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | argmode/argname/argtype                      | Specifies the schema, name, and type of the function parameters.                                                                                              | ``-``                                 |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | large_object_oid                             | Specifies the OID of the large object whose comment is defined or modified.                                                                                   | ``-``                                 |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | operator_name                                | Specifies the name of the operator.                                                                                                                           | ``-``                                 |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | left_type/right_type                         | Specifies the data type of the operation parameter, which can be specified by a pattern. Write NONE for the missing argument of a prefix or postfix operator. | ``-``                                 |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+
   | text                                         | Specifies the new comment.                                                                                                                                    | A string.                             |
   +----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------+

Examples
--------

Create a sample table **customer** and add comments to the **customer.c_custkey** column of the table.

::

   DROP TABLE IF EXISTS customer;
   CREATE TABLE customer
   (
       c_custkey     BIGINT NOT NULL CONSTRAINT c_custkey PRIMARY KEY  ,
       c_name        VARCHAR(25)  ,
       c_address     VARCHAR(40)  ,
       c_nationkey   INT          ,
       c_phone       CHAR(15)     ,
       c_acctbal     DECIMAL(15,2)
   )
   DISTRIBUTE BY HASH(c_custkey);

   COMMENT ON COLUMN customer.c_custkey IS 'Primary key of customer demographics table.';

Create the **customer_details_view** view and add comments to the view.

::

   CREATE OR REPLACE VIEW customer_details_view AS SELECT * FROM customer;
   COMMENT ON VIEW customer_details_view IS 'View of customer detail';

Add comments to the **customer** table.

::

   COMMENT ON TABLE customer IS 'This is my table';
