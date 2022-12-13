:original_name: dws_01_8201.html

.. _dws_01_8201:

Redistributing Data
===================

Data redistribution, where data in existing nodes is evenly allocated to the new nodes after you scale out a cluster, is a time-consuming yet crucial task that accelerates service response.

By default, redistribution is automatically started after cluster scale-out. For enhanced reliability, disable the automatic redistribution function and manually start a redistribution task after the scale-out is successful. In this way, both scale-out and redistribution can be retried upon failures.

Currently, :ref:`offline redistribution <en-us_topic_0000001134434168__section858715017148>` and :ref:`online redistribution <en-us_topic_0000001134434168__section2706112181411>` are supported. The default mode is offline redistribution.

.. important::

   -  The cluster redistribution function is supported in 8.1.1.200 or later.
   -  This function can be manually enabled only when the cluster task information displays **To be redistributed** after scale-out.
   -  You can also select the redistribution mode when you configure cluster scale-out (see :ref:`Configure advanced parameters <en-us_topic_0000001180320133__li1283703664815>`).

.. _en-us_topic_0000001134434168__section858715017148:

Offline Redistribution
----------------------

**Precautions**

-  In offline redistribution mode, the database does not support DDL and DCL operations. Tables that are being redistributed support only DQL operations.
-  During table redistribution, a shared lock is added to tables. All insert, update, and delete operations as well as DDL operations on the tables are blocked for a long time, which may cause a lock wait timeout. Do not perform queries that take longer than 20 minutes during the redistribution (the default time for applying for the write lock during redistribution is 20 minutes). Otherwise, data redistribution may fail due to lock wait timeout.

**Procedure**

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Redistribute**, as shown in the following figure.

   |image1|

#. On the **Redistribute** page that is displayed, keep the default **offline** redistribution mode and click **Next: Confirm** to submit the task.

   |image2|

   |image3|

   |image4|

.. _en-us_topic_0000001134434168__section2706112181411:

Online Redistribution
---------------------

**Precautions**

In online redistribution mode, the database supports partial DDL and DCL operations.

-  Local tables that are being redistributed support insert, delete and update operations and some DDL operations:

   -  **INSERT**, **DELETE**, **UPDATE**, **MERGE INTO**, **OVERWRITE**, **UPSERT**
   -  Join queries across node groups
   -  **DROP**, **TRUNCATE**, **TRUNCATE-PARTITION**

-  The following operations cannot be performed on tables that are being redistributed:

   -  Run **ALTER TABLE** statements (except for **TRUNCATE PARTITION**), including adding or deleting columns, renaming tables, and modifying schemas.
   -  Create, modify, or delete indexes.
   -  Run **VACUUM FULL** or **CLUSTER** on tables.
   -  Modify the sequence objects on which a column depends, including creating and modifying them. Typical statements are **CREATE** and **ALTER SEQUENCE ... OWNED BY**.

**Procedure**

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. All clusters are displayed by default.

#. In the **Operation** column of the target cluster, choose **More** > **Redistribute**, as shown in the following figure.

   |image5|

#. On the **Redistribute** page that is displayed, set **Advanced** to **Custom**, set the redistribution mode to **Online**, and click **Next: Confirm** to submit the task.

   |image6|

   |image7|

   |image8|

.. |image1| image:: /_static/images/en-us_image_0000001163636428.png
.. |image2| image:: /_static/images/en-us_image_0000001204891199.png
.. |image3| image:: /_static/images/en-us_image_0000001204878747.png
.. |image4| image:: /_static/images/en-us_image_0000001204760551.png
.. |image5| image:: /_static/images/en-us_image_0000001161787646.png
.. |image6| image:: /_static/images/en-us_image_0000001207147903.png
.. |image7| image:: /_static/images/en-us_image_0000001161788458.png
.. |image8| image:: /_static/images/en-us_image_0000001159800492.png
