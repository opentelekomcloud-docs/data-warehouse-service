:original_name: dws_01_72362.html

.. _dws_01_72362:

Managing Resource Management Plans
==================================

Overview
--------

The resource management plan is an advanced resource management feature provided by GaussDB(DWS). You can create a resource management plan, add multiple stages to the plan, and configure different queue resource ratios for the stages. After a plan is started, it automatically changes the resource configurations in different stages as scheduled. If you need to run services in different stages with different proportions of resources, you can create a resource management plan to automatically change resource configurations in different stages.

Creating a Resource Management Plan
-----------------------------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. Click to the **Resource Management Plans** tab and click **Add**.

#. Enter a plan name and click **OK**.

   .. important::

      1. Before creating a resource management plan, you must design and create a resource pool. For details, see :ref:`Creating a Resource Pool <dws_01_07233>`.

      2. You can create up to 10 resource management plans.

   |image1|

Starting a Resource Management Plan
-----------------------------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. Enter the plan details page and click **Start** to start a resource management plan.

   .. important::

      -  Only one plan can be started for each cluster.
      -  A plan must have at least two stages before it can be started.

   |image2|

Viewing the Execution Logs of a Resource Management Plan
--------------------------------------------------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. Go to the plan details page and view the switchover logs in the **Plan Execution Log** area.

   |image3|

   |image4|

Stopping a Resource Management Plan
-----------------------------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. Enter the plan details page and click **Stop** to stop a resource management plan.

   |image5|

Deleting a Resource Management Plan
-----------------------------------

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. Click the name of a cluster.

#. Choose **Resource Management Configurations**.

#. Enter the plan details page and click **Delete** to delete a resource management plan.

   .. important::

      You cannot delete a running resource management plan.

   |image6|

.. |image1| image:: /_static/images/en-us_image_0000001517754581.png
.. |image2| image:: /_static/images/en-us_image_0000001466914506.png
.. |image3| image:: /_static/images/en-us_image_0000001517754573.png
.. |image4| image:: /_static/images/en-us_image_0000001466754882.png
.. |image5| image:: /_static/images/en-us_image_0000001517914153.png
.. |image6| image:: /_static/images/en-us_image_0000001466595230.png
