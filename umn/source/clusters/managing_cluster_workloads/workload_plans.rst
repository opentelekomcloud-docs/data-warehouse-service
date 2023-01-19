:original_name: dws_01_72362.html

.. _dws_01_72362:

Workload Plans
==============

Overview
--------

Workload plan is an advanced workload management feature provided by GaussDB(DWS). You can create a workload plan, add multiple stages to the plan, and configure different queue resource ratios for the stages. When a plan is started, it automatically switches the queue resource configurations in different stages. If a customer runs different services in different stages and these services occupy different proportions of resources, the workload plan function can help the customer implement automatic switchover of queue resource configurations in different stages.

Adding Workload Plans
---------------------

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Click the plus sign (+) next to **Workload Plan** to add a workload plan.

#. Enter a plan name and click **OK**.

   .. important::

      1. Before creating a workload plan, you must plan and create workload queues. For details, see :ref:`Adding Workload Queues <dws_01_07233>`.

      2. You can create a maximum of 10 workload plans.

   |image1|

Starting Workload Plans
-----------------------

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Enter the plan details page and click **Start** to start a workload plan.

   .. important::

      -  Only one plan can be started for each cluster.
      -  A plan must have at least two stages before it can be started.

   |image2|

Checking Execution Logs of Workload Plans
-----------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Go to the plan details page and view the switchover logs in the **Plan Execution Log** area.

   |image3|

   |image4|

Stopping Workload Plans
-----------------------

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Enter the plan details page and click **Stop** to stop a workload plan.

   |image5|

Deleting Workload Plans
-----------------------

#. Log in to the GaussDB(DWS) management console.

#. On the displayed **Clusters** page, click the name of the target cluster.

#. Switch to the **Workload Management** tab page.

#. Enter the plan details page and click **Delete** to delete a workload plan.

   .. important::

      You cannot delete a running workload plan.

   |image6|

.. |image1| image:: /_static/images/en-us_image_0000001180440229.png
.. |image2| image:: /_static/images/en-us_image_0000001180440233.png
.. |image3| image:: /_static/images/en-us_image_0000001180320299.png
.. |image4| image:: /_static/images/en-us_image_0000001134400866.png
.. |image5| image:: /_static/images/en-us_image_0000001180320293.png
.. |image6| image:: /_static/images/en-us_image_0000001134560650.png
