:original_name: dws_06_0011.html

.. _dws_06_0011:

Boolean Type
============

.. table:: **Table 1** Boolean type

   +-----------------+-----------------+-----------------+-----------------------+
   | Name            | Description     | Storage Space   | Value                 |
   +=================+=================+=================+=======================+
   | BOOLEAN         | Boolean type    | 1 byte          | -  **true**           |
   |                 |                 |                 | -  **false**          |
   |                 |                 |                 | -  **null** (unknown) |
   +-----------------+-----------------+-----------------+-----------------------+

Valid literal values for the "true" state are:

**TRUE**, **'t'**, **'true'**, **'y'**, **'yes'**, **'1'**

Valid literal values for the "false" state include:

**FALSE**, **'f'**, **'false'**, **'n'**, **'no'**, **'0'**

**TRUE** and **FALSE** are standard expressions, compatible with SQL statements.

Examples
--------

Data type **boolean** is displayed with letters **t** and **f**.

::

   -- Create a table:
   CREATE TABLE bool_type_t1
   (
       BT_COL1 BOOLEAN,
       BT_COL2 TEXT
   ) DISTRIBUTE BY HASH(BT_COL2);

   --Insert data:
   INSERT INTO bool_type_t1 VALUES (TRUE, 'sic est');

   INSERT INTO bool_type_t1 VALUES (FALSE, 'non est');

   -- View data:
   SELECT * FROM bool_type_t1;
    bt_col1 | bt_col2
   ---------+---------
    t       | sic est
    f       | non est
   (2 rows)

   SELECT * FROM bool_type_t1 WHERE bt_col1 = 't';
    bt_col1 | bt_col2
   ---------+---------
    t       | sic est
   (1 row)

   -- Delete the tables:
   DROP TABLE bool_type_t1;
