:original_name: dws_06_0281.html

.. _dws_06_0281:

CREATE EXCEPT RULE
==================

Function
--------

**CREATE EXCEPT RULE** creates an exception rule set. You can specify specific rule thresholds and the operations (**abort** or **penalty**) to be performed after an exception rule is triggered.

In a distributed database, query tasks are executed by multiple nodes in parallel. Each node independently processes part of the data and summarizes the results. However, if any node encounters a performance bottleneck, the entire query will be slowed down even if other nodes complete the query quickly. In some extreme cases, a query statement may occupy a large number of resources, causing high node resource usage. As a result, compute or storage resources are insufficient, affecting services and even causing performance deterioration or even unavailability of the entire cluster.

When configuring exception rules in DWS, you can set resource restrictions (such as execution time, memory, and disk) for queries. When an exception rule is triggered during SQL statement execution, the execution is automatically terminated. This prevents abnormal queries from breaking down the cluster, ensuring stability and reducing the O&M workload of the database administrator. For example, if the execution time, disk space, or memory usage exceeds the threshold, the query is automatically terminated.

In DWS resource load management, exception rules take effect only after being bound to a resource pool, and then the user who executes a job is bound to a specified resource pool. If an exception rule is triggered when a user executes a job, the job will be terminated.

You can also set resource pools and exception rules on the management console. For details, see "DWS Resource Management Overview" in the *User Guide*.

Precautions
-----------

Exception rules within the same group work together using an AND condition. For example, if the execution time is set to 1,000 seconds (setting **elapsedtime** to **1000**) and the memory limit is set to 500 MB (setting **memsize** to **500**), the system terminates a job only when both conditions are met. Reaching just one threshold does not stop the job.

Syntax
------

::

   CREATE EXCEPT RULE except_rule_name
          WITH (
               | BLOCKTIME = VALUE,
               | CPUTIME = VALUE,
               | ELAPSEDTIME = VALUE,
               | CPUSKEWPERCENT = VALUE,
               | SPILLSIZE = VALUE,
               | BROADCASTSIZE = VALUE,
               | MEMSIZE = VALUE,
               | CPUAVGPERCENT = VALUE,
               | BANDWIDTH = VALUE,
               | ACTION = ['abort' | 'penalty']
               );

Parameter Description
---------------------

.. table:: **Table 1** CREATE EXCEPT RULE parameters

   +------------------+-----------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter        | Description                                                                 | Value Range                                                                                                                                          |
   +==================+=============================================================================+======================================================================================================================================================+
   | except_rule_name | Name of an exception rule set.                                              | A string, which must comply with :ref:`Identifier Naming Conventions <en-us_topic_0000001811634529__section1475018612353>`.                          |
   +------------------+-----------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | blocktime        | Maximum duration of job queue blocking, in seconds.                         | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**. Set the value to **-1**, which indicates that the value is not limited. |
   +------------------+-----------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | elapsedtime      | Maximum job execution duration, in seconds.                                 | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                                                         |
   +------------------+-----------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | allcputime       | Maximum CPU time used during job running. The unit is second.               | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                                                         |
   +------------------+-----------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cpuskewpercent   | Average CPU usage during job execution. The unit is percentage.             | Numeric type. The value can be **-1** or ranges from **1** to **100**.                                                                               |
   +------------------+-----------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cpuavgpercent    | CPU usage skew during job execution. The unit is percentage.                | Numeric type. The value can be **-1** or ranges from **1** to **100**.                                                                               |
   +------------------+-----------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | spillsize        | Maximum size of data spilled to disks during job execution. The unit is MB. | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                                                         |
   +------------------+-----------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | broadcastsize    | Maximum broadcast size of a job. The unit is MB.                            | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                                                         |
   +------------------+-----------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | memsize          | Maximum memory size used for job execution. Unit: MB.                       | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                                                         |
   +------------------+-----------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+
   | bandwidth        | Maximum bandwidth that can be used for job execution. Unit: MB.             | Numeric type. The value can be **-1** or ranges from **1** to **INT64_MAX**.                                                                         |
   +------------------+-----------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------+

