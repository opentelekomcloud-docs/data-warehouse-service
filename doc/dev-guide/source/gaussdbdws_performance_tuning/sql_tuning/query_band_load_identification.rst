:original_name: dws_04_0493.html

.. _dws_04_0493:

Query Band Load Identification
==============================

Overview
--------

GaussDB(DWS) implements load identification and intra-queue priority control based on query_band. It provides more flexible load identification methods and identifies load queues based on job types, application names, and script names. Users can flexibly configure query_band identification queues based on service scenarios. In addition, priority control of job delivery in the queue is implemented. In the future, priority control of resources in the queue will be gradually implemented.

Administrators can configure the queue associated with query_band and estimate the memory limit based on service scenarios and job types to implement more flexible load control and resource management and control. If query_band is not configured for the service or the user does not associate query_band with an action, the queue associated with the user and the priority in the queue is used by default.

Load Behaviors Supported by query_band
--------------------------------------

query_band is a session-level GUC parameter. It is a job identifier of the character data type. Its value can be any string. However, for easier differentiation and configuration, query_band only identifies key-value pairs. For example:

::

   SET query_band='JobName=abc;AppName=test;UserName=user';

**JobName=abc**, **AppName=test**, and **UserName=user** are independent key-value pairs. Specifications of the query_band key-value pairs:

-  query_band is set in key-value pair mode, that is, 'key=value'. Multiple query_band key-value pairs can be set in a session. Multiple key-value pairs are separated by semicolons (;). The maximum length of both the **query_band** key-value pair and parameter value is 1024 characters.

-  The query_band key-value pair supports the following valid characters: digits 0 to 9, uppercase letters A to Z, lowercase letters a to z, '.', '-', '_', and '#'.

query_band is configured, and identifies load behaviors, using key-value pairs. The supported load behaviors are described in :ref:`Table 1 <en-us_topic_0000002052813834__en-us_topic_0000001811609577_table107259598574>`.

.. _en-us_topic_0000002052813834__en-us_topic_0000001811609577_table107259598574:

.. table:: **Table 1** Load behaviors supported by QUERY_BAND

   +--------------------------------+--------------------------------------------------------------------+--------------------------------------------+
   | Type                           | Behavior                                                           | Behavior Description                       |
   +================================+====================================================================+============================================+
   | Workload management (workload) | Resource pool (respool)                                            | query_band associated with a resource pool |
   +--------------------------------+--------------------------------------------------------------------+--------------------------------------------+
   | Workload management (workload) | Priority                                                           | Priority in the queue                      |
   +--------------------------------+--------------------------------------------------------------------+--------------------------------------------+
   | Order                          | Queue (respool)                                                    | **query_band** query order                 |
   |                                |                                                                    |                                            |
   |                                | Currently, this field is invalid and is used for future extension. |                                            |
   +--------------------------------+--------------------------------------------------------------------+--------------------------------------------+

The "Type" is used to classify load behaviors. Different load behaviors may belong to a same type. For example, both "Resource pool" and a "Priority" belong to "Workload management". The "Behavior" indicates a load behavior associated with a query_band key-value pair. The "Behavior description" describes a specific load behavior. The "Order" in the "Type" is used to indicate the priority of the query_band load behavior identification. When a session has multiple query_band key-value pairs, the query_band key-value pair with a smaller order value is preferentially used to identify a load behavior. Each query_band key-value pair can have multiple associated load behaviors, while one load behavior can only have one associated key-value pair. The query_band load behavior is described as follows:

-  Resource pool: query_band can be associated with resource pools. During job execution, if a resource pool is associated with query_band, the resource pool is used in preference. Otherwise, the resource pool associated with the user is used.

   -  When query_band is associated with a resource pool, an error is reported if the resource pool does not exist, and the association fails.
   -  When query_band is associated with a resource pool, the dependency between query_band and the resource pool is recorded.
   -  When a resource pool associated with query_band is deleted, a message is displayed indicating that the resource pool fails to be deleted because of the dependency between query_band and the resource pool.

-  Intra-queue priority: query_band can be associated with job priorities, including high, medium, and low. Rush is provided as a special priority (green channel). The default priority is medium. In practice, most jobs use the medium priority, low-priority jobs use the low priority, and privileged jobs use the high priority. It is not recommended that a large number of jobs use the high priority. The rush priority is used only in special scenarios and is not recommended in normal cases.

   The intra-queue priority is used to implement the queuing priority.

   -  In the static load management scenario, when the CN concurrency is insufficient, CN global queuing is triggered. The CN global queue is a priority queue.
   -  In the dynamic load management scenario, if the DN memory is insufficient, CCN global queuing is triggered. The CCN global queue is a priority queue.
   -  When the resource pool concurrency or memory is insufficient, resource pool queuing is triggered. The resource pool queue is a priority queue.

   The preceding priority queues comply with the following scheduling rules:

   -  Jobs with a higher priority are scheduled first.
   -  After all jobs with a high priority are scheduled, jobs with a low priority are scheduled.
   -  In dynamic load management scenarios, the CN global queue does not support the query_band priority.

