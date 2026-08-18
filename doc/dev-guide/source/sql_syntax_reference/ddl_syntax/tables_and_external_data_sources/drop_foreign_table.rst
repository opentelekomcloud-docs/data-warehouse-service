:original_name: dws_06_0192.html

.. _dws_06_0192:

DROP FOREIGN TABLE
==================

Function
--------

**DROP FOREIGN TABLE** deletes a specified foreign table.

Precautions
-----------

**DROP FOREIGN TABLE** forcibly deletes a specified table. After a table is deleted, any indexes that exist for the table will be deleted. The functions and stored procedures used in this table cannot be run.

Syntax
------

::

   DROP FOREIGN TABLE [ IF EXISTS ]
       table_name [, ...] [ CASCADE | RESTRICT ];

Parameter Description
---------------------

.. table:: **Table 1** DROP FOREIGN TABLE parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                         | Value Range                                                                                                       |
   +=======================+=====================================================================================================+===================================================================================================================+
   | IF EXISTS             | Sends a message instead of an error if the specified foreign table does not exist.                  | ``-``                                                                                                             |
   +-----------------------+-----------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | table_name            | Specifies the name of the foreign table to be deleted.                                              | An existing table name.                                                                                           |
   +-----------------------+-----------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------+
   | CASCADE \| RESTRICT   | Specifies how to process related data in the dependent object when a delete operation is performed. | -  **CASCADE**: automatically deletes all objects (such as views) that depend on the foreign table to be deleted. |
   |                       |                                                                                                     | -  **RESTRICT**: refuses to delete the foreign table if any objects depend on it. This is the default behaviour.  |
   +-----------------------+-----------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------+

Examples
--------

Delete the foreign table named **customer_ft**:

::

   DROP FOREIGN TABLE IF EXISTS customer_ft;

Helpful Links
-------------

:ref:`ALTER FOREIGN TABLE (GDS Import and Export) <dws_06_0123>`, :ref:`ALTER FOREIGN TABLE (for HDFS or OBS) <dws_06_0124>`, :ref:`CREATE FOREIGN TABLE (for GDS Import and Export) <dws_06_0159>`, :ref:`CREATE FOREIGN TABLE (SQL on OBS or Hadoop) <dws_06_0161>`
