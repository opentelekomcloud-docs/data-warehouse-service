:original_name: dws_01_0008.html

.. _dws_01_0008:

Cluster Upgrade
===============

After you create a data warehouse cluster, the system automatically configures a random maintenance window for the cluster. Alternatively, you can customize a maintenance window as required. For details about how to view and configure the maintenance window, see :ref:`Configuring the Maintenance Window <en-us_topic_0000001180440157__section1583412504297>`.

The validity period of the maintenance window (maximum maintenance duration) is 4 hours. During this period, you can upgrade the cluster, install operating system patches, and harden the system. If no maintenance tasks are performed within the planned maintenance window, the cluster continues to run properly until the next maintenance window. GaussDB(DWS) will notify you of any cluster O&M operation by sending SMS messages. Exercise caution when performing operations on the cluster during the O&M period.

If the upgrade affects the current query requests or service running, contact technical support for emergency handling.

A cluster is charged by hour as long as it is in the **Available** state. Since the cluster is charged by hour, you will not see any difference in the bills if a faulty node or system upgrade causes a short interruption, for example, 15 minutes. If such events cause major system interruption, which is a very rare case, you will not be charged for those downtime hours.

Upgrading a Cluster
-------------------

You do not need to care about GaussDB(DWS) cluster patching or upgrading because GaussDB(DWS) will handle version upgrade automatically. After GaussDB(DWS) is upgraded, the service automatically upgrades the clusters to the latest version within the maintenance window. During the upgrade, the cluster is automatically restarted and cannot provide services for a short period of time. Therefore, you are advised to set a suitable time range when the number of connected users and the number of active tasks are small.

.. note::

   After the cluster is upgraded, it cannot be rolled back.

The following figure shows the cluster version.


.. figure:: /_static/images/en-us_image_0000001134400976.png
   :alt: **Figure 1** Version description

   **Figure 1** Version description

-  **Service patch upgrade**: The last digit of cluster version *X.X.X* is changed. For example, the cluster is upgraded from 1.1.0 to 1.1.1.

   -  Duration: The whole process will take less than 10 minutes.
   -  Impact on services: During this period, services will be interrupted for 1 to 3 minutes.

-  **Service upgrade**: The first two digits of cluster version *X.X.X* are changed. For example, the cluster is upgraded from 1.1.0 to 1.2.0.

   -  Duration: The whole process will take less than 30 minutes.
   -  Impact on services: During this period, the database cannot be accessed.

.. _en-us_topic_0000001180440157__section1583412504297:

Configuring the Maintenance Window
----------------------------------

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**.

#. In the cluster list, click the name of the target cluster. The **Basic Information** page is displayed.

   In the **Cluster Information** area, you can view the maintenance window.

#. Click **Configure** next to **Maintenance Window**.

#. In the dialog box that is displayed, configure the maintenance window.


   .. figure:: /_static/images/en-us_image_0000001134560768.png
      :alt: **Figure 2** Configuring the maintenance window

      **Figure 2** Configuring the maintenance window

#. Click **OK**.
