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

.. table:: **Table 1** ALTER TRIGGER parameters

   +--------------+------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | Parameter    | Description                                                                  | Value Range                                                                                                      |
   +==============+==============================================================================+==================================================================================================================+
   | trigger_name | Specifies the name of the trigger to be modified.                            | Valid trigger name.                                                                                              |
   +--------------+------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | table_name   | Specifies the name of the table where the trigger to be modified is located. | Name of the valid table where the trigger is located.                                                            |
   +--------------+------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+
   | new_name     | Specifies the new trigger name.                                              | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`. |
   +--------------+------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------+

Examples
--------

Create sample tables **test_trigger_src_tbl** and **test_trigger_des_tbl**.

::

   DROP TABLE IF EXISTS test_trigger_src_tbl;
   DROP TABLE IF EXISTS test_trigger_des_tbl;

   CREATE TABLE test_trigger_src_tbl(id1 INT, id2 INT, id3 INT);
   CREATE TABLE test_trigger_des_tbl(id1 INT, id2 INT, id3 INT);

Create the **tri_insert_func()** trigger function.

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

Create the **insert_trigger** trigger.

::

   CREATE TRIGGER insert_trigger
              BEFORE INSERT ON test_trigger_src_tbl
              FOR EACH ROW
              EXECUTE PROCEDURE tri_insert_func();

Modify the **insert_trigger** trigger.

::

   ALTER TRIGGER insert_trigger ON test_trigger_src_tbl RENAME TO delete_trigger_renamed;

Disable the **delete_trigger_renamed** trigger.

::

   ALTER TABLE test_trigger_src_tbl DISABLE TRIGGER delete_trigger_renamed;

Disable all triggers on the **test_trigger_src_tbl** table.

::

   ALTER TABLE test_trigger_src_tbl DISABLE TRIGGER ALL;

Helpful Links
-------------

:ref:`CREATE TRIGGER <dws_06_0184>`, :ref:`DROP TRIGGER <dws_06_0212>`, and :ref:`ALTER TABLE <dws_06_0142>`
