:original_name: dws_01_7165.html

.. _dws_01_7165:

Changing a Cluster Name
=======================

Scenario
--------

After a GaussDB(DWS) cluster is created, you can change the cluster name as required.

After the cluster name is changed, the names of all nodes in the current cluster are changed accordingly.

.. note::

   -  If the cluster name cannot be changed on the console, contact technical support to upgrade the console.
   -  If the cluster name fails to be changed, the cluster functions are not affect. You can contact technical support rectify the fault.

Constraints and Limitations
---------------------------

If the cluster is in the **Unavailable** status or is performing other tasks, the cluster name cannot be changed. You can change the cluster name only after the cluster status changes to **Available** or the running tasks are complete.

Procedure
---------

**Method 1**:

#. Log in to the GaussDB(DWS) console.

#. In the cluster list, click the modification icon next to a cluster name to modify the cluster.


   .. figure:: /_static/images/en-us_image_0000001924569912.png
      :alt: **Figure 1** Changing the name of a cluster in the cluster list

      **Figure 1** Changing the name of a cluster in the cluster list

#. In the displayed dialog box, enter a new cluster name.

#. Confirm the information and click **OK**.

**Method 2:**

#. Log in to the GaussDB(DWS) console.

#. In the cluster list, click the name of a cluster.

#. On the displayed **Cluster Details** page, click the modification icon next to the cluster name in the **Basic Information** area.


   .. figure:: /_static/images/en-us_image_0000001924729280.png
      :alt: **Figure 2** Changing the cluster name on the cluster details page

      **Figure 2** Changing the cluster name on the cluster details page

#. After confirming that the information is correct, click **OK** to deliver the cluster modification task. After the task is complete, the cluster name is changed successfully.
