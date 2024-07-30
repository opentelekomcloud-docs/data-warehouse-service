:original_name: dws_03_2112.html

.. _dws_03_2112:

Why the Tasks Executed by an Ordinary User Are Slower Than That Executed by the dbadmin User?
=============================================================================================

The execution speed of an ordinary user is slower than that of the dbadmin user in the following scenarios:

Scenario 1: Ordinary Users Are Subject to Resource Management.
--------------------------------------------------------------

Ordinary users are queuing: **waiting in queue/waiting in global queue/waiting in ccn queue**.

#. Ordinary users are **waiting in queue**/**waiting in global queue**.

   Ordinary users are queuing because the number of active statements exceeds the value of **max_active_statements**. Administrators do not need to wait in queue because they are not subject to resource management. You can change the value of **max_active_statements** on the management console.

   a. Log in to the GaussDB(DWS) management console.
   b. In the navigation tree on the left, choose **Clusters** > **Dedicated Clusters**.
   c. In the cluster list, find the target cluster and click the cluster name. The **Basic Information** page is displayed.
   d. Go to the **Parameter Modifications** page of the cluster, search for the **max_active_statements** parameter, change its value, and click **Save**.

#. It takes a long time for an ordinary user to wait in the ccn queue.

   When dynamic resource management is enabled (**enable_dynamic_workload**\ =\ **on**), if the concurrency is high and the available memory is small, ordinary users may wait in the ccn queue. Administrators are not subject to these controls. To mitigate this, one can either terminate certain statements or increase the memory parameter value. If the memory consumption on each DN remain low, consider disabling dynamic resource management (**enable_dynamic_workload**\ =\ **off**).

Scenario 2: The **OR** condition in the execution plan checks the statements executed by common users one by one. This consumes a lot of time.
----------------------------------------------------------------------------------------------------------------------------------------------

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

While the **OR** conditions of an ordinary user need to be checked one by one. If there are a large number of tables in the database, the execution time of the ordinary user is longer than that of the **dbadmin** user.

In this scenario, if the number of output result sets is small, you can set **set enable_hashjoin** and **enable_seqscan** to **off**, to use the index+nestloop plan.

Scenario 3: The resource pools allocated to ordinary users and administrators are different.
--------------------------------------------------------------------------------------------

Run the following command to check whether the resource pools corresponding to an ordinary user are the same as that of the administrator user. If they are different, check whether the tenant resources allocated to the two users are different.

::

   SELECt * FROM pg_user;

.. |image1| image:: /_static/images/en-us_image_0000001533637710.png
