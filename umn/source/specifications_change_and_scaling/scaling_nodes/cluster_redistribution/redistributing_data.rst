:original_name: dws_01_8201.html

.. _dws_01_8201:

Redistributing Data
===================

Data redistribution, where data in existing nodes is evenly allocated to the new nodes after you scale out a cluster, is a time-consuming yet crucial task that accelerates service response.

By default, redistribution is automatically started after cluster scale-out. For enhanced reliability, disable the automatic redistribution function and manually start a redistribution task after the scale-out is successful. In this way, both scale-out and redistribution can be retried upon failures.

Currently, :ref:`offline redistribution <en-us_topic_0000001466594838__section858715017148>`, :ref:`online redistribution <en-us_topic_0000001466594838__section2706112181411>`, and :ref:`Offline Scheduling <en-us_topic_0000001466594838__section3137102712147>` are supported. The default mode is offline redistribution.

.. important::

   -  The cluster redistribution function is supported in 8.1.1.200 or later.
   -  This function can be manually enabled only when the cluster task information displays **To be redistributed** after scale-out.
   -  You can also select the redistribution mode when you configure cluster scale-out (see :ref:`Configure advanced parameters <en-us_topic_0000001466914106__li1283703664815>`).

.. _en-us_topic_0000001466594838__section858715017148:

Offline Redistribution
----------------------

**Precautions**

-  In offline redistribution mode, the database does not support DDL and DCL operations. Tables that are being redistributed support only simple DQL operations.
-  During table redistribution, a shared lock is added to tables. All insert, update, and delete operations as well as DDL operations on the tables are blocked for a long time, which may cause a lock wait timeout. Do not perform queries that take more than 20 minutes during the redistribution (the default time for applying for the write lock during redistribution is 20 minutes). Otherwise, data redistribution may fail due to lock wait timeout.

**Procedure**

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**. All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Scale Node** > **Redistribute**, as shown in the following figure.

   |image1|

#. On the **Redistribute** page that is displayed, keep the default **offline** redistribution mode and click **Next: Confirm** to submit the task.

   |image2|

   |image3|

   |image4|

.. _en-us_topic_0000001466594838__section2706112181411:

Online Redistribution
---------------------

**Precautions**

In online redistribution mode, the database supports partial DDL and DCL operations.

-  Local tables that are being redistributed support insert, delete and update operations and some DDL operations:

   -  **INSERT**, **DELETE**, **UPDATE**, **MERGE INTO**, **OVERWRITE**, **UPSERT**
   -  Join queries across node groups
   -  Local table renaming, schema modification, **DROP**, **TRUNCATE**, **TRUNCATE-PARTITION**

-  The following operations cannot be performed on tables that are being redistributed:

   -  Run **ALTER TABLE** statements (except for **TRUNCATE PARTITION**), including adding or deleting columns or partitions.
   -  Create, modify, or delete indexes.
   -  Run **VACUUM FULL** or **CLUSTER** on tables.
   -  Modify the sequence objects on which a column depends, including creating and modifying them. Typical statements are **CREATE** and **ALTER SEQUENCE ... OWNED BY**.
   -  During the redistribution of a table with more than 996 columns, **UPDATE** and **DELETE** statements cannot be executed. **SELECT** and **INSERT** statements are allowed.
   -  Database and tablespace objects cannot be created, deleted, or modified during redistribution.
   -  A partition swap can be performed only if the redistribution is complete for both of the tables to be swapped. The two tables belong to different node groups and do not allow partition swap if either of them is being redistributed.

**Procedure**

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Scale Node** > **Redistribute**, as shown in the following figure.

   |image5|

#. On the **Redistribute** page that is displayed, set **Advanced** to **Custom**, set the redistribution mode to **Online mode**, and click **Next: Confirm** to submit the task.

   |image6|

   |image7|

.. _en-us_topic_0000001466594838__section3137102712147:

Offline Scheduling
------------------

**Precautions**

Offline scheduling is similar to offline redistribution. In offline scheduling mode, tables are redistributed only within the configured time window, and redistribution is paused outside the time window.

.. important::

   -  Offline scheduling is supported only in 8.1.3 or later.
   -  If a cluster breaks down during redistribution, **Redistribution failed** will be displayed. If the cluster recovers, redistribution will automatically resume. To refresh the status, click **Redistribute** after the redistribution completes.

**Procedure**

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**. All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Scale Node** > **Redistribute**, as shown in the following figure.

   |image8|

#. On the **Redistribute** page that is displayed, set **Advanced** to **Custom**, set the redistribution mode to **Offline scheduling** and configure the scheduling window, and click **Next: Confirm** to submit the task.

   |image9|

   |image10|

   |image11|

.. note::

   In scheduled redistribution mode and out of the scheduled time window, a redistribution task is paused and the cluster status is **Redistribution paused**.

.. |image1| image:: /_static/images/en-us_image_0000001466914322.png
.. |image2| image:: /_static/images/en-us_image_0000001518033865.png
.. |image3| image:: /_static/images/en-us_image_0000001466914318.png
.. |image4| image:: /_static/images/en-us_image_0000001466754690.png
.. |image5| image:: /_static/images/en-us_image_0000001517355369.png
.. |image6| image:: /_static/images/en-us_image_0000001466754694.png
.. |image7| image:: /_static/images/en-us_image_0000001518033857.png
.. |image8| image:: /_static/images/en-us_image_0000001517754401.png
.. |image9| image:: /_static/images/en-us_image_0000001517913973.png
.. |image10| image:: /_static/images/en-us_image_0000001517754397.png
.. |image11| image:: /_static/images/en-us_image_0000001518033869.png
