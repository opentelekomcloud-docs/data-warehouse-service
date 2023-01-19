:original_name: dws_06_0251.html

.. _dws_06_0251:

PREPARE
=======

Function
--------

**PREPARE** creates a prepared statement.

A prepared statement is a performance optimizing object on the server. When the **PREPARE** statement is executed, the specified query is parsed, analyzed, and rewritten. When the **EXECUTE** is executed, the prepared statement is planned and executed. This avoids repetitive parsing and analysis. After the PREPARE statement is created, it exists throughout the database session. Once it is created (even if in a transaction block), it will not be deleted when a transaction is rolled back. It can only be deleted by explicitly invoking :ref:`DEALLOCATE <dws_06_0246>` or automatically deleted when the session ends.

Precautions
-----------

None

Syntax
------

::

   PREPARE name [ ( data_type [, ...] ) ] AS statement;

Parameter Description
---------------------

-  **name**

   Specifies the name of a prepared statement. It must be unique in the current session.

-  **data_type**

   Specifies the type of a parameter.

-  **statement**

   Specifies a **SELECT**, **INSERT**, **UPDATE**, **DELETE**, or **VALUES** statement.

Examples
--------

Create and run a prepared statement for the **INSERT** statement.

::

   PREPARE insert_reason(integer,character(16),character(100)) AS INSERT INTO tpcds.reason_t1 VALUES($1,$2,$3);
   EXECUTE insert_reason(52, 'AAAAAAAADDAAAAAA', 'reason 52');

Helpful Links
-------------

:ref:`DEALLOCATE <dws_06_0246>`, :ref:`EXECUTE <dws_06_0248>`
