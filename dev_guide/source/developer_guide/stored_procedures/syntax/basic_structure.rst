:original_name: dws_04_0520.html

.. _dws_04_0520:

Basic Structure
===============

Structure
---------

A PL/SQL block can contain a sub-block which can be placed in any section. The following describes the architecture of a PL/SQL block:

-  **DECLARE**: declares variables, types, cursors, and regional stored procedures and functions used in the PL/SQL block.

   .. code-block::

      DECLARE

   .. note::

      This part is optional if no variable needs to be declared.

      -  An anonymous block may omit the **DECLARE** keyword if no variable needs to be declared.
      -  For a stored procedure, **AS** is used, which is equivalent to **DECLARE**. The **AS** keyword must be reserved even if there is no variable declaration part.

-  **EXECUTION**: specifies procedure and SQL statements. It is the main part of a program. Mandatory

   .. code-block::

      BEGIN

-  **EXCEPTION**: processes errors. Optional

   .. code-block::

      EXCEPTION

-  **END**

   .. code-block::

      END;
      /

   .. important::

      You are not allowed to use consecutive tabs in the PL/SQL block, because they may result in an exception when the parameter **-r** is executed using the **gsql** tool.

Type
----

PL/SQL blocks are classified into the following types:

-  Anonymous block: a dynamic block that can be executed only for once. For details about the syntax, see :ref:`Figure 1 <en-us_topic_0000001145494671__f0d3f58494e2241b8867d81e8cc747d36>`.
-  Subprogram: a stored procedure, function, operator, or packages stored in a database. A subprogram created in a database can be called by other programs.
