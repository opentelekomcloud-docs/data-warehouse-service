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

      -  The CPU usage limit can be configured only in 8.1.3 and later versions.

Adding a Resource Management Plan Stage
---------------------------------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Clusters**. Click the name of a cluster.

#. Go to the **Basic Information** page and click the **Resource Management** tab in the navigation pane on the left.

#. Go to the plan details page and click **Add** in the **Plan stage** area. On the **Add Stage** page, enter the stage name and configure the resource information. Confirm the configuration and click **OK**.

   .. important::

      -  Stages cannot be added to a running resource management plan.
      -  You can add a maximum of 48 stages for each plan.
      -  The switchover time of all phases in a plan cannot be the same.
      -  Configure the time, date, and month. Do not set an invalid date, for example, February 30.

   |image1|

Modifying a Resource Management Plan Stage
------------------------------------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Clusters**. Click the name of a cluster.

#. Go to the **Basic Information** page and click the **Resource Management** tab in the navigation pane on the left.

#. Switch to the **Resource Management Plans** tab page and click **Modify** in the **Operation** column of the plan stage.

#. Modify parameters, such as the stage changing time and resource configurations.

   |image2|

   .. note::

      Only clusters of the version 8.2.1 and later support the network bandwidth weight.

Manually Changing the Resource Management Plan Stage
----------------------------------------------------

If a running plan needs to be switched to a stage in advance, you can manually do it.

#. Log in to the GaussDB(DWS) console.

#. Choose **Clusters**. Click the name of a cluster.

#. Go to the **Basic Information** page and click the **Resource Management** tab in the navigation pane on the left.

#. Click **Resource Management Plans** and click the switch button in the plan overview area, and select the target stage.

   |image3|

Importing/Exporting Resource Management Plan Stages
---------------------------------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Clusters**. Click the name of a cluster.
#. Go to the **Basic Information** page and click the **Resource Management** tab in the navigation pane on the left.
#. Switch to the **Resource Management Plans** tab page. In the plan stages area, click **Import**/**Export** to import or export a resource management plan stage.

   .. important::

      -  Configurations cannot be imported to a running resource management plan.
      -  Ensure there is a resource pool before import.

Deleting a Resource Management Plan Stage
-----------------------------------------

#. Log in to the GaussDB(DWS) management console.
#. Choose **Clusters**. Click the name of a cluster.
#. Go to the **Basic Information** page and click the **Resource Management** tab in the navigation pane on the left.
#. Switch to the **Resource Management Plans** tab page and click **Delete** in the **Operation** column of the plan stage.

.. important::

   Stages in a running resource management plan cannot be deleted.

.. |image1| image:: /_static/images/en-us_image_0000002203312525.png
.. |image2| image:: /_static/images/en-us_image_0000002203426985.png
.. |image3| image:: /_static/images/en-us_image_0000002168066000.png
