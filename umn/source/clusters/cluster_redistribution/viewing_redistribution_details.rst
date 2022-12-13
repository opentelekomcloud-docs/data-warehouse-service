:original_name: dws_01_8202.html

.. _dws_01_8202:

Viewing Redistribution Details
==============================

On the **View Redistribution Details** page, you can view the redistribution mode and progress of the current cluster. In offline scheduling mode, you can pause, resume, and modify redistribution, and perform retries upon failures.

.. note::

   -  This function is supported in 8.1.1.200 or later.

**Precautions**

-  This function is available only when the cluster task information is **Redistributing**, **Redistribution failed**, or **Redistribution paused**.

**Procedure**

#. Log in to the GaussDB(DWS) management console.
#. Choose **Clusters**. All clusters are displayed by default.
#. In the **Operation** column of the target cluster, choose **More** > **View Redistribution Details**, as shown in the following figure.

   -  In offline redistribution or online redistribution mode, only data progress and table progress are displayed.

      |image1|

   .. note::

      If redistribution fails, click **Retry**, as shown in the following figure.

      |image2|

      |image3|

.. |image1| image:: /_static/images/en-us_image_0000001161314776.png
.. |image2| image:: /_static/images/en-us_image_0000001207033065.png
.. |image3| image:: /_static/images/en-us_image_0000001161313142.png
