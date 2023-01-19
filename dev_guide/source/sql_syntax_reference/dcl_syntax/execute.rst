:original_name: dws_06_0248.html

.. _dws_06_0248:

EXECUTE
=======

Function
--------

**EXECUTE** executes a prepared statement. A prepared statement only exists in the lifecycle of a session. Therefore, only prepared statements created using **PREPARE** earlier in the session can be executed.

Precautions
-----------

If the **PREPARE** statement creating the prepared statement declares certain parameters, the parameter set transferred to the **EXECUTE** statement must be compatible. Otherwise, an error occurs.

Syntax
------

::

   EXECUTE name [ ( parameter [, ...] ) ];

Parameter Description
---------------------

-  **name**

   Specifies the name of the statement to be executed.

-  **parameter**

   Specifies a parameter of the prepared statement. It must be an expression that generates a value compatible with the data type specified when the prepared statement is created.

Examples
--------

Create and run a prepared statement for the **INSERT** statement.

::

   PREPARE insert_reason(integer,character(16),character(100)) AS INSERT INTO tpcds.reason_t1 VALUES($1,$2,$3);
   EXECUTE insert_reason(52, 'AAAAAAAADDAAAAAA', 'reason 52');

Helpful Links
-------------

:ref:`PREPARE <dws_06_0251>`
