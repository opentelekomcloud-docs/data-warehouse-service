:original_name: dws_01_72365.html

.. _dws_01_72365:

Importing and Exporting Workload Plans
======================================

You can commission a workload plan in the test environment and export the plan configurations to the production environment.

Exporting a Workload Plan
-------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Enter the plan details page and click **Export** to export a workload plan.

   |image1|

Importing a Workload Plan
-------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Enter the plan details page, click **Import**, and select and import the target configuration file to the workload plan.

   .. important::

      -  An ongoing workload plan cannot be imported.
      -  Before importing a workload plan, you need to create workload queues.

   |image2|

.. |image1| image:: /_static/images/en-us_image_0000001134560618.png
.. |image2| image:: /_static/images/en-us_image_0000001180320263.png
