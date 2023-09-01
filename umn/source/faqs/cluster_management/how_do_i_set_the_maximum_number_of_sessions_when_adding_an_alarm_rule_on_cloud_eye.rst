:original_name: dws_03_2122.html

.. _dws_03_2122:

How Do I Set the Maximum Number of Sessions When Adding an Alarm Rule on Cloud Eye?
===================================================================================

After connecting to a database, run the following SQL statement to check the maximum number of concurrent sessions globally:

::

   show max_active_statements;

Go to the Cloud Eye console and set the threshold to 70% to 80% of the obtained value. For example, if the value of **max_active_statements** is **80**, set the threshold to **56** (80 x 70%).

Procedure:

#. Go to the **Clusters** page on the GaussDB(DWS) management console.

#. Click **View Metric** in the **Operation** column of the target cluster to go to the Cloud Eye console.

#. Click |image1| in the upper left corner on the displayed page and click **Create Alarm Rule** of the target cluster.

   |image2|

#. Set **Method** to **Configure manually**, **Metric Name** to **Session Count**, **Alarm Policy** to **56**, and **Alarm Severity** to **Major**. Then click **Create**.

   |image3|

.. |image1| image:: /_static/images/en-us_image_0000001381609445.png
.. |image2| image:: /_static/images/en-us_image_0000001330488872.png
.. |image3| image:: /_static/images/en-us_image_0000001381889113.png
