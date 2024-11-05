:original_name: dws_06_0257.html

.. _dws_06_0257:

BEGIN
=====

Function
--------

Initiates an anonymous block or a single transaction. This section describes the syntax of **BEGIN** used to initiate an anonymous block. For details about the **BEGIN** syntax that initiates transactions, see :ref:`START TRANSACTION <dws_06_0265>`.

An anonymous block is a structure that can dynamically create and execute stored procedure code instead of permanently storing code as a database object in the database.

Precautions
-----------

None

Syntax
------

-  Enable an anonymous block:

   ::

      [DECLARE [declare_statements]]
      BEGIN
      execution_statements
      END;
      /

-  -- Start a transaction:

   ::

      BEGIN [ WORK | TRANSACTION ]
        [
          {
             ISOLATION LEVEL { READ COMMITTED | READ UNCOMMITTED | SERIALIZABLE | REPEATABLE READ }
             | { READ WRITE | READ ONLY }
            } [, ...]
        ];

Parameter Description
---------------------

-  **declare_statements**

   Declares a variable, including its name and type, for example, **sales_cnt int**.

-  **execution_statements**

   Specifies the statement to be executed in an anonymous block.

   Value range: an existing function name

Examples
--------

-  Start a transaction block.

   ::

      BEGIN;

-  Start a transaction block at the REPEATABLE READ isolation level.

   ::

      BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;

-  Generate a string using an anonymous block.

   ::

      BEGIN
      dbms_output.put_line('Hello');
      END;
      /

Helpful Links
-------------

:ref:`START TRANSACTION <dws_06_0265>`
