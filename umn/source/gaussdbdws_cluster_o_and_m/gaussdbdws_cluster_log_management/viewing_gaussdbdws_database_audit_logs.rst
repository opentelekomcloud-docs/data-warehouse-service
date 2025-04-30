:original_name: dws_01_0183.html

.. _dws_01_0183:

Viewing GaussDB(DWS) Database Audit Logs
========================================

Database audit logs can be set on the **Security Settings** page of the cluster. Security configurations can be modified only for clusters in the **Available** or **Unbalanced** state. Furthermore, the target cluster should not be undergoing any node additions, specification changes, configurations, upgrades, redistribution operations, or restarts.

Prerequisites
-------------

-  The audit function has been enabled by setting **audit_enabled**. The default value of **audit_enabled** is **ON**. To disable audit, set it to **OFF** by referring to :ref:`Modifying GUC Parameters of the GaussDB(DWS) Cluster <dws_01_0152>`.
-  The audit items have been configured. For how to enable audit items, see :ref:`Configuring the Database Audit Logs <en-us_topic_0000002167905872__section17422755918>`.
-  The database is running properly and a series of addition, modification, deletion, and query operations have been executed in the database. Otherwise, no audit result is generated.
-  The audit logs of each database node are recorded separately.
-  Only users with the **AUDITADMIN** permission can view audit records.

.. _en-us_topic_0000002167905872__section17422755918:

Configuring the Database Audit Logs
-----------------------------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters**.

#. In the cluster list, click the name of a cluster. Choose **Security**.

   By default, **Configuration Status** is **Synchronized**, which indicates that the latest database result is displayed.

#. In the **Audit Settings** area, set the audit items:

   .. note::

      The default audit log retention policy is space-first, which means audit logs will be automatically deleted when the size of audit logs on a single node exceeds 1 GB. This function prevents node faults or low performance caused by high disk space occupied by audit logs.

   :ref:`Table 1 <en-us_topic_0000002167905872__en-us_topic_0000001098656870_table48954270153356>` describes the detailed information about the audit items.

   .. _en-us_topic_0000002167905872__en-us_topic_0000001098656870_table48954270153356:

   .. table:: **Table 1** Audit items

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Audit Item                        | Description                                                                                                                                                                                             |
      +===================================+=========================================================================================================================================================================================================+
      | Unauthorized access               | Whether to record unauthorized operations. This parameter is disabled by default.                                                                                                                       |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DQL operations                    | **SELECT** operations can be selected.                                                                                                                                                                  |
      |                                   |                                                                                                                                                                                                         |
      |                                   | .. note::                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                         |
      |                                   |    Clusters of 8.1.1.100 and later versions support the **DQL operations** audit item.                                                                                                                  |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DML operations                    | Whether to record **INSERT**, **UPDATE**, and **DELETE** operations on tables. This parameter is disabled by default.                                                                                   |
      |                                   |                                                                                                                                                                                                         |
      |                                   | .. note::                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                         |
      |                                   |    8.1.1.100 and later versions support fine-grained splitting of audit items, and the **COPY** and **MERGE** options are added.                                                                        |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | DDL operations                    | Whether to record the **CREATE**, **DROP**, and **ALTER** operations of specified database objects. **DATABASE**, **SCHEMA**, and **USER** are selected by default.                                     |
      |                                   |                                                                                                                                                                                                         |
      |                                   | .. note::                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                         |
      |                                   |    8.1.1.100 and later versions support **TABLE**, **DATA SOURCE**, and **NODE GROUP** operations. These operations are enabled by default.                                                             |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Other operations                  | Whether to record other operations. Only the **TRANSACTION** and **CURSOR** operations are selected by default.                                                                                         |
      |                                   |                                                                                                                                                                                                         |
      |                                   | .. note::                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                         |
      |                                   |    -  8.1.1.100 and later versions support the **Other operations** audit item.                                                                                                                         |
      |                                   |    -  You are advised to select **TRANSACTION**. Otherwise, statements in a transaction will not be audited.                                                                                            |
      |                                   |    -  You are advised to select **CURSOR**. Otherwise, **SELECT** statements in a cursor will not be audited. The Data Studio client automatically encapsulates **SELECT** statements using **CURSOR**. |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   Except the audit items listed in :ref:`Table 1 <en-us_topic_0000002167905872__en-us_topic_0000001098656870_table48954270153356>`, key audit items in :ref:`Table 2 <en-us_topic_0000002167905872__en-us_topic_0000001098656870_table24262392153654>` are enabled by default on GaussDB(DWS).

   .. _en-us_topic_0000002167905872__en-us_topic_0000001098656870_table24262392153654:

   .. table:: **Table 2** Key audit items

      +-----------------+-----------------------------------------------------------+
      | Parameter       | Description                                               |
      +=================+===========================================================+
      | Key audit items | Records successful and failed logins and logout.          |
      +-----------------+-----------------------------------------------------------+
      |                 | Records database startup, stop, recovery, and switchover. |
      +-----------------+-----------------------------------------------------------+
      |                 | Records user locking and unlocking.                       |
      +-----------------+-----------------------------------------------------------+
      |                 | Records the grants and reclaims of user permissions.      |
      +-----------------+-----------------------------------------------------------+
      |                 | Records the audit function of the **SET** operation.      |
      +-----------------+-----------------------------------------------------------+

