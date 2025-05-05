:original_name: dws_04_0126.html

.. _dws_04_0126:

SELECT Operation
================

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section1749794416463:

Rule 3.7: Avoiding Executing SQL Statements That Do Not Support Pushdown
------------------------------------------------------------------------

.. note::

   GaussDB(DWS) uses a distributed architecture, and to achieve optimal performance, SQL statements need to be pushed down to utilize distributed computing resources.

   **Impact of rule violation:**

   -  SQL statements that are not pushed down may experience poor execution performance and, in severe cases, can lead to CN resource bottlenecks, impacting overall services.

   **Solution:**

   -  Do not use syntax or functions that cannot be executed near the data source. For details, see :ref:`Optimizing Statement Pushdown <dws_04_0447>`.

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section17685947134614:

Rule 3.8: Specifying Association Conditions when Multiple Tables Are Associated
-------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  If no association condition is specified when linking multiple tables, it will result in a Cartesian product calculation. This can lead to an expanded result set, posing risks of performance issues and resource overload.

   **Solution:**

   -  Specify filter and association conditions for each table during the association process.

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section1777795264615:

Rule 3.9: Ensuring Consistency of Data Types in Associated Fields across Multiple Tables
----------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Ensure consistent data types for associated fields to avoid unnecessary type conversions, data redistribution issues, and hindered generation of optimal plans.

   **Solution:**

   -  Use the same data type for associated fields when tables are associated.

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section18864134354719:

Suggestion 3.10: Avoiding Function Calculation on Association and Filter Condition Fields
-----------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  In cases where function calculations are involved in association and filter conditions, the optimizer may fail to obtain accurate field statistics, impacting execution performance.

   **Solution:**

   -  When comparing association condition fields, preprocess the data before importing it into the database, especially when calculations are required for comparison.

   -  When filter criteria are compared with constants, perform function calculation only on constant columns. The following is an example:

      ::

         SELECT id, from_image_id, from_person_id, from_video_id
         FROM face_data
         WHERE SS.DEL_FLAG = 'N'
         AND NVL(SS.DELETE_FLAG, 'N') = 'N'
         The modification is as follows:
         SELECT id, from_image_id, from_person_id, from_video_id
         FROM face_data
         where SS.DEL_FLAG = 'N'
         AND (SS.DELETE_FLAG = 'N' or SS.DELETE_FLAG is null)

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section152761264813:

Suggestion 3.11: Performing Pressure Tests and Concurrency Control for Resource-intensive SQL Statements
--------------------------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Storage and computing resources are overloaded, and the overall running performance deteriorates.

   **Solution:**

   A resource-intensive SQL statement contains:

   -  A large number of **UNION ALL**.
   -  A large number of AGGs (such as **COUNT DISTINCT** and **MAX**).
   -  A lot of **JOIN** operations for a large number of tables.
   -  A large number of **STREAM** operators (plan dimension).

   Before rolling out, conduct pressure tests and implement concurrency control for these SQL statements. If the resource capacity is exceeded, optimizing the service should be prioritized before reassessing the rollout plan.

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section113151534184817:

Rule 3.12: Avoiding Excessive COUNT Operations on Large Row-store Tables
------------------------------------------------------------------------

.. note::

   If SSDs or other high-performance disk types are used, it may not be necessary to adhere strictly to this rule, but it is still crucial to monitor the I/O consumption.

   **Impact of rule violation:**

   -  Performing frequent **COUNT** operations on large row-store tables can consume a significant amount of I/O resources, potentially leading to performance issues if an I/O bottleneck occurs.

   **Solution:**

   -  Reduce the frequency of **COUNT** operations, use result caching, and collect statistics by partition to minimize I/O consumption.

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section16145155217486:

Suggestion 3.13: Avoid Getting Large Result Sets (Except for Data Exports)
--------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  If you do not need to view all the results, querying ultra-large result sets becomes inefficient and wasteful in terms of resources.

   **Solution:**

   -  Use the **LIMIT** clause to retrieve only the necessary result segments.
   -  Use a cursor to obtain the result sets by segment and set an appropriate value for **FETCH SIZE** if you need to query a large number of result sets.

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section18347121492:

Suggestion 3.14: Avoiding the Usage of SELECT \* for Queries
------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Querying unnecessary columns increases the computing load and wastes computing resources.

   **Solution:**

   -  Clearly list the fields required for the query in the **SELECT** statement to improve the query performance.

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section186162944912:

Suggestion 3.15: Using WITH RECURSIVE with Defined Termination Condition for Recursion
--------------------------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  In cases where there is no specific termination condition, recursive operations can enter an infinite loop.
   -  Recursive operations generate duplicate data and occupy excessive resources.

   **Solution:**

   -  Design proper termination conditions based on the volume and characteristics of the data in the service table.

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section19367195264917:

Suggestion 3.16: Setting Schema Prefix for Table and Function Access
--------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  If the schema name prefix is not specified, the search will be performed sequentially across all tablespaces based on the tablespace list in the current **search_path**. This can lead to accessing unexpected tables due to schema switchover.

   **Solution:**

   -  To enhance readability, stability, and portability, explicitly specify the schema prefix as **SCHEMA.** when accessing tables and function objects.

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section72646917506:

Suggestion 3.17: Identifying an SQL Statement with a Unique SQL Comment
-----------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  The service's source tracing capability is limited. You can only verify it with R&D engineers using the database, user name, and client IP address.

   **Solution:**

   -  You are advised to use **query_band**. The following is an example:

      ::

         SET query_band='JobName=abc;AppName=test;UserName=user';

   -  Add a unique comment for each SQL statement to facilitate troubleshooting and application performance analysis. The following is an example of such comment.

      .. code-block::

         /* Module name_Tool name_Job name_Step */, for example, /* mca_python_xxxxxx_step1 */ insert into xxx select … from

.. _en-us_topic_0000002136185113__en-us_topic_0000002135806941_section10314162412165:

Recommendation 3.18: Restricting SQL Statements to 64 KB in Length
------------------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  SQL parsing is time-consuming and difficult to maintain. Frequent execution of SQL statements leads to severe log expansion.

   **Solution:**

   -  Set a 64 KB limit on SQL statements.
