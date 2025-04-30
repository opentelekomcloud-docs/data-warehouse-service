:original_name: dws_01_8202.html

.. _dws_01_8202:

Viewing Redistribution Details
==============================

On the **View Redistribution Details** page, you can check the monitoring information, including the redistribution mode, redistribution progress, and table redistribution details of the current cluster. You can pause and resume redistribution, set the redistribution priority, and change the number of concurrent redistribution tasks.

.. note::

   The function of viewing redistribution details is supported by 8.1.1.200 and later cluster versions. Details about the data table redistribution progress are supported only by 8.2.1 and later cluster versions.

**Precautions**

You can check redistribution details only if the cluster is being redistributed, failed to be redistributed, or is suspended. There may be a delay in the statistics update.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters**. All clusters are displayed by default.

#. In the **Task Information** column of a cluster, click **View Details**.

#. Check the redistribution status, configuration, progress, and redistribution details of all the tables in a specified database. Specify a database that and can be searched by table redistribution status and table name. If all the tables in a database have completed redistribution, no data will be displayed for the database.

#. When redistribution is paused, you can set the redistribution priority (in schema or table dimension), and redistribution will be performed based on the configured redistribution sequence. You can also set the redistribution priority before the redistribution starts.

   |image1|

#. The number of concurrent redistribution tasks can be adjusted during redistribution.

   .. note::

      Cluster 8.1.0 and earlier versions do not support dynamic adjustment. To change redistribution concurrency, suspend redistribution first.

   |image2|

#. Check the redistribution progress. After the redistribution is complete, the amount of completed data, amount of remaining data, number of completed tables, number of remaining tables, and average rate during redistribution are displayed.

   |image3|

.. |image1| image:: /_static/images/en-us_image_0000002167906420.png
.. |image2| image:: /_static/images/en-us_image_0000002203427097.png
.. |image3| image:: /_static/images/en-us_image_0000002203312645.png
