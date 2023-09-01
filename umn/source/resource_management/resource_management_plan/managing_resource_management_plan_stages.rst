:original_name: dws_01_72363.html

.. _dws_01_72363:

Managing Resource Management Plan Stages
========================================

Prerequisites
-------------

The following conditions must be met when you add or modify a resource management plan:

-  The total CPU share of all resource pools does not exceed 99%.
-  The total CPU limit of all resource pools does not exceed 100%.

   .. note::

      -  The CPU limit can be configured only in 8.1.3 and later versions.

Adding a Resource Management Plan Stage
---------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. Go to the plan details page and click **Add** in the **Plan stage** area. On the **Add Stage** page, enter the stage name and configure the resource information. Confirm the configuration and click **OK**.

   .. important::

      -  Stages cannot be added to a running resource management plan.
      -  You can add a maximum of 48 stages for each plan.
      -  The switchover time of all phases in a plan cannot be the same.
      -  Configure the time, date, and month. Do not set an invalid date, for example, February 30.

   |image1|

   |image2|

Modifying a Resource Management Plan Stage
------------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. Go to the plan details page and click **Modify** in the **Operation** column of the target plan stage.

   |image3|

#. Modify parameters, such as the stage changing time and resource configurations.

   |image4|

Manually Changing the Resource Management Plan Stage
----------------------------------------------------

If a running plan needs to be switched to a stage in advance, you can manually do it.

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. Go to the plan details page, click the **Switch over** button in the plan overview area, and select a stage.

   |image5|

Deleting a Resource Management Plan Stage
-----------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. Go to the plan details page and click **Delete** in the **Operation** column of the target plan stage.

   |image6|

.. note::

   Stages in a running resource management plan cannot be deleted.

.. |image1| image:: /_static/images/en-us_image_0000001466754670.png
.. |image2| image:: /_static/images/en-us_image_0000001466914302.png
.. |image3| image:: /_static/images/en-us_image_0000001518033837.png
.. |image4| image:: /_static/images/en-us_image_0000001517754369.png
.. |image5| image:: /_static/images/en-us_image_0000001517355341.png
.. |image6| image:: /_static/images/en-us_image_0000001517913949.png
