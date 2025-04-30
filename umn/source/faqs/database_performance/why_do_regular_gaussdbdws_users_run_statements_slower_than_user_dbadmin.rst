:original_name: dws_03_2112.html

.. _dws_03_2112:

Why Do Regular GaussDB(DWS) Users Run Statements Slower Than User dbadmin?
==========================================================================

There are three main scenarios where regular GaussDB(DWS) users experience slower execution compared to user **dbadmin**.

Scenario 1: Impact of Resource Management on Common Users
---------------------------------------------------------

Common users often find themselves waiting in various queues, such as the global queue or the CCN queue.

#. Reasons for common users waiting in queues or global queues

   The primary reason for this queueing is the high number of active statements exceeding the maximum value set for **max_active_statements**. Administrators are exempt from queueing as they are not subject to any control measures. To solve this problem, you can modify **max_active_statements** on the console.

   a. Log in to the GaussDB(DWS) management console.
   b. Choose **Dedicated Clusters** > **Clusters** in the navigation tree on the left.
   c. In the cluster list, find the target cluster and click the cluster name. The **Cluster Information** page is displayed.
   d. Click **Parameter Modifications**, search for **max_active_statements**, modify its value, and click **Save**. After confirming that the value is correct, click **Save**.

Scenario 2: Executing OR Conditions on Common User Statements Is Time-Consuming
-------------------------------------------------------------------------------

The **OR** conditions in the execution plans contain permission-related checks. This scenario usually occurs when the system view is used. For example, in the following SQL statement:

::

   SELECT distinct(dtp.table_name),
    ta.table_catalog,
    ta.table_schema,
    ta.table_name,
    ta.table_type
   from information_schema.tables ta left outer join DBA_TAB_PARTITIONS dtp
   on (dtp.schema = ta.table_schema and dtp.table_name = ta.table_name)
   where ta.table_schema = 'public';

Part of the execution plan is as follows:

|image1|

In the system view, the **OR** condition is used for permission check.

::

   pg_has_role(c.relowner, 'USAGE'::text) OR has_table_privilege(c.oid, 'SELECT, INSERT, UPDATE, DELETE, TRUNCATE, REFERENCES, TRIGGER'::text) OR has_any_column_privilege(c.oid, 'SELECT, INSERT, UPDATE, REFERENCES'::text)

**true** is always returned for **pg_has_role** of the **dbadmin** use. Therefore, the conditions after **OR** do not need to be checked.

However, the **OR** condition of common users needs to be checked one by one. If there are a large number of tables in the database, the execution time of common users is longer than that of **dbadmin**.

In this scenario, if the number of output result sets is small, you can set **set enable_hashjoin** and **set enable_seqscan** to **off**, to use the index+nestloop plan.

Scenario 3: Differences in Resource Pool Allocation for Common Users and Administrators
---------------------------------------------------------------------------------------

Check whether the resource pools corresponding to the user are the same. If they are different, check whether the tenant resources allocated to the two resource pools are different.

::

   SELECt * FROM pg_user;

.. |image1| image:: /_static/images/en-us_image_0000001533637710.png
