:original_name: dws_04_0492.html

.. _dws_04_0492:

Table Diagnosis
===============

GaussDB(DWS) provides statistics and diagnostic tools for you to learn table status, including:

-  Skew Rate: monitors and analyzes uneven data distribution in a cluster, and displays information about the 50 largest tables whose skew rate is higher than 5%.
-  Dirty Page Rate: monitors and analyzes dirty pages in a cluster, and displays information about the 50 largest tables whose dirty page rate is higher than 50%.

Skew Rate
---------

Improper distribution columns can cause severe skew during operator computing or data spill to disk. The workloads will be unevenly distributed on DNs, resulting in high disk usage on individual DNs and affecting their performance. After identifying tables with a high skew rate and a relatively large size, you can reselect distribution columns for these tables to have their data redistributed.

**Procedure**

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be monitored.
#. In the **Operation** column of the target cluster, click **Monitoring Panel**.
#. In the navigation tree on the left, choose **Utilities** > **Table Diagnosis** and click the **Skew Rate** tab. The tables that meet the statistics collection conditions in the cluster are displayed.

Dirty Page Rate
---------------

DML operations on tables may generate dirty data, which unnecessarily occupies cluster storage. You can identify tables with a high dirty page rate and a relatively large size, and handle them accordingly..

**Procedure**

#. Log in to the GaussDB(DWS) console.
#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be monitored.
#. In the **Operation** column of the target cluster, click **Monitoring Panel**.
#. In the navigation tree on the left, choose **Utilities** > **Table Diagnosis** and click the **Dirty Page Rate** tab. The tables that meet the statistics collection conditions in the cluster are displayed.
