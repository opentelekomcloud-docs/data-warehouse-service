:original_name: dws_06_0233.html

.. _dws_06_0233:

EXPLAIN PLAN
============

Function
--------

Saves the information about an execution plan to the **PLAN_TABLE** table. Different from the **EXPLAIN** statement, **EXPLAIN PLAN** only stores plan information and does not print it on the screen.

Syntax
------

::

   EXPLAIN PLAN
   [ SET STATEMENT_ID = string ]
   FOR statement ;

Parameter Description
---------------------

-  **PLAN**

   Stores plan information in **PLAN_TABLE**. If the storing is successful, **EXPLAIN SUCCESS** is returned.

-  **STATEMENT_ID**

   Tags a query. The tag information will be stored in **PLAN_TABLE**.

   .. note::

      If the **EXPLAIN PLAN** statement does not contain **SET STATEMENT_ID**, the value of **STATEMENT_ID** is empty by default. In addition, the value of **STATEMENT_ID** cannot exceed 30 bytes. Otherwise, an error will be reported.

Precautions
-----------

-  **EXPLAIN PLAN** cannot be executed on DNs.
-  Plan information cannot be collected for SQL statements that failed to be executed.
-  Data in **PLAN_TABLE** is in a session-level life cycle. Sessions are isolated from users and thereby users can view data of only the current session and current user.
-  **PLAN_TABLE** cannot be joined with GDS foreign tables.
-  For a query that cannot be pushed down, object information cannot be collected and only such information as **REMOTE_QUERY** and **CTE** can be collected. For details, see :ref:`Example 2 <en-us_topic_0000001460561412__s30671eba40e14af88f1e45bd5bbd6c5d>`.

Example 1
---------

You can perform the following steps to collect execution plans of SQL statements by running **EXPLAIN PLAN**:

#. Import TPC-H sample data.

#. Run the **EXPLAN PLAN** statement.

   .. note::

      After the **EXPLAIN PLAN** statement is executed, plan information is automatically stored in **PLAN_TABLE**. **INSERT**, **UPDATE**, and **ANALYZE** cannot be performed on **PLAN_TABLE**.

      For details about **PLAN_TABLE**, see the PLAN_TABLE system view.

   ::

      explain plan set statement_id='TPCH-Q4' for
      select
      o_orderpriority,
      count(*) as order_count
      from
      orders
      where
      o_orderdate >= '1993-07-01'::date
      and o_orderdate < '1993-07-01'::date + interval '3 month'
      and exists (
      select
      *
      from
      lineitem
      where
      l_orderkey = o_orderkey
      and l_commitdate < l_receiptdate
      )
      group by
      o_orderpriority
      order by
      o_orderpriority;

#. Query **PLAN_TABLE**.

   ::

      SELECT * FROM PLAN_TABLE;

   |image1|

#. Delete data from **PLAN_TABLE**.

   .. code-block:: text

      DELETE FROM PLAN_TABLE WHERE xxx;

.. _en-us_topic_0000001460561412__s30671eba40e14af88f1e45bd5bbd6c5d:

Example 2
---------

For a query that cannot be pushed down, only such information as **REMOTE_QUERY** and **CTE** can be collected from **PLAN_TABLE** after **EXPLAIN PLAN** is executed.

The optimizer generates a plan for pushing down statements. In this case, only **REMOTE_QUERY** can be collected.

::

     explain plan set statement_id = 'test remote query' for
     select
     current_user
     from
     customer;

Query **PLAN_TABLE**.

::

   SELECT * FROM PLAN_TABLE;

|image2|

.. |image1| image:: /_static/images/en-us_image_0000001460561528.png
.. |image2| image:: /_static/images/en-us_image_0000001510401077.png
