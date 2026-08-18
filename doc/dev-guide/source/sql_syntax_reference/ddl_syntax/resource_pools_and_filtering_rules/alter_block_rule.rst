:original_name: dws_06_0291.html

.. _dws_06_0291:

ALTER BLOCK RULE
================

Function
--------

This function modifies the attributes of a filtering rule, including the filtering rule name, bound client name, client IP address, user, and matching mode.

During routine database operations, executing abnormal SQL statements may use too many resources, affecting database performance and stability. You can create query filtering rules to intercept such SQL statements.

Precautions
-----------

-  This syntax is supported only by clusters of version 9.1.0.100 or later.

-  Only a user with the database owner permission or the **gs_role_block** role permission can run the **ALTER BLOCK RULE** statement. A system administrator has this permission by default.

-  Common users cannot run **CREATE BLOCK RULE**. To run it, they must be authorized by the system administrator. The authorization syntax is as follows:

   ::

      GRANT gs_role_block TO user;

   You are advised to narrow down the application scope when creating a query filtering rule to improve performance.

-  The **DEFAULT** syntax is used to reset an attribute, but it applies only to optional options.

Syntax
------

::

   ALTER BLOCK RULE block_name
       [ [ TO user_name@'host' ] | [ TO user_name ] | [ TO 'host' ] | [ TO DEFAULT ] ] |
       [ FOR UPDATE | SELECT | INSERT | DELETE | MERGE | DEFAULT ] |
       FILTER BY
       { SQL ( 'text' ) | TEMPLATE ( template_parameter = value ) }
       [ WITH ( { with_parameter = value }, [, ... ] ) ];
   ALTER BLOCK RULE block_name RENAME to new_block_name;

Parameter Description
---------------------

.. table:: **Table 1** ALTER BLOCK RULE parameters

   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                                                                                                                                                                                                  | Value Range                                                                                                                                                                                            |
   +=======================+==============================================================================================================================================================================================================================================================================+========================================================================================================================================================================================================+
   | block_name            | Name of the query filtering rule whose attributes need to be modified.                                                                                                                                                                                                       | A string, which must comply with the :ref:`naming convention <en-us_topic_0000001811634529__section1475018612353>`.                                                                                    |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TO                    | Specifies the object or client IP address that determines whether the filtering rule takes effect. The options are as follows:                                                                                                                                               | The query filtering rule takes effect only when the client IP address is **192.168.0.59** and the login username is **user1**.                                                                         |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       | -  **TO user_name@'host'**: The filtering rule takes effect only when both the username and client IP address are met.                                                                                                                                                       | ::                                                                                                                                                                                                     |
   |                       | -  **TO user_name**: The query filtering rule takes effect only when the username is met, regardless of the client IP address.                                                                                                                                               |                                                                                                                                                                                                        |
   |                       | -  **TO 'host'**: The query filtering rule takes effect only when the client IP address is met, regardless of the username.                                                                                                                                                  |    ALTER BLOCK RULE query_block TO user1@'192.168.0.59' FOR SELECT  FILTER BY SQL('test.test');                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              | The query filtering rule takes effect only when the login username is **user1**, regardless of the client IP address.                                                                                  |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              | ::                                                                                                                                                                                                     |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              |    ALTER BLOCK RULE query_block TO user1 FOR SELECT  FILTER BY SQL('test.test');                                                                                                                       |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              | The query filtering rule takes effect only when the client IP address is **192.168.0.59**, regardless of the login user.                                                                               |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              | ::                                                                                                                                                                                                     |
   |                       |                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                        |
   |                       |                                                                                                                                                                                                                                                                              |    ALTER BLOCK RULE query_block TO '192.168.0.59' FOR SELECT  FILTER BY SQL('test.test');                                                                                                              |
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
   | with_parameter        | Parameters for querying filter rule details.                                                                                                                                                                                                                                 | -  **application_name**: client name.                                                                                                                                                                  |
   |                       |                                                                                                                                                                                                                                                                              | -  **query_band**: load identifier.                                                                                                                                                                    |
   |                       |                                                                                                                                                                                                                                                                              | -  **table_num**: number of base tables scanned by the statement.                                                                                                                                      |
   |                       |                                                                                                                                                                                                                                                                              | -  **partition_num**: maximum number of partitions to be scanned by the operator.                                                                                                                      |
   |                       |                                                                                                                                                                                                                                                                              | -  **estimate_row**: estimated maximum number of table rows scanned by the operator.                                                                                                                   |
   |                       |                                                                                                                                                                                                                                                                              | -  **resource_pool**: name of the resource pool to be switched to.                                                                                                                                     |
   |                       |                                                                                                                                                                                                                                                                              | -  **max_active_num**: maximum number of concurrent statements corresponding to the rule.                                                                                                              |
   |                       |                                                                                                                                                                                                                                                                              | -  **is_warning**: whether an alarm or error is reported when a statement is intercepted. **on** indicates that an alarm is reported, and **off** (default value) indicates that an error is reported. |
   +-----------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

Create a query rule **query_block**.

::

   CREATE BLOCK RULE query_block FILTER BY SQL('update table_table set a=1');

Create a test user.

::

   DROP USER IF EXISTS user12;
   CREATE USER user12 PASSWORD '{Password}';

Modify the attributes of the query rule **query_block**.

::

   ALTER BLOCK RULE query_block
   TO user12@'192.168.0.59'
   FOR SELECT
   FILTER BY
   SQL('select * from table_name')
   WITH (application_name='gsql',
   query_band='test1',
   table_num='2',
   partition_num='3',
   estimate_row='1000',
   resource_pool='rsp1',
   max_active_num='3',
   is_warning='off'
   );

Query the modified filter rule through the **pg_get_blockruledef** view.

::

   SELECT * FROM pg_get_blockruledef('query_block');

|image2|

Rename the query rule **query_block**.

::

   ALTER BLOCK RULE query_block RENAME TO query_block_new;

Note that if only some parameters are reset when the rule is modified, the original values of other parameters are retained. In other words, only the specified parameters are modified, and all parameters are not overwritten or cleared.

::

   ALTER BLOCK RULE query_block_new
   TO user12@'192.168.0.59'
   FOR SELECT
   FILTER BY
   SQL('select * from table_name')
   WITH (
   query_band='test2',
   table_num='3');

   SELECT * FROM pg_get_blockruledef('query_block_new');

The command output shows that only the values of **query_band** and **table_num** are changed, and the values of other parameters are retained.

|image3|

Similarly, new filter criteria will not clear the previous filter criteria. For example:

::

   CREATE BLOCK RULE query_block_1 FILTER BY SQL('update table_table set a=1')
   WITH (application_name='gsql',
   query_band='test1');

   ALTER BLOCK RULE query_block_1 FILTER BY SQL('update table_table set a=1')
   WITH (
   table_num='2',
   partition_num='3',
   estimate_row='1000',
   resource_pool='rsp1',
   max_active_num='3',
   is_warning='off'
   );
   SELECT * FROM pg_get_blockruledef('query_block_1');

The command output shows that the original **application_name** and **query_band** parameters are retained.

|image4|

Helpful Links
-------------

:ref:`CREATE BLOCK RULE <dws_06_0292>`, :ref:`DROP BLOCK RULE <dws_06_0293>`

.. |image1| image:: /_static/images/en-us_image_0000002503156223.png
.. |image2| image:: /_static/images/en-us_image_0000002567713529.png
.. |image3| image:: /_static/images/en-us_image_0000002536954446.png
.. |image4| image:: /_static/images/en-us_image_0000002536834434.png
