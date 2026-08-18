:original_name: dws_06_0292.html

.. _dws_06_0292:

CREATE BLOCK RULE
=================

Function
--------

**CREATE BLOCK RULE** creates a query filtering rule, including the query filtering rule name, bound client name, client IP address, user, and matching mode. During routine database operations, executing abnormal SQL statements may use too many resources, affecting database performance and stability. You can create query filtering rules to intercept such SQL statements.

Currently, you can specify the regular expression of an SQL statement for interception. For example, intercepting the SELECT statement of a table of a user. You can also specify the value of **unique_sql_id** or **sql_hash** of an SQL statement for interception.

This document provides basic SQL syntax. You can also configure the query filtering rules on the console. For details, see "Using DWS Query Filters to Intercept Slow SQL Statements" in the *User Guide*.

Precautions
-----------

-  This syntax is supported only by clusters of version 9.1.0.100 or later.

-  Only a user with the database owner permission or the **gs_role_block** role permission can run **CREATE BLOCK RULE**. A system administrator has this permission by default.

-  Common users cannot run **CREATE BLOCK RULE**. To run it, they must be authorized by the system administrator. The authorization syntax is as follows:

   ::

      GRANT gs_role_block TO user;

   You are advised to narrow down the application scope when creating a query filtering rule to improve performance.

Syntax
------

::

   CREATE BLOCK RULE [ IF NOT EXISTS ] block_name
       [ [ TO user_name@'host' ] | [ TO user_name ] | [ TO 'host' ] ] |
       [ FOR UPDATE | SELECT | INSERT | DELETE | MERGE ] |
       FILTER BY
       { SQL ( 'text' ) | TEMPLATE ( template_parameter = value ) }
       [ WITH ( { with_parameter = value }, [, ... ] ) ];

Parameter Description
---------------------

.. table:: **Table 1** CREATE BLOCK RULE parameters

   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                                                                                                                                                                                                  | Value Range or Example                                                                                                                                                                                 |
   +=======================+==============================================================================================================================================================================================================================================================================+========================================================================================================================================================================================================+
   | block_name            | Specifies the name of the filtering rule to be created.                                                                                                                                                                                                                      | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.                                                                                       |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TO                    | Specifies the object or client IP address that determines whether the filtering rule takes effect. The options are as follows:                                                                                                                                               | The query filtering rule takes effect only when the client IP address is **192.168.0.59** and the login username is **user1**.                                                                         |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       | -  **TO user_name@'host'**: The filtering rule takes effect only when both the username and client IP address are met.                                                                                                                                                       | ::                                                                                                                                                                                                     |
   |                       | -  **TO user_name**: The query filtering rule takes effect only when the username is met, regardless of the client IP address.                                                                                                                                               |                                                                                                                                                                                                        |
   |                       | -  **TO 'host'**: The query filtering rule takes effect only when the client IP address is met, regardless of the username.                                                                                                                                                  |    CREATE BLOCK RULE query_block TO user1@'192.168.0.59' FOR SELECT  FILTER BY SQL('test.test');                                                                                                       |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              | The query filtering rule takes effect only when the login username is **user1**, regardless of the client IP address.                                                                                  |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              | ::                                                                                                                                                                                                     |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              |    CREATE BLOCK RULE query_block TO user1 FOR SELECT  FILTER BY SQL('test.test');                                                                                                                      |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              | The query filtering rule takes effect only when the client IP address is **192.168.0.59**, regardless of the login user.                                                                               |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              | ::                                                                                                                                                                                                     |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              |    CREATE BLOCK RULE query_block TO '192.168.0.59' FOR SELECT  FILTER BY SQL('test.test');                                                                                                             |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | FOR                   | Specifies the statement type. Its value can be UPDATE, SELECT, INSERT, DELETE, or MERGE INTO.                                                                                                                                                                                | ``-``                                                                                                                                                                                                  |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | FILTER BY             | Specifies the filtering method. Value:                                                                                                                                                                                                                                       | Query the values of **unique_sql_id** and **sql_hash** using EXPLAIN VERBOSE.                                                                                                                          |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       | -  **SQL**: SQL statements are filtered based on its content. Regular expression matching is performed on statements based on keywords. For example, the length of a table name cannot exceed 1024 characters. It is recommended that the keywords be as simple as possible. | ::                                                                                                                                                                                                     |
   |                       | -  **TEMPLATE**: SQL statements are filtered based on templates. This method intercepts specific statements executed by all users, which is more efficient. The options are as follows:                                                                                      |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              |    EXPLAIN VERBOSE SELECT * FROM test.test;                                                                                                                                                            |
   |                       |    -  **unique_sql_id**: normalized 64-bit hash value, which is more likely to be repeated than **sql_hash**.                                                                                                                                                                |                                                                                                                                                                                                        |
   |                       |    -  **sql_hash**: normalized hash value (MD5), which is generally unique and is recommended compared with **unique_sql_id**.                                                                                                                                               | |image1|                                                                                                                                                                                               |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | with_parameter        | Indicates the parameters for querying filter rule details.                                                                                                                                                                                                                   | The following parameters can be set together. The query that meets any of the parameters will be filtered.                                                                                             |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              | -  **application_name**: client name.                                                                                                                                                                  |
   |                       |                                                                                                                                                                                                                                                                              | -  **query_band**: load identifier.                                                                                                                                                                    |
   |                       |                                                                                                                                                                                                                                                                              | -  **table_num**: number of base tables scanned by the statement.                                                                                                                                      |
   |                       |                                                                                                                                                                                                                                                                              | -  **partition_num**: maximum number of partitions to be scanned by the operator.                                                                                                                      |
   |                       |                                                                                                                                                                                                                                                                              | -  **estimate_row**: estimated maximum number of table rows scanned by the operator.                                                                                                                   |
   |                       |                                                                                                                                                                                                                                                                              | -  **resource_pool**: name of the resource pool to be switched to.                                                                                                                                     |
   |                       |                                                                                                                                                                                                                                                                              | -  **max_active_num**: maximum number of concurrent statements corresponding to the rule.                                                                                                              |
   |                       |                                                                                                                                                                                                                                                                              | -  **is_warning**: whether an alarm or error is reported when a statement is intercepted. **on** indicates that an alarm is reported, and **off** (default value) indicates that an error is reported. |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Example: Intercept SQL Statements Based on Keywords
