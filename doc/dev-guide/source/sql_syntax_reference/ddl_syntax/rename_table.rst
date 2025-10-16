:original_name: dws_06_0276.html

.. _dws_06_0276:

RENAME TABLE
============

Function
--------

**RENAME TABLE** renames a specified table.

Precautions
-----------

RENAME TABLE has the same function as the following command:

::

   ALTER TABLE table_name RENAME to new_table_name

Syntax
------

::

   RENAME TABLE
   {[schema.]table_name TO new_table_name} [, ...];

Parameters
----------

-  **schema**

   Specifies the schema name.

-  **table_name**

   Specifies the name of the table to be modified.

-  **new_table_name**

   Specifies the new table name.

Examples
--------

Rename a table:

::

   RENAME TABLE customer_address TO new_customer_address;

Helpful Links
-------------

:ref:`CREATE TABLE <dws_06_0177>`, :ref:`ALTER TABLE <dws_06_0142>`, and :ref:`DROP TABLE <dws_06_0208>`
