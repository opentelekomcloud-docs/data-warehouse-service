:original_name: dws_04_0395.html

.. _dws_04_0395:

Monitoring Memory Resources
===========================

Monitoring the Memory
---------------------

DWS allows you to view cluster memory usage on the monitoring panel (DMS). For details, see "Viewing DWS Cluster Monitoring Information on the Monitoring Panel (DMS)" in *Data Warehouse Service (DWS) User Guide*.

DWS provides a view for monitoring the memory usage of the entire cluster.

Query the **pgxc_total_memory_detail** view as a user with the **sysadmin** permission.

::

   SELECT * FROM pgxc_total_memory_detail;

If the following error message is returned during the query, enable the memory management function.

::

   SELECT * FROM pgxc_total_memory_detail;
   ERROR:  unsupported view for memory protection feature is disabled.
   CONTEXT:  PL/pgSQL function pgxc_total_memory_detail() line 12 at FOR over EXECUTE statement

You can set **max_process_memory** on the DWS console to enable memory management. The procedure is as follows:

#. Log in to the DWS console.
#. Choose **Dedicated Clusters** > **Clusters** in the navigation tree on the left.
#. In the cluster list, find the target cluster and click the cluster name to go to the cluster details page.
#. Click the **Parameter Modifications** tab and change the value of **max_process_memory** to a proper one. For details about the modification suggestions, see :ref:`max_process_memory <en-us_topic_0000001811610373__sadc1e0e8c1c246a4a6cad3967deebaad>`. After it is done, click **Save**.
#. In the **Modification Preview** dialog box, confirm the modifications and click **Save**. After the modification, restart the cluster for the modification to take effect.
