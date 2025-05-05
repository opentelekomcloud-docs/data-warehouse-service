:original_name: dws_01_72362.html

.. _dws_01_72362:

Managing Resource Management Plans
==================================

Overview
--------

The resource management plan is an advanced resource management feature provided by GaussDB(DWS). You can create a resource management plan, add multiple stages to the plan, and configure different queue resource ratios for the stages. After a plan is started, it automatically changes the resource configurations in different stages as scheduled. If you need to run services in different stages with different proportions of resources, you can create a resource management plan to automatically change resource configurations in different stages.

Creating a Resource Management Plan
-----------------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Clusters**. Click the name of a cluster.
#. Go to the **Basic Information** page and click the **Resource Management** tab in the navigation pane on the left.
#. Click to the **Resource Management Plans** tab and click **Add**.
#. Enter a plan name and click **OK**.

   .. important::

      -  Before creating a resource management plan, you must design and create a resource pool. For details, see :ref:`Creating a Resource Pool <dws_01_07233>`.
      -  You can create up to 10 resource management plans.

Starting or Stopping a Resource Management Plan
-----------------------------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Clusters**. Click the name of a cluster.
#. Go to the **Basic Information** page and click the **Resource Management** tab in the navigation pane on the left.
#. Switch to the **Resource Management Plans** tab page and click **Start**/**Stop**.
#. After confirming that the information is correct, click **OK** in the dialog box that is displayed to start or stop the plan.

   .. important::

      -  Only one plan can be started for each cluster.
      -  A plan must have at least two stages before it can be started.

Viewing the Execution Logs of a Resource Management Plan
--------------------------------------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Clusters**. Click the name of a cluster.
#. Go to the **Basic Information** page and click the **Resource Management** tab in the navigation pane on the left.
#. Click **Resource Management Plans**. In the plan execution logs area, click **View** to view the plan execution logs.

Deleting a Resource Management Plan
-----------------------------------

#. Log in to the GaussDB(DWS) console.
#. Choose **Clusters**. Click the name of a cluster.
#. Go to the **Basic Information** page and click the **Resource Management** tab in the navigation pane on the left.
#. Click **Resource Management Plans** and click **Delete** to delete the current resource management plan.

   .. important::

      You cannot delete a running resource management plan.