#. Enable or disable audit log dumps.

   For more information, see :ref:`Enabling Audit Log Dumps <en-us_topic_0000002203312033__en-us_topic_0000001145696613_section8182105814130>`.

#. Click **Apply**.

   If **Configuration Status** is **Applying**, the system is saving the settings.

   When the status changes to **Synchronized**, the configurations are saved and take effect.

   You can click |image1| to refresh the configuration information.

Viewing Database Audit Logs
---------------------------

Method 1: Audit logs will occupy disk space. To prevent excessive disk usage, GaussDB(DWS) supports audit log dumping. You can enable the **Log Dump** function to dump audit logs to OBS (you need to create an OBS bucket for storing audit logs first). For details about how to view the dumped logs, see :ref:`Enabling Audit Log Dumps <en-us_topic_0000002203312033__en-us_topic_0000001145696613_section8182105814130>`.

Method 2: Use the **Log** function of LTS to view or download the collected database audit logs. For details, see :ref:`Checking Cluster Logs <en-us_topic_0000002167905796__section1600157575>`.

Method 3: Database audit logs are stored in the database by default. After connecting to the cluster, you can use the **pg_query_audit** function to view the logs. For details, see :ref:`Using Functions to View Database Audit Logs <en-us_topic_0000002167905872__en-us_topic_0000001405788485_en-us_topic_0000001233761719_s0aec83296dc54e8f92966415aaaa3a6f>`.

.. _en-us_topic_0000002167905872__en-us_topic_0000001405788485_en-us_topic_0000001233761719_s0aec83296dc54e8f92966415aaaa3a6f:

Using Functions to View Database Audit Logs
-------------------------------------------

#. Use the SQL client tool to connect to the database cluster. For details, see :ref:`Connecting to a GaussDB(DWS) Cluster <dws_01_0131>`.

