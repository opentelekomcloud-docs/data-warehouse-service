:original_name: dws_04_0847.html

.. _dws_04_0847:

PLAN_TABLE_DATA
===============

**PLAN_TABLE_DATA** stores the plan information collected by **EXPLAIN PLAN**. Different from the :ref:`PLAN_TABLE <dws_04_0846>` view, the system catalog **PLAN_TABLE_DATA** stores the plan information collected by all sessions and users.

.. table:: **Table 1** PLAN_TABLE columns

   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name         | Type           | Description                                                                                                                                            |
   +==============+================+========================================================================================================================================================+
   | session_id   | text           | Session that inserts the data. Its value consists of a service thread start timestamp and a service thread ID. Values are constrained by **NOT NULL**. |
   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | user_id      | oid            | User who inserts the data. Values are constrained by **NOT NULL**.                                                                                     |
   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | statement_id | varchar2(30)   | Query tag specified by a user                                                                                                                          |
   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | plan_id      | bigint         | ID of a plan to be queried                                                                                                                             |
   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | id           | int            | Node ID in a plan                                                                                                                                      |
   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | operation    | varchar2(30)   | Operation description                                                                                                                                  |
   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | options      | varchar2(255)  | Operation parameters                                                                                                                                   |
   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | object_name  | name           | Name of an operated object. It is defined by users.                                                                                                    |
   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | object_type  | varchar2(30)   | Object type                                                                                                                                            |
   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | object_owner | name           | User-defined schema to which an object belongs                                                                                                         |
   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
   | projection   | varchar2(4000) | Returned column information                                                                                                                            |
   +--------------+----------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+

.. note::

   -  **PLAN_TABLE_DATA** records data of all users and sessions on the current node. Only administrators can access all the data. Common users can view only their own data in the :ref:`PLAN_TABLE <dws_04_0846>` view.
   -  Data of inactive (exited) sessions is cleaned from **PLAN_TABLE_DATA** by **gs_clean** after being stored in this system catalog for a certain period of time (5 minutes by default). You can also manually run **gs_clean -C** to delete inactive session data from the table..
   -  Data is automatically inserted into **PLAN_TABLE_DATA** after **EXPLAIN PLAN** is executed. Therefore, do not manually insert data into or update data in **PLAN_TABLE_DATA**. Otherwise, data in **PLAN_TABLE_DATA** may be disordered. To delete data from **PLAN_TABLE_DATA**, you are advised to use the :ref:`PLAN_TABLE <dws_04_0846>` view.
   -  Information in the **statement_id**, **object_name**, **object_owner**, and **projection** columns is stored in letter cases specified by users and information in other columns is stored in uppercase.
