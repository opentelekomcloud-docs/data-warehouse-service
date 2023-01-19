:original_name: dws_06_0184.html

.. _dws_06_0184:

CREATE TRIGGER
==============

Function
--------

**CREATE TRIGGER** creates a trigger. The trigger will be associated with a specified table or view, and will execute a specified function when certain events occur.

Precautions
-----------

-  Currently, triggers can be created only on ordinary row-store tables, instead of on column-store tables, temporary tables, or unlogged tables.
-  If multiple triggers of the same kind are defined for the same event, they will be fired in alphabetical order by name.
-  A trigger works only on one table. There is no limit on the number of triggers that can be created. However, more triggers on a table consume more performance.
-  Triggers are usually used for data association and synchronization between multiple tables. SQL execution performance is greatly affected. Therefore, you are advised not to use this statement when a large amount of data needs to be synchronized and performance requirements are high.
-  When a trigger meets the following conditions, the trigger statement and trigger itself can be pushed together down to a DN for execution, improving the trigger execution performance:

   -  **enable_trigger_shipping** and **enable_fast_query_shipping** are both enabled. (This is the default configuration.)
   -  The trigger function used by the source table is a PL/pgSQL function (recommended).
   -  The source and target tables have the same type and number of distribution keys, are both row-store tables, and belong to the same Node Group.
   -  The **INSERT**, **UPDATE**, or **DELETE** statement on the source table contains an expression about equality comparison between all the distribution keys and the *NEW* or *OLD* variable.
   -  The **INSERT**, **UPDATE**, or **DELETE** statement on the source table can be pushed down without a trigger.
   -  There are only six types of triggers, specified by **INSERT**/**UPDATE**/**DELETE**, **AFTER**/**BEFORE**, and **FOR EACH ROW**, on the source table, and all the triggers can be pushed down.

Syntax
------

::

   CREATE [ CONSTRAINT ] TRIGGER trigger_name { BEFORE | AFTER | INSTEAD OF } { event [ OR ... ] }
       ON table_name
       [ FROM referenced_table_name ]
       { NOT DEFERRABLE | [ DEFERRABLE ] { INITIALLY IMMEDIATE | INITIALLY DEFERRED } }
       [ FOR [ EACH ] { ROW | STATEMENT } ]
       [ WHEN ( condition ) ]
       EXECUTE PROCEDURE function_name ( arguments );

Events include:

::

       INSERT
       UPDATE [ OF column_name [, ... ] ]
       DELETE
       TRUNCATE

Parameter Description
---------------------

-  **CONSTRAINT**

   (Optional) Creates a constraint trigger, that is, a trigger is used as a constraint. Such a trigger is similar to a regular trigger except that the timing of the trigger firing can be adjusted using **SET CONSTRAINTS**. Constraint triggers must be **AFTER ROW** triggers.

-  **trigger_name**

   Specifies the name of a new trigger. The name cannot be schema-qualified because the trigger inherits the schema of its table. In addition, triggers on the same table cannot be named the same. For a constraint trigger, this is also the name to use when you modify the trigger's behavior using :ref:`SET CONSTRAINTS <dws_06_0221>`.

   Value range: a string that complies with the identifier naming convention. A value can contain a maximum of 63 characters.

-  **BEFORE**

   Specifies that a trigger function is called before the trigger event.

-  **AFTER**

   Specifies that a trigger function is called after the trigger event. A constraint trigger can only be specified as **AFTER**.

-  **INSTEAD OF**

   Specifies that a trigger function directly replaces the trigger event.

-  **event**

   Specifies the event that will fire a trigger. Values are **INSERT**, **UPDATE**, **DELETE**, and **TRUNCATE**. You can also specify multiple trigger events through **OR**.

   For **UPDATE** events, use the following syntax to specify a list of columns:

   ::

      UPDATE OF column_name1 [, column_name2 ... ]

   The trigger will only fire if at least one of the listed columns is mentioned as a target of the **UPDATE** statement. **INSTEAD OF UPDATE** events do not support lists of columns.

-  **table_name**

   Specifies the name of the table where a trigger needs to be created.

   Value range: name of an existing table in the database

-  **referenced_table_name**

   Specifies the name of another table referenced by a constraint. This parameter can be specified only for constraint triggers. It does not support foreign key constraints and is not recommended for general use.

   Value range: name of an existing table in the database

-  **DEFERRABLE \|** **NOT DEFERRABLE**

   Controls whether a constraint can be deferred. The two parameters determine the timing for firing a constraint trigger, and can be specified only for constraint triggers.

   For details, see :ref:`CREATE TABLE <dws_06_0177>`.

-  **INITIALLY IMMEDIATE** **\|** **INITIALLY DEFERRED**

   If a constraint is deferrable, the two clauses specify the default time to check the constraint, and can be specified only for constraint triggers.

   For details, see :ref:`CREATE TABLE <dws_06_0177>`.

-  **FOR EACH ROW \|** **FOR EACH STATEMENT**

   Specifies the frequency of firing a trigger.

   -  **FOR EACH ROW** indicates that the trigger should be fired once for every row affected by the trigger event.
   -  **FOR EACH STATEMENT** indicates that the trigger should be fired just once per SQL statement.

   If this parameter is not specified, the default value **FOR EACH STATEMENT** will be used. Constraint triggers can only be specified as **FOR EACH ROW**.

-  **condition**

   Specifies a Boolean expression that determines whether a trigger function will actually be executed. If **WHEN** is specified, the function will be called only when **condition** returns **true**.

   In **FOR EACH ROW** triggers, the **WHEN** condition can reference the columns of old or new row values by writing **OLD.**\ *column_name* or **NEW.**\ *column_name*, respectively. In addition, **INSERT** triggers cannot reference **OLD** and **DELETE** triggers cannot reference **NEW**.

   **INSTEAD OF** triggers do not support **WHEN** conditions.

   **WHEN** expressions cannot contain subqueries.

   For constraint triggers, evaluation of the **WHEN** condition is not deferred, but occurs immediately after the update operation is performed. If the condition does not return **true**, the trigger will not be queued for deferred execution.

-  **function_name**

   Specifies a user-defined function, which must be declared as taking no parameters and returning data of the trigger type. This function is executed when a trigger fires.

-  **arguments**

   Specifies an optional, comma-separated list of parameters to be provided to a function when a trigger is executed. Parameters are literal string constants. Simple names and numeric constants can also be included, but they will all be converted to strings. Check descriptions of the implementation language of a trigger function to find out how these parameters are accessed within the function.

   .. note::

      The following details trigger types:

      -  **INSTEAD OF** triggers must be marked as **FOR EACH ROW** and can be defined only on views.
      -  **BEFORE** and **AFTER** triggers on a view must be marked as **FOR EACH STATEMENT**.
      -  **TRUNCATE** triggers must be marked as **FOR EACH STATEMENT**.

   .. table:: **Table 1** Types of triggers supported on tables and views

      ============== ==================== ============= ================
      Trigger Timing Trigger Event        Row-level     Statement-level
      ============== ==================== ============= ================
      BEFORE         INSERT/UPDATE/DELETE Tables        Tables and views
      \              TRUNCATE             Not supported Tables
      AFTER          INSERT/UPDATE/DELETE Tables        Tables and views
      \              TRUNCATE             Not supported Tables
      INSTEAD OF     INSERT/UPDATE/DELETE Views         Not supported
      \              TRUNCATE             Not supported Not supported
      ============== ==================== ============= ================

   .. table:: **Table 2** Special variables in the functions PL/pgSQL triggers

      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | Variable        | Description                                                                                                          |
      +=================+======================================================================================================================+
      | NEW             | New tuple for **INSERT**/**UPDATE** operations. This variable is **NULL** for **DELETE** operations.                 |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | OLD             | Old tuple for **UPDATE**/**DELETE** operations. This variable is **NULL** for **INSERT** operations.                 |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | TG_NAME         | Trigger name                                                                                                         |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | TG_WHEN         | Trigger timing (**BEFORE**/**AFTER**/**INSTEAD OF**)                                                                 |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | TG_LEVEL        | Trigger frequency (**ROW**/**STATEMENT**)                                                                            |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | TG_OP           | Trigger event (**INSERT**/**UPDATE**/**DELETE**/**TRUNCATE**)                                                        |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | TG_RELID        | OID of the table where a trigger is located                                                                          |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | TG_RELNAME      | Name of the table where a trigger is located. (This variable is now discarded and is replaced by **TG_TABLE_NAME**.) |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | TG_TABLE_NAME   | Name of the table where a trigger is located.                                                                        |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | TG_TABLE_SCHEMA | Schema information of the table where a trigger is located                                                           |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | TG_NARGS        | Number of parameters for a trigger function                                                                          |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+
      | TG_ARGV[]       | List of parameters for a trigger function                                                                            |
      +-----------------+----------------------------------------------------------------------------------------------------------------------+

Examples
--------

Create a source table and a target table.

::

   CREATE TABLE test_trigger_src_tbl(id1 INT, id2 INT, id3 INT);

::

   CREATE TABLE test_trigger_des_tbl(id1 INT, id2 INT, id3 INT);

Create the trigger function **tri_insert_func()**.

::

   CREATE OR REPLACE FUNCTION tri_insert_func() RETURNS TRIGGER AS
              $$
              DECLARE
              BEGIN
                      INSERT INTO test_trigger_des_tbl VALUES(NEW.id1, NEW.id2, NEW.id3);
                      RETURN NEW;
              END
              $$ LANGUAGE PLPGSQL;

Create the trigger function **tri_update_func()**.

::

   CREATE OR REPLACE FUNCTION tri_update_func() RETURNS TRIGGER AS
              $$
              DECLARE
              BEGIN
                      UPDATE test_trigger_des_tbl SET id3 = NEW.id3 WHERE id1=OLD.id1;
                      RETURN OLD;
              END
              $$ LANGUAGE PLPGSQL;

Create the trigger function **tri_delete_func()**.

::

   CREATE OR REPLACE FUNCTION tri_delete_func() RETURNS TRIGGER AS
              $$
              DECLARE
              BEGIN
                      DELETE FROM test_trigger_des_tbl WHERE id1=OLD.id1;
                      RETURN OLD;
              END
              $$ LANGUAGE PLPGSQL;

Create an **INSERT** trigger.

::

   CREATE TRIGGER insert_trigger
              BEFORE INSERT ON test_trigger_src_tbl
              FOR EACH ROW
              EXECUTE PROCEDURE tri_insert_func();

Create an **UPDATE** trigger.

::

   CREATE TRIGGER update_trigger
              AFTER UPDATE ON test_trigger_src_tbl
              FOR EACH ROW
              EXECUTE PROCEDURE tri_update_func();

Create a **DELETE** trigger.

::

   CREATE TRIGGER delete_trigger
              BEFORE DELETE ON test_trigger_src_tbl
              FOR EACH ROW
              EXECUTE PROCEDURE tri_delete_func();

Helpful Links
-------------

:ref:`ALTER TRIGGER <dws_06_0147>`, :ref:`DROP TRIGGER <dws_06_0212>`, :ref:`ALTER TABLE <dws_06_0142>`
