:original_name: dws_01_72366.html

.. _dws_01_72366:

Configuring the Schema Storage Space of the DWS Database
========================================================

Functions
---------

Your cluster may run out of space if disk usage is not controlled, resulting in cluster exceptions and service interruption. Once disks are full, it takes long and huge efforts to recover workloads. Setting a database to read-only can reduce disk usage, but it also interrupts services. To solve this problem, DWS provides multi-dimensional storage management. You can limit the permanent space that can be occupied by a schema; and can limit the usage of permanent space, temporary space, and operator space for a user.

-  Schema level: Schema space management allows you to query database and schema space information in a cluster and modify the total schema space.
-  User level: User space management allows you to limit users' space usage, preventing task execution from being blocked due to insufficient storage space. When you create a resource pool in DWS, you can specify the space size to manage storage resources. Concurrently, only **PREM SPACE** is supported, which limits the space occupied by permanent tables (non-temporary tables) created by users.

Notes and Constraints
---------------------

-  The schema space management function is supported only by clusters of version 8.1.1 and later versions.
-  The space quota limits only common users but not database administrators. Therefore, when the used space is equal to the space limit, the actual used space may exceed the specified value.
-  Quota for a single DN = Total quota/Number of DNs. Therefore, the configured value may be slightly different from the displayed value.

Procedure
---------

#. Log in to the DWS console.
#. In the cluster list, click the name of the target cluster to go to the **Cluster Information** page.
#. In the navigation pane, choose **Resource Management**.
#. Click the **Schema Space Management** tab and select the database to be viewed from the drop-down list.
#. In the row where the scheme to be edited resides, click **Edit** and modify the space limit.
#. Click **OK**.
