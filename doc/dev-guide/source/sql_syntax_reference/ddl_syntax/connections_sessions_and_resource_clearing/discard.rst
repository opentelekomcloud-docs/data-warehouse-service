:original_name: dws_06_0283.html

.. _dws_06_0283:

DISCARD
=======

Function
--------

Releases internal resources related to database sessions.

The **DISCARD** command is used to reset the status of some or all sessions. Different **DISCARD** clauses release different types of resources. The **DISCARD ALL** command releases all temporary resources related to the current session and resets them to the initial state.

Precautions
-----------

-  After the **DISCARD VOLATILE { TEMPORARY \| TEMP }** statement is executed, all volatile temporary table resources in the current session will be cleared. However, the statement cannot clear a single volatile temporary table resource.
-  If a global temporary table occupies resources in a session, you need to run the **DISCARD** command to clear the resources of all sessions before performing DDL operations.

-  After **DISCARD ALL** is executed successfully, schemas starting with **pg_temp** and **pg_toast_temp** are also deleted.
-  **DISCARD ALL** cannot be executed in a transaction.

Syntax
------

::

   DISCARD {{ GLOBAL { TEMPORARY | TEMP } [ TABLE table_name ] } |
           { VOLATILE { TEMPORARY | TEMP } }  |
           { ALL | TEMP | TEMPORARY | PLANS | SEQUENCES }}

Parameter Description
---------------------

.. table:: **Table 1** DISCARD parameters

   +---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                         | Description                                                                                                                                                                             |
   +===================================================+=========================================================================================================================================================================================+
   | VOLATILE { TEMPORARY \| TEMP }                    | Releases resources related to the VOLATILE temporary table in the current session.                                                                                                      |
   +---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | GLOBAL { TEMPORARY \| TEMP } [ TABLE table_name ] | -  Run the **DISCARD GLOBAL TEMP** command to release resources related to the global temporary table in the current session.                                                           |
   |                                                   | -  **DISCARD GLOBAL TEMP TABLE table_name** releases resources of a specified global temporary table in the current session.                                                            |
   +---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TEMP \| TEMPORARY                                 | Releases resources related to all temporary tables in the current session, including volatile and global temporary tables.                                                              |
   +---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PLANS                                             | Releases all cached query plans in the current session and forces them to be replanned when related **PREPARE** statements are used next time.                                          |
   +---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SEQUENCES                                         | Discards all cached sequence-related states, including **currval()**/**lastval()** information and any pre-allocated sequence values that have not been returned through **nextval()**. |
   +---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ALL                                               | Releases all temporary resources related to the current session and resets them to their initial state. This has almost the same effect as executing the following statement sequence:  |
   |                                                   |                                                                                                                                                                                         |
   |                                                   | .. code-block::                                                                                                                                                                         |
   |                                                   |                                                                                                                                                                                         |
   |                                                   |    SET SESSION AUTHORIZATION DEFAULT;                                                                                                                                                   |
   |                                                   |    RESET ALL;                                                                                                                                                                           |
   |                                                   |    DEALLOCATE ALL;                                                                                                                                                                      |
   |                                                   |    CLOSE ALL;                                                                                                                                                                           |
   |                                                   |    UNLISTEN *;                                                                                                                                                                          |
   |                                                   |    SELECT pg_advisory_unlock_all();                                                                                                                                                     |
   |                                                   |    DISCARD PLANS; DISCARD SEQUENCES;                                                                                                                                                    |
   |                                                   |    DISCARD TEMP;                                                                                                                                                                        |
   +---------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

DISCARD global temporary tables

.. code-block::

   DROP TABLE IF EXISTS t_global_temp;
   CREATE GLOBAL TEMP TABLE t_global_temp(a int,b int);
   INSERT INTO t_global_temp VALUES(1,1),(2,2);
   DROP TABLE t_global_temp;

|image1|

.. code-block::

   DISCARD GLOBAL TEMP TABLE t_global_temp;
   DROP TABLE t_global_temp;

|image2|

The **DISCARD VOLATILE** command clears all resources related to volatile temporary tables in the current session.

::

   DROP TABLE IF EXISTS TX1;
   DROP TABLE IF EXISTS TX2;
   CREATE VOLATILE TEMP TABLE TX1(A INT) DISTRIBUTE BY HASH(A);
   CREATE VOLATILE TEMP TABLE TX2(A INT) DISTRIBUTE BY HASH(A);

   SELECT * FROM TX1;
   SELECT * FROM TX2;               ^

|image3|

::

   DISCARD VOLATILE TEMP;
   SELECT * FROM TX1;

|image4|

::

   SELECT * FROM TX2;

|image5|

After **DISCARD TEMP** is run, all temporary table resources in the current session are cleared.

::

   DROP TABLE IF EXISTS t_global_temp;
   CREATE GLOBAL TEMP TABLE t_global_temp(a int,b int);
   INSERT INTO t_global_temp VALUES(1,1),(2,2);
   DROP TABLE IF EXISTS t_volatile_temp;
   DROP TABLE IF EXISTS t_temp;
   CREATE VOLATILE TEMP TABLE t_volatile_temp(a int,b int);
   CREATE TEMP TABLE t_temp(a int,b int);

   DISCARD TEMP;
   SELECT * FROM t_global_temp;

|image6|

::

   SELECT * FROM t_volatile_temp;

|image7|

::

   SELECT * FROM t_temp;

|image8|

.. |image1| image:: /_static/images/en-us_image_0000002611632297.png
.. |image2| image:: /_static/images/en-us_image_0000002611632449.png
.. |image3| image:: /_static/images/en-us_image_0000002581233602.png
.. |image4| image:: /_static/images/en-us_image_0000002611633027.png
.. |image5| image:: /_static/images/en-us_image_0000002611633103.png
.. |image6| image:: /_static/images/en-us_image_0000002611633753.png
.. |image7| image:: /_static/images/en-us_image_0000002611713949.png
.. |image8| image:: /_static/images/en-us_image_0000002611714013.png