---------------------------------------------------

Connect to the database as the system administrator and run the following SQL statements to create a temporary table (**src**) to provide data sources for subsequent INSERT operations:

::

   DROP SCHEMA IF EXISTS test CASCADE;
   CREATE SCHEMA test;
   CREATE TABLE test.src AS SELECT 1;

Run the following SQL statements to create table **test.test**:

::

   DROP TABLE IF EXISTS test.test;
   CREATE TABLE test.test(a int, b numeric(15,2)) WITH(orientation=column);

Run the SQL statements to insert 20 million rows of data into the two tables. It takes about half a minute. Please wait.

::

   INSERT INTO test.test SELECT generate_series(1,20000000) % 1000,generate_series(1,20000000) FROM test.src;

Create **user1** and grant the user the permission to access table **test.test**.

::

   DROP USER IF EXISTS user1 CASCADE;
   CREATE USER user1 PASSWORD '{password}';
   GRANT USAGE ON SCHEMA test TO user1;
   GRANT SELECT ON TABLE test.test TO user1;

Connect to the database as **user1** and query table data. The query result shows that **user1** can query data in table **test.test** without creating a filtering rule

::

   SELECT sum(b) FROM test.test GROUP BY a;

|image2|

Connect to the database as a system administrator and create a query filtering rule (query_block) to intercept the SQL statements queried by **user1**. The filtering method is **SQL**. If an SQL statement contains the keyword **test** (table name), the SQL statement is filtered.

::

   CREATE BLOCK RULE query_block
   TO user1@'192.168.0.59'        --The rule applies to user1 and the client IP address is 192.168.0.59.
   FOR SELECT                      --Filter SELECT-related operations.
   FILTER BY
   SQL('test.test')               --The filtering mode is SQL. Any statements that contain keyword test (table name) are intercepted.
   WITH                            --The statements that meet the following conditions are intercepted.
   (
   application_name='gsql',       --The client name is gsql.
   table_num='1',                 --The number of base table scans is 1.
   estimate_row='10',             --The number of rows in the base table to be scanned is 10.
   is_warning='off''               --An error is reported.
   )
   ;

Query the created filtering rule by using the **pg_get_blockruledef** view. The command output indicates that the rule has taken effect.

::

   SELECT * FROM pg_get_blockruledef('query_block');

|image3|

Switch back to the user, connect to the database, and query the following SQL statement again. If information similar to "the statement is intercepted: ERROR: hit block rule query_block xxx" is displayed, the statement is intercepted.

