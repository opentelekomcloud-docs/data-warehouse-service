:original_name: dws_06_0147.html

.. _dws_06_0147:

ALTER TRIGGER
=============

Function
--------

**ALTER TRIGGER** modifies the definition of a trigger.

Precautions
-----------

Only the owner of a table where a trigger is created and system administrators can run the **ALTER TRIGGER** statement.

Syntax
------

::

   ALTER TRIGGER trigger_name ON table_name RENAME TO new_name;

Parameter Description
---------------------

-  **trigger_name**

   Specifies the name of the trigger to be modified.

   Value range: an existing trigger

-  **table_name**

   Specifies the name of the table where the trigger to be modified is located.

   Value range: an existing table having a trigger

-  **new_name**

   Specifies the new trigger name.

   Value range: a string that complies with the identifier naming convention. A value contains a maximum of 63 characters and cannot be the same as other triggers on the same table.

Example
-------

Create a source table and a trigger table:

::

   DROP TABLE IF EXISTS test_trigger_src_tbl;
   DROP TABLE IF EXISTS test_trigger_des_tbl;

   CREATE TABLE test_trigger_src_tbl(id1 INT, id2 INT, id3 INT);
   CREATE TABLE test_trigger_des_tbl(id1 INT, id2 INT, id3 INT);

Create the trigger function **tri_insert_func()**:

::

   DROP FUNCTION IF EXISTS tri_insert_func;
   CREATE OR REPLACE FUNCTION tri_insert_func() RETURNS TRIGGER AS
              $$
              DECLARE
              BEGIN
                      INSERT INTO test_trigger_des_tbl VALUES(NEW.id1, NEW.id2, NEW.id3);
                      RETURN NEW;
              END
              $$ LANGUAGE PLPGSQL;

Create an **INSERT** trigger:

::

   CREATE TRIGGER insert_trigger
              BEFORE INSERT ON test_trigger_src_tbl
              FOR EACH ROW
              EXECUTE PROCEDURE tri_insert_func();

Modified the trigger **delete_trigger**.

::

   ALTER TRIGGER insert_trigger ON test_trigger_src_tbl RENAME TO insert_trigger_renamed;

Disable the trigger **insert_trigger**.

::

   ALTER TABLE test_trigger_src_tbl DISABLE TRIGGER insert_trigger_renamed;

Disable all triggers on the **test_trigger_src_tbl** table.

::

   ALTER TABLE test_trigger_src_tbl DISABLE TRIGGER ALL;

Helpful Links
-------------

:ref:`CREATE TRIGGER <dws_06_0184>`, :ref:`DROP TRIGGER <dws_06_0212>`, :ref:`ALTER TABLE <dws_06_0142>`
