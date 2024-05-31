:original_name: dws_01_0008.html

.. _dws_01_0008:

Cluster Upgrade
===============

By default, you do not need to manually upgrade a GaussDB(DWS) cluster. To upgrade a cluster on the console, see :ref:`Delivering a Cluster Upgrade Task on the Console <en-us_topic_0000001658895302__en-us_topic_0000001372520106_section1121565214183>`.

GaussDB(DWS) will notify you of any cluster O&M operation by sending SMS messages. Exercise caution when performing operations on the cluster during the O&M period.

If the upgrade affects the current query requests or service running, contact technical support for emergency handling.

Upgrading a Cluster
-------------------

By default, you do not need to care about GaussDB(DWS) cluster patching or upgrading because GaussDB(DWS) will handle version upgrade automatically. After GaussDB(DWS) is upgraded, it will automatically upgrade the cluster to the latest version. During the upgrade, the cluster will be restarted and cannot provide services for a short period of time.

.. note::

   -  After a cluster is upgraded to 8.1.3 or later, it enters the observation period. During this period, you can check service status and roll back to the earlier version if necessary.
   -  Upgrading the cluster does not affect the original cluster data or specifications.

The following figure shows the cluster version.


.. figure:: /_static/images/en-us_image_0000001711440304.png
   :alt: **Figure 1** Version description

   **Figure 1** Version description

-  **Service patch upgrade**: The last digit of cluster version *X.X.X* is changed. For example, the cluster is upgraded from 1.1.0 to 1.1.1.

   -  Duration: The whole process will take less than 10 minutes.
   -  Impact on services: During this period, if the source version is upgraded to 8.1.3 or later, online patching is supported. During the patch upgrade, you do not have to stop services, but the services will be intermittently interrupted for seconds. If the destination version is earlier than 8.1.3, services will be interrupted for 1 to 3 minutes. Therefore, you are advised to perform this operation during off-peak hours.

-  **Service upgrade**: The first two digits of cluster version *X.X.X* are changed. For example, the cluster is upgraded from 1.1.0 to 1.2.0.

   -  Duration: The whole process will take less than 30 minutes.
   -  Impact on services: Online upgrade is supported for update to 8.1.1 or later. During the upgrade, you are not required to stop services, but services are intermittently interrupted for seconds. You are advised to perform the upgrade during off-peak hours.

.. _en-us_topic_0000001658895302__en-us_topic_0000001372520106_section1121565214183:

Delivering a Cluster Upgrade Task on the Console
------------------------------------------------

**Prerequisites**

For clusters 8.1.1 or later, you need to deliver cluster upgrade operations on the console.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. In the cluster list, click the name of a cluster.

#. In the navigation pane, choose **Upgrade Management**.

#. On the **Upgrade Management** page, select a version from the **Target Version** drop-down list.

   |image1|

#. Click **Upgrade**. Click **OK** in the displayed dialog box.

   |image2|

#. Check whether the cluster is successfully upgraded.

   -  If the cluster version is 8.1.3 or later, the cluster enters the service observation period after the upgrade is complete. If you have verified your services, click **Submit** on the **Upgrade Management** page to complete the cluster upgrade. If you find your cluster performance affected by the upgrade, you can click **Rollback** to roll back the upgrade.

      .. note::

         -  In versions earlier than 8.1.3, there is neither rollback nor submission button after the upgrade is complete.
         -  If you do not submit the upgraded version, there will be a **wlm** thread which occupies the system storage space and affects the performance.

      |image3|

   -  If the cluster upgrade fails, click **Rollback** to roll back to the original cluster version, or click **Retry** to deliver the upgrade again.

      |image4|

.. |image1| image:: /_static/images/en-us_image_0000001711599804.png
.. |image2| image:: /_static/images/en-us_image_0000001759519205.png
.. |image3| image:: /_static/images/en-us_image_0000001759359337.png
.. |image4| image:: /_static/images/en-us_image_0000001711440308.png