#. Use the **pg_query_audit** function to query the audit logs of the current CN. The syntax is as follows:

   ::

      pg_query_audit(timestamptz starttime,timestamptz endtime,audit_log)

   **starttime** and **endtime** indicate the start time and end time of the audit record, respectively. **audit_log** indicates the physical file path of the queried audit logs. If **audit_log** is not specified, the audit log information of the current instance is queried.

   For example, view the audit records of the current CN node in a specified period.

   ::

      SELECT * FROM pg_query_audit('2021-02-23 21:49:00','2021-02-23 21:50:00');

   The query result is as follows:

   ::

               begintime         |          endtime          | operation_type | audit_type | result |  username  | database | client_conninfo | object_name | command_text |                           detail_info                            | transaction_xid | query_id |  node_name   |               session_id                | local_port | remote_port
      ---------------------------+---------------------------+----------------+------------+--------+------------+----------+-----------------+-------------+-----------------+------------------------------------------------------------------+-----------------+----------+--------------+------------------------------+------------+-------------
       2021-02-23 21:49:57.76+08 | 2021-02-23 21:49:57.82+08 | login_logout   | user_login | ok     | dbadmin | gaussdb | gsql@[local]    | gaussdb    | login db     | login db(gaussdb) successfully, the current user is: dbadmin | 0               | 0        | coordinator1 | 140324035360512.667403397820909.coordinator1 | 27777      |

   This record indicates that user **dbadmin** logged in to the **gaussdb** database at 2021-02-23 21:49:57.82 (GMT+08:00). After the host specified by **log_hostname** is started and a client is connected to its IP address, the host name found by reverse DNS resolution is displayed following the at sign (@) in the value of **client_conninfo**.

#. Use the **pgxc_query_audit** function to query audit logs of all CNs. The syntax is as follows:

   ::

      pgxc_query_audit(timestamptz starttime,timestamptz endtime)

   For example, view the audit records of all CN nodes in a specified period.

   ::

      SELECT * FROM pgxc_query_audit('2021-02-23 22:05:00','2021-02-23 22:07:00') where audit_type = 'user_login' and username = 'user1';

   The query result is as follows:

   ::

               begintime          |          endtime           | operation_type | audit_type | result | username | database | client_conninfo | object_name | command_text |                         detail_info                        | transaction_xid | query_id |  node_name   |               session_id                     | local_port | remote_port
      ----------------------------+----------------------------+----------------+------------+--------+----------+----------+-----------------+-------------+--------------+------------------------------------------------------------+-----------------+----------+--------------+----------------------------------------------+------------+-------------
       2021-02-23 22:06:22.219+08 | 2021-02-23 22:06:22.271+08 | login_logout    | user_login | ok     | user1    | gaussdb  | gsql@[local]    | gaussdb     | login db     | login db(gaussdb) successfully, the current user is: user1 | 0               | 0        | coordinator2 | 140689577342720.667404382271356.coordinator  | 27782      |
       2021-02-23 22:05:51.697+08 | 2021-02-23 22:05:51.749+08 | login_logout    | user_login | ok     | user1    | gaussdb  | gsql@[local]    | gaussdb     | login db     | login db(gaussdb) successfully, the current user is: user1 | 0               | 0        | coordinator1 | 140525048424192.667404351749143.coordinator1 | 27777      |

   The query result shows the successful login records of **user1** in to CN1 and CN2.

#. Query the audit records of multiple objects.

   ::

      SET audit_object_name_format TO 'all';
      SELECT object_name,result,operation_type,command_text FROM pgxc_query_audit('2022-08-26 8:00:00','2022-08-26 22:55:00') where command_text like '%student%';

   The query result is as follows:

   ::

                                 object_name                            | result | operation_type |                                                                         command_text

      ------------------------------------------------------------------+--------+----------------+------------------------------------------------------------------------------------------------------------------
      --------------------------------------------
       student                                                          | ok     | ddl            | CREATE TABLE student(stuNo int, stuName TEXT);
       studentscore                                                     | ok     | ddl            | CREATE TABLE studentscore(stuNo int, stuscore int);
       ["public.student_view01","public.studentscore","public.student"] | ok     | ddl            | CREATE OR REPLACE VIEW student_view01 AS SELECT * FROM student t1 where t1.stuNo in (select stuNo from studentscore t2 where t1.stuNo = t2.stuNo);
       ["public.student_view01","public.student","public.studentscore"] | ok     | dml            | SELECT * FROM student_view01;

   In the **object_name** column, the table, view, and base table associated with the view are displayed.

.. |image1| image:: /_static/images/en-us_image_0000002167906244.png
