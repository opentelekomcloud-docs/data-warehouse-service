:original_name: dws_01_72366.html

.. _dws_01_72366:

Workspace Management
====================

Overview
--------

Your cluster may run out of space if disk usage is not controlled, resulting in cluster exceptions and service interruption. Once disks are full, it takes long and huge efforts to recover workloads. Setting a database to read-only can reduce disk usage, but it also interrupts services. To solve this problem, GaussDB(DWS) provides multi-dimensional storage management. You can limit the permanent space that can be occupied by a schema; and can limit the usage of permanent space, temporary space, and operator space for a user.

-  Schema level: Schema space management allows you to query database and schema space information in a cluster and modify the total schema space.
-  User level: User space management allows you to limit users' space usage, preventing task execution from being blocked due to insufficient storage space. When you create a user in GaussDB(DWS), you can specify the space available to the user. The following types of storage space can be managed:

   -  Permanent space (**PREM SPACE**)

      Space occupied by permanent tables (non-temporary tables) created by users

   -  Temporary space (**TEMP SPACE**)

      Space occupied by temporary tables created by users

   -  Operator spill space (**SPILL SPACE**)

      During query execution, if the actual memory usage is greater than estimated, the query may be spilled to disks. The storage space occupied in this case is called operator spill space. You can control a user's operator spill space usage during query execution.

.. note::

   -  This feature is supported only in cluster version 8.1.1 or later.
   -  Currently, the GaussDB(DWS) management plane only supports schema space management.

Procedure
---------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. On the **Schema Space Manage** page, select a database.

#. In the row where the scheme to be edited resides, click **Edit** and modify the space limit.

   |image1|

#. Click **OK**.

   |image2|

   .. note::

      -  The space quota limits only common users but not database administrators. Therefore, when the used space is equal to the space limit, the actual used space may exceed the specified value.
      -  Quota of a single DN = Total quota/Number of DNs. Therefore, the configured value may slightly fluctuate with the displayed value.

.. |image1| image:: /_static/images/en-us_image_0000001518034053.png
.. |image2| image:: /_static/images/en-us_image_0000001466914514.png
