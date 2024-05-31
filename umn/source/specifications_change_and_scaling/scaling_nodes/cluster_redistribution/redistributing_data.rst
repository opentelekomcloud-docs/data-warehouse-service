:original_name: dws_01_8201.html

.. _dws_01_8201:

Redistributing Data
===================

Data redistribution, where data in existing nodes is evenly allocated to the new nodes after you scale out a cluster, is a time-consuming yet crucial task that accelerates service response.

By default, redistribution is automatically started after cluster scale-out. For enhanced reliability, disable the automatic redistribution function and manually start a redistribution task after the scale-out is successful. In this way, both scale-out and redistribution can be retried upon failures.

Currently, :ref:`offline redistribution <en-us_topic_0000001659054558__en-us_topic_0000001422959169_section858715017148>` and :ref:`online redistribution <en-us_topic_0000001659054558__en-us_topic_0000001422959169_section2706112181411>` are supported. The default mode is offline redistribution.

Before redistribution starts or when redistribution is paused, you can set redistribution priorities for the tables that have not been redistributed by schema or table.

.. important::

   -  The cluster redistribution function is supported in 8.1.1.200 or later cluster versions.
   -  This function can be manually enabled only when the cluster task information displays **To be redistributed** after scale-out.
   -  You can also select the redistribution mode when you configure cluster scale-out (see :ref:`Configure advanced parameters <en-us_topic_0000001707254605__en-us_topic_0000001372839430_li85162515474>`).
   -  Redistribution queues are sorted based on the relpage size of tables. To ensure that the relpage size is correct, you are advised to perform the **ANALYZE** operation on the tables to be redistributed.

.. _en-us_topic_0000001659054558__en-us_topic_0000001422959169_section858715017148:

Offline Redistribution
----------------------

**Precautions**

-  In offline redistribution mode, the database does not support DDL and DCL operations. Tables that are being redistributed support only simple DQL operations.
-  During table redistribution, a shared lock is added to tables. All insert, update, and delete operations as well as DDL operations on the tables are blocked for a long time, which may cause a lock wait timeout. Do not perform queries that take more than 20 minutes during the redistribution (the default time for applying for the write lock during redistribution is 20 minutes). Otherwise, data redistribution may fail due to lock wait timeout.

**Procedure**

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters** > **Dedicated Cluster**. All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Scale Node** > **Redistribute**, as shown in the following figure.

   The **Redistribution** page is displayed.

   |image1|

#. On the **Redistribute** page that is displayed, keep the default **offline** redistribution mode and click **Next: Confirm** to submit the task.

   |image2|

.. _en-us_topic_0000001659054558__en-us_topic_0000001422959169_section2706112181411:

Online Redistribution
---------------------

**Precautions**

In online redistribution mode, the database supports partial DDL and DCL operations.

-  Tables that are being redistributed support insert, delete and update operations and some DDL operations. The following functions are supported:

   -  **INSERT**, **DELETE**, **UPDATE**, **MERGE INTO**, **OVERWRITE**, **UPSERT**
   -  Join queries across node groups
   -  Local table renaming, schema modification, **DROP**, **TRUNCATE**, **TRUNCATE-PARTITION**

-  The following operations cannot be performed on tables that are being redistributed:

   -  Run **ALTER TABLE** statements (except for **TRUNCATE PARTITION**), including adding or deleting columns or partitions.
   -  Create, modify, or delete indexes.
   -  **VACUUM FULL** or **CLUSTER** can not be executed on tables during table redistribution.
   -  Modify the sequence objects on which a column depends, including creating and modifying them. Typical statements are **CREATE** and **ALTER SEQUENCE ...** **OWNED BY**.
   -  During the redistribution of a table with more than 996 columns, **UPDATE** and **DELETE** statements cannot be executed. **SELECT** and **INSERT** statements are allowed.
   -  Database and tablespace objects cannot be created, deleted, or modified during redistribution.
   -  A partition swap can be performed only if the redistribution is complete for both of the tables to be swapped. The two tables belong to different node groups and do not allow partition swap if either of them is being redistributed.

**Procedure**

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters** > **Dedicated Clusters**. All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Scale Node** > **Redistribute**, as shown in the following figure.

   |image3|

#. On the **Redistribute** page that is displayed, set **Advanced** to **Custom**, set the redistribution mode to **Online mode**, and click **Next: Confirm** to submit the task.

.. |image1| image:: /_static/images/en-us_image_0000001759358109.png
.. |image2| image:: /_static/images/en-us_image_0000001711598576.png
.. |image3| image:: /_static/images/en-us_image_0000001759358113.png
