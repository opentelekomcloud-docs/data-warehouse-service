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

#. Click **Clusters**. All clusters are displayed by default.

#. In the **Task Information** column of a cluster, click **View Details**.

   |image1|

   -  The status, configurations, and progress of offline or online redistribution is displayed.

      |image2|

   .. note::

      If redistribution fails, click **Retry**, as shown in the following figure.

      |image3|

      |image4|

.. |image1| image:: /_static/images/en-us_image_0000001518033953.png
.. |image2| image:: /_static/images/en-us_image_0000001467074278.png
.. |image3| image:: /_static/images/en-us_image_0000001517754485.png
.. |image4| image:: /_static/images/en-us_image_0000001466754786.png