::

   SELECT sum(b) FROM test.test GROUP BY a;

|image4|

Example: Intercept SQL Statements Based on the **unique_sql_id** or **sql_hash**
--------------------------------------------------------------------------------

Continue with the preceding example. Switch back to the system administrator, connect to the database, and run the following command to query **unique_sql_id** and **sql_hash**:

::

   EXPLAIN VERBOSE  SELECT sum(b) FROM test.test GROUP BY a;

Record **unique_sql_id** and **sql_hash** in the command output.

|image5|

Delete the original filtering rule and create one based on **unique_sql_id**.

::

   DROP BLOCK RULE query_block;
   CREATE BLOCK RULE query_block FILTER BY TEMPLATE(unique_sql_id='3324222175');

Switch to **user1** again. An error is reported and the statement is intercepted.

::

   SELECT sum(b) FROM test.test GROUP BY a;

|image6|

Similarly, execute the statement as the system administrator. The statement is intercepted. It can be concluded that the **unique_sql_id** is used to filter and intercept SQL statements regardless of users. The SQL statement executed by all users is intercepted, and the interception effect is better.

::

   SELECT sum(b) FROM test.test GROUP BY a;

|image7|

Delete the filtering rule and create one based on **sql_hash**.

::

   DROP BLOCK RULE query_block;
   CREATE BLOCK RULE query_block FILTER BY TEMPLATE(sql_hash='sql_170d0c75c472043b0cd35e05903c382f');

Switch to **user1** again. An error is reported and the statement is intercepted.

|image8|

Helpful Links
-------------

:ref:`ALTER BLOCK RULE <dws_06_0291>`, :ref:`DROP BLOCK RULE <dws_06_0293>`

This document provides basic SQL syntax. You can also configure the query filtering rules on the console. For details, see "Using DWS Query Filters to Intercept Slow SQL Statements" in the *User Guide*.

FAQ: Why Does the Statement Fail to Be Intercepted After a Query Filtering Rule Is Created?
-------------------------------------------------------------------------------------------

Some fields may not be intercepted as expected in different plans. For example:

::

   CREATE BLOCK RULE test filter BY SQL('test')with(estimate_row='3');
   SELECT * FROM test;

|image9|

In this case, the statement keyword is matched, and the number of rows queried exceeds the upper limit of three rows, but the statement is not intercepted.

|image10|

The **FQS (fast_query_shipping)** plan is used, that is, the **enable_fast_query_shipping** parameter is set to **on**. As a result, no estimation information is available. The interception fails. CN lightweight plan will cause the same result. If the statement is forced to use the stream plan, the interception is successful.

.. code-block::

   SET enable_stream_operator=on;
   SET enable_fast_query_shipping=off;

|image11|

In conclusion, if the estimation information is inaccurate, false interception or missing interception may occur. Plan information is obtained through estimation, so this error cannot be avoided.

FAQ: How Do I Check Interception Records?
-----------------------------------------

After a query filtering rule is created, many slow SQL statements are intercepted. You can view the intercepted statements in the system catalog **GS_WLM_SESSION_INFO**. The **abort_info** column records the interception information, that is, the error information of the query.

Connect to the **postgres** database and run the following command:

::

   SELECT abort_info,query FROM GS_WLM_SESSION_INFO WHERE abort_info like '%hit block rule%'; -- Querying all interception records
   SELECT abort_info,query FROM GS_WLM_SESSION_INFO WHERE abort_info like '%hit block rule query_block_new%' -- Querying only the interception records whose rule name is query_block_new.

|image12|

.. |image1| image:: /_static/images/en-us_image_0000002503084135.png
.. |image2| image:: /_static/images/en-us_image_0000002503311155.png
.. |image3| image:: /_static/images/en-us_image_0000002503311557.png
.. |image4| image:: /_static/images/en-us_image_0000002503311235.png
.. |image5| image:: /_static/images/en-us_image_0000002470231862.png
.. |image6| image:: /_static/images/en-us_image_0000002503151925.png
.. |image7| image:: /_static/images/en-us_image_0000002503313043.png
.. |image8| image:: /_static/images/en-us_image_0000002470232168.png
.. |image9| image:: /_static/images/en-us_image_0000002470517408.png
.. |image10| image:: /_static/images/en-us_image_0000002503437699.png
.. |image11| image:: /_static/images/en-us_image_0000002503597679.png
.. |image12| image:: /_static/images/en-us_image_0000002503439459.png
