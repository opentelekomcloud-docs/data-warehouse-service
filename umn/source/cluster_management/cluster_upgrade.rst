:original_name: dws_01_0008.html

.. _dws_01_0008:

Cluster Upgrade
===============

GaussDB(DWS) allows users to upgrade clusters on the console. For details, see :ref:`Upgrading a Cluster <en-us_topic_0000001924569420__section1121565214183>`.

During cluster O&M operations, GaussDB(DWS) will send SMS notifications to keep you informed. It is important to be careful when performing any operations on the cluster during this time.

During the upgrade, the cluster will be restarted and cannot provide services for a short period of time.

.. note::

   -  After a cluster is upgraded to 8.1.3 or later, it enters the observation period. During this period, you can check service status and roll back to the earlier version if necessary.
   -  Upgrading the cluster does not affect the original cluster data or specifications.

Upgrade Version Description
---------------------------

The following figure shows the cluster version.


.. figure:: /_static/images/en-us_image_0000001951849045.png
   :alt: **Figure 1** Version description

   **Figure 1** Version description

-  **Service patch upgrade**: The last digit of cluster version *X.X.X* is changed. For example, the cluster is upgraded from 1.1.0 to 1.1.1.

   -  Duration: The whole process will take less than 10 minutes.
   -  Impact on services: During this period, if the source version is upgraded to 8.1.3 or later, online patching is supported. During the patch upgrade, you do not have to stop services, but the services will be intermittently interrupted for seconds. If the destination version is earlier than 8.1.3, services will be interrupted for 1 to 3 minutes. Therefore, you are advised to perform this operation during off-peak hours.

-  **Service upgrade**: The first two digits of cluster version *X.X.X* are changed. For example, the cluster is upgraded from 1.1.0 to 1.2.0.

   -  Duration: The whole process will take less than 30 minutes.
   -  Impact on services: Online upgrade is supported for update to 8.1.1 or later. During the upgrade, you are not required to stop services, but services are intermittently interrupted for seconds. You are advised to perform the upgrade during off-peak hours.

-  Hot patch upgrade: A hot patch upgrade involves adding a one-digit version number (in the format of **0001-9999**) to the current cluster version.

   -  Duration: The upgrade of a single hot patch takes less than 10 minutes.
   -  Impact on services: The hot patch upgrade will not affect services, but there is a chance that the issues resolved by the current hot patch may come back after it is uninstalled.

.. _en-us_topic_0000001924569420__section1121565214183:

Upgrading a Cluster
-------------------

**Prerequisites**

Cluster 8.1.1 and later versions allow users to deliver cluster upgrade operations on the console.

**Procedure**

#. Log in to the GaussDB(DWS) console.
#. In the cluster list, click the name of a cluster.
#. In the navigation pane, choose **Upgrade Management**.
#. Choose either **Upgrade** or **Hot patch** from the **Type** drop-down list depending on the type of upgrade you want to perform.
#. On the **Upgrade Management** page, select a version from the **Target Version** drop-down list.

   .. note::

      -  Before the upgrade, it is crucial to verify if it meets the inspection conditions. Click **Immediate Inspection** to complete the inspection and proceed to the next step only if it passes. For more information, see :ref:`Viewing Inspection Results <dws_01_01755>`.
      -  DR cannot be established after a hot patch is installed in a cluster.

#. Click **Upgrade**. Click **OK** in the displayed dialog box.
#. Check whether the cluster is successfully upgraded.

   -  If the cluster version is 8.1.3 or later, the cluster enters the service observation period after the upgrade is complete. If you have verified your services, click **Submit** on the **Upgrade Management** page to complete the cluster upgrade. If you find your cluster performance affected by the upgrade, you can click **Rollback** to roll back the upgrade.

      .. note::

         -  If you are using a version of 8.1.3 or earlier, you will not be able to roll back or submit upgrade tasks until the cluster upgrade is finished.

         -  After an upgrade task is successfully delivered, if the upgrade task is not submitted, the **wlm** thread occupies the system storage space and affects the system performance.

   -  If the cluster upgrade fails, click **Rollback** to roll back to the original cluster version, or click **Retry** to deliver the upgrade again.
