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

#. Create a table.

   ::

      DROP TABLE IF EXISTS bool_type_t1;
      CREATE TABLE bool_type_t1
      (
          BT_COL1 BOOLEAN,
          BT_COL2 TEXT
      ) DISTRIBUTE BY HASH(BT_COL2);

#. Insert data.

   ::

      INSERT INTO bool_type_t1 VALUES (TRUE, 'sic est');
      INSERT INTO bool_type_t1 VALUES (FALSE, 'non est');

#. Query data.

   ::

      SELECT * FROM bool_type_t1;

   |image1|

   ::

      SELECT * FROM bool_type_t1 WHERE bt_col1 = 't';

   |image2|

#. Drop the table.

   ::

      DROP TABLE bool_type_t1;

.. |image1| image:: /_static/images/en-us_image_0000002568800157.png
.. |image2| image:: /_static/images/en-us_image_0000002537960450.png
