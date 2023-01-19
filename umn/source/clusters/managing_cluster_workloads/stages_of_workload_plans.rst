:original_name: dws_01_72363.html

.. _dws_01_72363:

Stages of Workload Plans
========================

Adding Stages for a Workload Plan
---------------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Go to the plan details page and click **Add** in the **Plan stage** area. On the **Add Stage** page, enter the stage name and configure the queue information. Confirm the configuration and click **OK**.

   .. important::

      -  You must stop the workload plan when adding a stage. Otherwise, the stage cannot be added.
      -  You can add a maximum of 48 stages for each plan.
      -  The switchover time of all phases in a plan cannot be the same.

   |image1|

Modifying Stages for a Workload Plan
------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Go to the plan details page and click **Modify** in the **Operation** column of the target plan stage.

   |image2|

#. On the **Modify Stage** page, you can modify information such as the switchover time and queue configurations.

   |image3|

Manually Switching Stages for a Workload Plan
---------------------------------------------

If a running plan needs to be switched to a stage in advance, you can manually do it.

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Go to the plan details page, click the **Handover** button in the plan overview area, and select the target stage.

   |image4|

Deleting Stages for a Workload Plan
-----------------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Go to the plan details page and click **Delete** in the **Operation** column of the target plan stage.

   |image5|

.. note::

   You must stop the workload plan when deleting a stage. Otherwise, the stage cannot be deleted.

.. |image1| image:: /_static/images/en-us_image_0000001180440421.png
.. |image2| image:: /_static/images/en-us_image_0000001180440423.png
.. |image3| image:: /_static/images/en-us_image_0000001134401054.png
.. |image4| image:: /_static/images/en-us_image_0000001134401050.png
.. |image5| image:: /_static/images/en-us_image_0000001134401052.png
