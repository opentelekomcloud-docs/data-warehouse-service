:original_name: dws_06_0248.html

.. _dws_06_0248:

EXECUTE
=======

Function
--------

**EXECUTE** executes a prepared statement. Because a prepared statement exists only in the lifetime of a session, the prepared statement must be created by an earlier **PREPARE** statement in the current session.

Precautions
-----------

If the **PREPARE** statement creating the prepared statement declares certain parameters, the parameter set transferred to the **EXECUTE** statement must be compatible. Otherwise, an error occurs.

Syntax
------

::

   EXECUTE name [ ( parameter [, ...] ) ];

Parameter Description
---------------------

.. table:: **Table 1** EXECUTE parameters

   +-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | Parameter | Description                                                                                                                                                                      | Value Range                                                                                                   |
   +===========+==================================================================================================================================================================================+===============================================================================================================+
   | name      | Specifies the name of the statement to be executed.                                                                                                                              | A string, which must comply with the naming rules of prepared statements.                                     |
   +-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | parameter | Specifies a parameter of the prepared statement. It must be an expression that generates a value compatible with the data type specified when the prepared statement is created. | A string, which is an expression of a value that is compatible with the data type of the specified parameter. |
   +-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------+

Examples
--------

Prepare data.

::

   CREATE SCHEMA IF NOT EXISTS tpcds;
   DROP TABLE IF EXISTS  tpcds.reason_t1;
   CREATE TABLE tpcds.reason_t1 (
       r_reason_sk    integer,
       r_reason_id    character(16),
       r_reason_desc  character(100)
   );

Create and run a prepared statement for the **INSERT** statement:

::

   PREPARE insert_reason(integer,character(16),character(100)) AS INSERT INTO tpcds.reason_t1 VALUES($1,$2,$3);
   EXECUTE insert_reason(52, 'AAAAAAAADDAAAAAA', 'reason 52');
