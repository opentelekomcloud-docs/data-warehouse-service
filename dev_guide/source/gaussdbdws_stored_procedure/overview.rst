:original_name: dws_04_0513.html

.. _dws_04_0513:

Overview
========

What Is a GaussDB(DWS) Stored Procedure?
----------------------------------------

In GaussDB(DWS), business rules and logics are saved as stored procedures.

A stored procedure is a combination of SQL, PL/SQL, and Java statements. Stored procedures can move the code that executes business rules from applications to databases. In this way, code can be used by multiple programs at a time.

For how to create and invoke a stored procedure, see "CREATE PROCEDURE" in the *Data Warehouse Service (DWS) SQL Syntax Reference*.

The functions created using the PL/pgSQL language mentioned in :ref:`GaussDB(DWS) PL/pgSQL Functions <dws_04_0510>` are similar to the application methods of stored procedures. Unless otherwise specified, the following sections apply to stored procedures and PL/pgSQL functions.

GaussDB(DWS) Stored Procedure Data Types
----------------------------------------

A data type refers to a value set and an operation set defined on the value set. A GaussDB(DWS) database consists of tables, each of which is defined by its own columns. Each column corresponds to a data type. GaussDB(DWS) uses corresponding functions to perform operations on data based on data types. For example, GaussDB(DWS) can perform addition, subtraction, multiplication, and division operations on data of numeric values.
