:original_name: dws_04_0846.html

.. _dws_04_0846:

PLAN_TABLE
==========

**PLAN_TABLE** displays the plan information collected by **EXPLAIN PLAN**. Plan information is in a session-level life cycle. After the session exits, the data will be deleted. Data is isolated between sessions and between users.

.. table:: **Table 1** PLAN_TABLE columns

   +--------------+----------------+---------------------------------------------------------------------------------------------+
   | Column       | Type           | Description                                                                                 |
   +==============+================+=============================================================================================+
   | statement_id | varchar2(30)   | Query tag specified by a user                                                               |
   +--------------+----------------+---------------------------------------------------------------------------------------------+
   | plan_id      | Bigint         | ID of a plan to be queried                                                                  |
   +--------------+----------------+---------------------------------------------------------------------------------------------+
   | id           | Int            | ID of each operator in a generated plan                                                     |
   +--------------+----------------+---------------------------------------------------------------------------------------------+
   | operation    | varchar2(30)   | Operation description of an operator in a plan                                              |
   +--------------+----------------+---------------------------------------------------------------------------------------------+
   | options      | varchar2(255)  | Operation parameters                                                                        |
   +--------------+----------------+---------------------------------------------------------------------------------------------+
   | object_name  | Name           | Name of an operated object. It is defined by users, not the object alias used in the query. |
   +--------------+----------------+---------------------------------------------------------------------------------------------+
   | object_type  | varchar2(30)   | Object type                                                                                 |
   +--------------+----------------+---------------------------------------------------------------------------------------------+
   | object_owner | Name           | User-defined schema to which an object belongs                                              |
   +--------------+----------------+---------------------------------------------------------------------------------------------+
   | projection   | varchar2(4000) | Returned column information                                                                 |
   +--------------+----------------+---------------------------------------------------------------------------------------------+

.. note::

   -  A valid **object_type** value consists of a relkind type defined in :ref:`PG_CLASS <dws_04_0578>` (**TABLE** ordinary table, **INDEX**, **SEQUENCE**, **VIEW**, **FOREIGN TABLE**, **COMPOSITE TYPE**, or **TOASTVALUE TOAST** table) and the rtekind type used in the plan (**SUBQUERY**, **JOIN**, **FUNCTION**, **VALUES**, **CTE**, or **REMOTE_QUERY**).
   -  For RangeTableEntry (RTE), **object_owner** is the object description used in the plan. Non-user-defined objects do not have **object_owner**.
   -  Information in the **statement_id**, **object_name**, **object_owner**, and **projection** columns is stored in letter cases specified by users and information in other columns is stored in uppercase.
   -  **PLAN_TABLE** supports only **SELECT** and **DELETE** and does not support other DML operations.