Basic Example
-------------

Create an exception rule set (**except_rule1**) and set the **blocktime** to 3000 seconds and **spillsize** to 4000 MB. If a statement is blocked for more than 3000 seconds and the spill size exceeds 4000 MB, the statement will be terminated.

::

   DROP EXCEPT RULE IF EXISTS except_rule1;
   CREATE EXCEPT RULE except_rule1 WITH (blocktime=3000, spillsize=4000, action=abort);

Create an exception rule set (**except_rule2**) and set the **memsize** threshold to 5000 MB. The default operation following an exception is **abort**.

::

   DROP EXCEPT RULE IF EXISTS except_rule2;
   CREATE EXCEPT RULE except_rule2 WITH (memsize=3000);

Create a resource pool and bind it to exception rule set **except_rule1**.

::

   DROP resource pool IF EXISTS resource_pool_a1;
   CREATE resource pool resource_pool_a1 WITH (except_rule='except_rule1');

Example: Create an exception rule to terminate a job when the job runs for more than 10 seconds.
------------------------------------------------------------------------------------------------

In DWS resource load management, exception rules take effect only after being bound to a resource pool, and then the user who executes a job is bound to a specified resource pool. If an exception rule is triggered when a user executes a job, the job will be terminated. Perform the following operations to simulate the termination process.

#. Create a test table and import data to the table.

   a. Connect to the database as the system administrator and run the following SQL statements to create temporary table **src** to provide data sources for subsequent INSERT operations:

      ::

         DROP SCHEMA IF EXISTS test CASCADE;
         CREATE SCHEMA test;
         CREATE TABLE test.src AS SELECT 1;

   b. Run the following SQL statements to create table **test.test**:

      ::

         DROP TABLE IF EXISTS test.test;
         CREATE TABLE test.test(a int, b numeric(15,2)) WITH(orientation=column);

   c. Run the SQL statements to insert 20 million rows of data into the two tables. It takes about half a minute. Please wait.

      ::

         INSERT INTO test.test SELECT generate_series(1,20000000) % 1000,generate_series(1,20000000) FROM test.src;

#. Create exception rule set **except_rule4** and set **elapsedtime** to 10 seconds.

   ::

      DROP EXCEPT RULE IF EXISTS except_rule4;
      CREATE EXCEPT RULE except_rule4 WITH (elapsedtime=10);

#. Create resource pool **resource_pool_a4** and bind the exception rule to it.

   ::

      DROP resource pool IF EXISTS resource_pool_a4;
      CREATE resource pool resource_pool_a4 WITH (except_rule='except_rule4');

#. Create a job user and bind the user to **resource_pool_a4**. The password is configured as required.

   ::

      CREATE USER user4 RESOURCE POOL 'resource_pool_a4' PASSWORD '{password}';

#. Grant the permission to access table **test.test** to **user4**.

   ::

      GRANT USAGE ON SCHEMA test TO user4;
      GRANT SELECT ON TABLE test.test TO user4;

#. Connect to the database as **user4** and run the following SQL statement. Once the statement is executed for more than 10 seconds, it is terminated.

   ::

      SELECT * FROM test.test;

   |image1|

#. Check exception rule definitions in the **PG_EXCEPT_RULE** system catalog.

   ::

      SELECT * FROM PG_EXCEPT_RULE WHERE name = 'except_rule4';

   |image2|

Helpful Links
-------------

:ref:`ALTER EXCEPT RULE <dws_06_0280>`, :ref:`DROP EXCEPT RULE <dws_06_0282>`, :ref:`CREATE RESOURCE POOL <dws_06_0171>`

.. |image1| image:: /_static/images/en-us_image_0000002471311742.png
.. |image2| image:: /_static/images/en-us_image_0000002504037263.png
