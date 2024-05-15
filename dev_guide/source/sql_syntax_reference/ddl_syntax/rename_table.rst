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

Create a column-store table and specify the storage format and compression mode:

::

   DROP TABLE IF EXISTS customer_address;
   CREATE TABLE customer_address
   (
       ca_address_sk       INTEGER                  NOT NULL   ,
       ca_address_id       CHARACTER(16)            NOT NULL   ,
       ca_street_number    CHARACTER(10)                       ,
       ca_street_name      CHARACTER varying(60)               ,
       ca_street_type      CHARACTER(15)                       ,
       ca_suite_number     CHARACTER(10)
   )
   WITH (ORIENTATION = COLUMN, COMPRESSION=HIGH,COLVERSION=2.0)
   DISTRIBUTE BY HASH (ca_address_sk);

Rename a table:

::

   RENAME TABLE customer_address TO new_customer_address;

Helpful Links
-------------

:ref:`CREATE TABLE <dws_06_0177>`, :ref:`ALTER TABLE <dws_06_0142>`, and :ref:`DROP TABLE <dws_06_0208>`