-  Order: The identification order of query_bands can be configured. The default order value is **-1**. Except the default order value, there are no two query_bands with the same order value. The query_band order is verified when being configured. If there are query_bands with the same order value, the order values are recursively increased by 1 until there are no query_bands with the same order value.

   -  If a session has multiple query_band key-value pairs, the query_band key-value pair with a smaller order value is used for load identification.
   -  **0** is the smallest order value, and the default order value **-1** is the largest order value.
   -  If the query_bands are all of the same order value, the anterior query_band is used for load identification.
   -  For example, if in **set query_band='b=1;a=3;c=1'; b=1**, the order value of **b=1** is **-1**, **a=3** is **4**, **c=1** is **1**, **c=1** is used as the query_band for load identification. This design enables load administrators to adjust load scheduling.

Application and Configuration of query_band
-------------------------------------------

-  The pg_workload_action cross-database system catalog is used to store the query_band action and order. For details, see :ref:`PG_WORKLOAD_ACTION <dws_04_0632>`.
-  The default action and order are not stored in the **pg_workload_action** system catalog. If a non-default action is set for query_band, the default action is also displayed when actions are queried. The message <query_band information not found> is displayed when the action and order to be queried are the default query_band action.
-  The **gs_wlm_set_queryband_action** function sets the query_band sequence. The maximum length of the first parameter, that is, the query_band key value pair, is 63 characters. For the second parameter, it is case insensitive and multiple actions are separated by semicolons (;). **order** is the default parameter and its default value is **-1**. For details, see **gs_wlm_set_queryband_action** in "Resource Management Functions" in *SQL Syntax*.
-  The **gs_wlm_set_queryband_order** function sets the query_band sequence. The maximum length of the first parameter, that is, a query_band key value pair, is 63 characters. The value of query_band must be greater than or equal to **-1**. Except the default value **-1**, the value of query_band order must be unique. When setting the query_band order, if there are query_bands with the same order value, the original order value is increased by 1. For details, see **gs_wlm_set_queryband_order** in "Resource Management Functions" in *SQL Syntax*.
-  You can use the **gs_wlm_get_queryband_action** function to query the **query_band** action. For details, see **gs_wlm_set_queryband_action** in "Resource Management Functions" in *SQL Syntax*.
-  **pg_queryband_action** provides the system view for querying all query_band actions. For details, see :ref:`PG_QUERYBAND_ACTION <dws_04_0743>`.
-  The query_band priority is displayed as an integer in the load management view (:ref:`PG_SESSION_WLMSTAT <dws_04_0749>`). The mapping between numbers and priorities is as follows:

   -  0: not controlled by load management
   -  1: low
   -  2: medium
   -  4: high
   -  8: rush

-  Permission control: Except initial users, other users have the permission to set and query query_band only when they are authorized.

.. note::

   -  When all running jobs are canceled in batches or the maximum number of concurrent jobs in a queue is 1 and only one queue is running jobs, the CN may be triggered to automatically wake up jobs. As a result, jobs are not delivered by priority.

Examples
--------

#. Set the associated resource pool to **p1**, priority to **rush**, and order to **1** for query_band **JobName to abc**.

   ::

      SELECT * FROM gs_wlm_set_queryband_action('JobName=abc','respool=p1;priority=rush',1);
      gs_wlm_set_queryband_action
      -----------------------------
       t
      (1 row)

#. Change the associated resource pool to **p2** for query_band **JobName=abc**.

   ::

      SELECT * FROM gs_wlm_set_queryband_action('JobName=abc','respool=p2');
      gs_wlm_set_queryband_action
      -----------------------------
       t
      (1 row)

#. Change the priority to **high** for query_band **JobName=abc**.

   ::

      SELECT * FROM gs_wlm_set_queryband_action('JobName=abc','priority=high');
      gs_wlm_set_queryband_action
      -----------------------------
       t
      (1 row)

#. Change the order to **3** for query_band **JobName=abc**.

   ::

      SELECT * FROM gs_wlm_set_queryband_order('JobName=abc',3);
      gs_wlm_set_queryband_order
      -----------------------------
       t
      (1 row)

#. Query the load behaviors associated with query_band.

   ::

      SELECT * FROM pg_queryband_action;
          qband     | respool_id | respool | priority | qborder
      --------------+------------+---------+----------+---------
       JobName=abc  |      17119 | p2      | high     |       1
      (1 row)

#. In **query_band**, set the priority of **AppName=test** to **Low**, associate the user with the resource pool, and use the default sequence.

   ::

      SELECT * FROM gs_wlm_set_queryband_action('AppName=test','priority=low');
      gs_wlm_set_queryband_action
      -----------------------------
       t
      (1 row)

#. Query the load behaviors associated with query_band.

   ::

      SELECT * FROM pg_queryband_action;
          qband     | respool_id | respool | priority | qborder
      --------------+------------+---------+----------+---------
       AppName=test |          0 | NULL    | low      |      -1
       JobName=abc  |      16754 | p2      | high     |       3
      (2 rows)

#. In **query_band**, cancel all the workload behaviors associated with **JobName=abc** and set them to default behaviors.

   ::

      SELECT * FROM gs_wlm_set_queryband_action('JobName=abc','respool=null;priority=medium',-1);
      NOTICE:  The respool of query_band(JobName=abc) will be removed.
      NOTICE:  The priority of query_band(JobName=abc) will be removed.
       gs_wlm_set_queryband_action
      -----------------------------
       t
      (1 row)

#. Query the load behaviors associated with query_band.

   ::

      SELECT * FROM pg_queryband_action;
          qband     | respool_id | respool | priority | qborder
      --------------+------------+---------+----------+---------
       AppName=test |          0 | NULL    | low      |      -1
      (1 row)
