:original_name: dws_06_0343.html

.. _dws_06_0343:

Comment Checking Functions
==========================

col_description(table_oid, column_number)
-----------------------------------------

Description: Gets comment for a table column.

Return type: text

Note: **col_description** returns the comment for a table column, which is specified by the OID of its table and its column number.

Example: Query the **pg_class** system catalog to obtain the table OID, and query the INFORMATION_SCHEMA.COLUMNS system view to obtain **column_number**.

::

   SELECT COLUMN_NAME,ORDINAL_POSITION FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 't' AND COLUMN_NAME = 'a';
   column_name | ordinal_position
   -------------+------------------
   a           |                1
   (1 row)

   SELECT oid FROM pg_class WHERE relname='t';
     oid
   -------
    44020
   (1 row)

   SELECT col_description(44020,1);
       col_description
   -----------------------
    This is a test table.
   (1 row)

obj_description(object_oid, catalog_name)
-----------------------------------------

Description: Gets comment for a database object.

Return type: text

Note: The two-parameter form of **obj_description** returns the comment for a database object specified by its OID and the name of the containing system catalog. For example, **obj_description(123456,'pg_class')** would retrieve the comment for the table with OID 123456. The one-parameter form of **obj_description** requires only the object OID.

**obj_description** cannot be used for table columns since columns do not have OIDs of their own.

obj_description(object_oid)
---------------------------

Description: Gets comment for a database object.

Return type: text

shobj_description(object_oid, catalog_name)
-------------------------------------------

Description: Gets comment for a shared database object.

Return type: text

Note: The functions of **shobj_description** and **obj_description** are similar. The only difference is that **shobj_description** is used for shared objects. Some system catalogs are global to all databases within each cluster, and the comments for objects in them are stored globally as well.
