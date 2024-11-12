:original_name: dws_01_01755.html

.. _dws_01_01755:

Viewing Inspection Results
==========================

Context
-------

GaussDB(DWS) allows you to inspect the cluster before making any changes like scaling, changing specifications, or upgrading. Simply click **Inspect** on the relevant page, and the system will check if the cluster's health status and metrics meet the requirements for the change. Once the inspection is passed, you can proceed with the change. If the inspection fails, you can view the inspection details page to see which items did not pass the inspection. From there, you can handle the inspection items based on the details provided.

.. note::

   This feature is supported only in cluster version 8.1.1 or later.

Precautions
-----------

-  The inspection plug-in 8.3.1.100 or later has been installed in the cluster.
-  The inspection result is valid for 24 hours, during which you can make the change operation. Once the 24-hour validity period expires, you will need to perform the inspection again.
-  If the cluster has not been inspected within 24 hours before the change, inspect it before making any changes like scaling, changing specifications, or upgrading. Ensure that the inspection is passed before proceeding with the change.

Viewing Inspection Details
--------------------------

#. Log in to the GaussDB(DWS) console.

#. In the cluster list, click the name of the target cluster.

#. On the cluster details page, click **Inspection Management**.

#. Click the drop-down button next to its name to check the inspection status, execution progress, inspection result, and pass rate. For more details about these inspection items, click **View Details** in the task's row.


   .. figure:: /_static/images/en-us_image_0000001951848641.png
      :alt: **Figure 1** Viewing inspection details

      **Figure 1** Viewing inspection details

   .. note::

      After creating an inspection task on the configuration change page, you can keep track of its progress and view details. You can even stop the inspection from that same page.

Stopping an Inspection Task
---------------------------

#. Log in to the GaussDB(DWS) console.
#. In the cluster list, click the name of the target cluster.
#. On the cluster details page, click **Inspection Management**.
#. Locate the row that contains the inspection task and click **Stop** in the **Operation** column to stop the inspection task.
