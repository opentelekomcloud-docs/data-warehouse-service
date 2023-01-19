:original_name: dws_04_0409.html

.. _dws_04_0409:

Query Execution Process
=======================

The process from receiving SQL statements to the statement execution by the SQL engine is shown in :ref:`Figure 1 <en-us_topic_0000001100671270__en-us_topic_0000001145894855_f80940097a2954477aa3804492a970ca9>` and :ref:`Table 1 <en-us_topic_0000001100671270__en-us_topic_0000001145894855_tf8c00935fe1f49dbadbcda59ade2fb28>`. The texts in red are steps where database administrators can optimize queries.

.. _en-us_topic_0000001100671270__en-us_topic_0000001145894855_f80940097a2954477aa3804492a970ca9:

.. figure:: /_static/images/en-us_image_0000001145815059.png
   :alt: **Figure 1** Execution process of query-related SQL statements by the SQL engine

   **Figure 1** Execution process of query-related SQL statements by the SQL engine

.. _en-us_topic_0000001100671270__en-us_topic_0000001145894855_tf8c00935fe1f49dbadbcda59ade2fb28:

.. table:: **Table 1** Execution process of query-related SQL statements by the SQL engine

   +----------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Procedure                              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
   +========================================+===================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
   | 1. Perform syntax and lexical parsing. | Converts the input SQL statements from the string data type to the formatted structure stmt based on the specified SQL statement rules.                                                                                                                                                                                                                                                                                                                                                                                                                           |
   +----------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 2. Perform semantic parsing.           | Converts the formatted structure obtained from the previous step into objects that can be recognized by the database.                                                                                                                                                                                                                                                                                                                                                                                                                                             |
   +----------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 3. Rewrite the query statements.       | Converts the output of the last step into the structure that optimizes the query execution.                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
   +----------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 4. Optimize the query.                 | Determines the execution mode of SQL statements (the execution plan) based on the result obtained from the last step and the internal database statistics. For details about the impact of statistics and GUC parameters on query optimization (execution plan), see :ref:`Optimizing Queries Using Statistics <en-us_topic_0000001100671270__en-us_topic_0000001145894855_seba6e5b861784d30a426c3f8b464d756>` and :ref:`Optimizing Queries Using GUC parameters <en-us_topic_0000001100671270__en-us_topic_0000001145894855_sd45492c661b74edea38db47f7641e549>`. |
   +----------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | 5. Perform the query.                  | Executes the SQL statements based on the execution path specified in the last step. Selecting a proper underlying storage mode improves the query execution efficiency. For details, see :ref:`Optimizing Queries Using the Underlying Storage <en-us_topic_0000001100671270__en-us_topic_0000001145894855_s1b84a2a8b5d9414c89efea23f12be975>`.                                                                                                                                                                                                                   |
   +----------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001100671270__en-us_topic_0000001145894855_seba6e5b861784d30a426c3f8b464d756:

Optimizing Queries Using Statistics
-----------------------------------

The GaussDB(DWS) optimizer is a typical Cost-based Optimization (CBO). By using CBO, the database calculates the number of tuples and the execution cost for each execution step under each execution plan based on the number of table tuples, column width, NULL record ratio, and characteristic values, such as distinct, MCV, and HB values, and certain cost calculation methods. The database then selects the execution plan that takes the lowest cost for the overall execution or for the return of the first tuple. These characteristic values are the statistics, which is the core for optimizing a query. Accurate statistics helps the planner select the most appropriate query plan. Generally, you can collect statistics of a table or that of some columns in a table using **ANALYZE**. You are advised to periodically execute **ANALYZE** or execute it immediately after you modified most contents in a table.

.. _en-us_topic_0000001100671270__en-us_topic_0000001145894855_sd45492c661b74edea38db47f7641e549:

Optimizing Queries Using GUC parameters
---------------------------------------

Optimizing queries aims to select an efficient execution mode.

Take the following statement as an example:

::

   select count(1)
   from customer inner join store_sales on (ss_customer_sk = c_customer_sk);

During execution of **customer inner join store_sales**, GaussDB(DWS) supports nested loop, merge join, and hash join. The optimizer estimates the result set value and the execution cost under each join mode based on the statistics of the **customer** and **store_sales** tables and selects the execution plan that takes the lowest execution cost.

As described in the preceding content, the execution cost is calculated based on certain methods and statistics. If the actual execution cost cannot be accurately estimated, you need to optimize the execution plan by setting the GUC parameters.

.. _en-us_topic_0000001100671270__en-us_topic_0000001145894855_s1b84a2a8b5d9414c89efea23f12be975:

Optimizing Queries Using the Underlying Storage
-----------------------------------------------

GaussDB(DWS) supports row- and column-based tables. The selection of an underlying storage mode strongly depends on specific customer business scenarios. You are advised to use column-store tables for computing service scenarios (mainly involving association and aggregation operations) and row-store tables for service scenarios, such as point queries and massive **UPDATE** or **DELETE** executions.

Optimization methods of each storage mode will be described in details in the performance optimization chapter.

Optimizing Queries by Rewriting SQL Statements
----------------------------------------------

Besides the preceding methods that improve the performance of the execution plan generated by the SQL engine, database administrators can also enhance SQL statement performance by rewriting SQL statements while retaining the original service logic based on the execution mechanism of the database and abundant practical experience.

This requires that the system administrators know the customer business well and have professional knowledge of SQL statements.
