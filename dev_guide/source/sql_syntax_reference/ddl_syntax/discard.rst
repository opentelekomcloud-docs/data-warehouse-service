:original_name: dws_06_0283.html

.. _dws_06_0283:

DISCARD
=======

Function
--------

Releases internal resources related to database sessions. The **DISCARD** command is used to reset the status of some or all sessions. Different **DISCARD** clauses release different types of resources. The **DISCARD ALL** command releases all temporary resources related to the current session and resets them to the initial state.

Precautions
-----------

None

Syntax
------

::

   DISCARD
           { VOLATILE { TEMPORARY | TEMP } }  |
           { ALL | TEMP | TEMPORARY | PLANS | SEQUENCES } }

Parameter Description
---------------------

-  **VOLATILE { TEMPORARY \| TEMP }**

   Releases resources related to the VOLATILE temporary table in the current session.

   .. caution::

      After the **DISCARD VOLATILE { TEMPORARY \| TEMP }** statement is executed, all volatile temporary table resources in the current session will be cleared. However, the statement cannot clear a single volatile temporary table resource.

-  **GLOBAL { TEMPORARY \| TEMP }** **[ TABLE table_name ]**

   -  Run the **DISCARD GLOBAL TEMP** command to release resources related to the global temporary table in the current session.
   -  **DISCARD GLOBAL TEMP TABLE table_name** releases resources of a specified global temporary table in the current session.

   .. caution::

      If a global temporary table occupies resources in a session, you need to run the **DISCARD** command to clear the resources of all sessions before performing DDL operations.

-  **TEMP \| TEMPORARY**

   Releases resources related to all temporary tables in the current session, including volatile and global temporary tables.

-  **PLANS**

   Releases all cached query plans in the current session and forces them to be replanned when related **PREPARE** statements are used next time.

-  **SEQUENCES**

   Discards all cached sequence-related states, including **currval()**/**lastval()** information and any pre-allocated sequence values that have not been returned through **nextval()**.

-  **ALL**

   Releases all temporary resources related to the current session and resets them to their initial state. This has almost the same effect as executing the following statement sequence:

   .. code-block::

      SET SESSION AUTHORIZATION DEFAULT;
      RESET ALL;
      DEALLOCATE ALL;
      CLOSE ALL;
      UNLISTEN *;
      SELECT pg_advisory_unlock_all();
      DISCARD PLANS; DISCARD SEQUENCES;
      DISCARD TEMP;

   .. caution::

      -  After **DISCARD ALL** is executed successfully, schemas starting with **pg_temp** and **pg_toast_temp** are also deleted.
      -  **DISCARD ALL** cannot be executed in a transaction.

Examples
--------

-  DISCARD global temporary tables

   .. code-block::

      CREATE GLOBAL TEMP TABLE t_global_temp(a int,b int);
      NOTICE:  The 'DISTRIBUTE BY' clause is not specified. Using round-robin as the distribution mode by default.
      HINT:  Please use 'DISTRIBUTE BY' clause to specify suitable data distribution column.
      CREATE TABLE

      INSERT INTO t_global_temp VALUES(1,1),(2,2);
      INSERT 0 2

      DROP TABLE t_global_temp;
      ERROR:  can not DROP TABLE when global temp table "t_global_temp" is in use

      DISCARD GLOBAL TEMP TABLE t_global_temp;

      DROP TABLE t_global_temp;
      DROP TABLE

-  DISCARD VOLATILE temporary tables

   After the DISCARD operation is performed, all resources related to volatile temporary tables in the current session are cleared.

   ::

      CREATE VOLATILE TEMP TABLE TX1(A INT) DISTRIBUTE BY HASH(A);
      CREATE TABLE
      CREATE VOLATILE TEMP TABLE TX2(A INT) DISTRIBUTE BY HASH(A);
      CREATE TABLE

      SELECT * FROM TX1;
       a
      ---
      (0 rows)
      SELECT * FROM TX2;
       a
      ---
      (0 rows)

      DISCARD VOLATILE TEMP;

      SELECT * FROM TX1;
      ERROR:  relation "tx1" does not exist
      LINE 1: SELECT * FROM TX1;
                            ^
      SELECT * FROM TX2;
      ERROR:  relation "tx2" does not exist
      LINE 1: SELECT * FROM TX2;

-  DISCARD TEMP

   After **DISCARD TEMP** is run, all temporary table resources in the current session are cleared.

   ::

      CREATE GLOBAL TEMP TABLE t_global_temp(a int,b int);
      NOTICE:  The 'DISTRIBUTE BY' clause is not specified. Using round-robin as the distribution mode by default.
      HINT:  Please use 'DISTRIBUTE BY' clause to specify suitable data distribution column.
      CREATE TABLE
      INSERT INTO t_global_temp VALUES(1,1),(2,2);
      INSERT 0 2

      CREATE VOLATILE TEMP TABLE t_volatile_temp(a int,b int);
      CREATE TEMP TABLE t_temp(a int,b int);

      DISCARD TEMP;

      SELECT * FROM t_global_temp;
       a | b
      ---+---
      (0 rows)

      SELECT * FROM t_volatile_temp;
      ERROR:  relation "t_volatile_temp" does not exist
      LINE 1: select * from t_volatile_temp;

      SELECT * FROM t_temp;
      ERROR:  relation "t_temp" does not exist
      LINE 1: select * from t_temp;
