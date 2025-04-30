:original_name: dws_01_1163.html

.. _dws_01_1163:

Viewing O&M Tasks
=================

#. Log in to the GaussDB(DWS) console.

#. Click the name of the target cluster.

#. In the navigation pane, choose **Intelligent O&M**.

#. Switch to the **O&M Status** area.

   |image1|

#. Click the arrow next to the specified O&M task name to view the task details.

   -  **O&M Task**: **Vacuum**
   -  Status: **Waiting**, **Running**, **Completed**, or **Failed**.
   -  **Progress**
   -  **Remaining Time Window**
   -  **Time Window** (Local Time)
   -  Tables being vacuumed
   -  Tables to be vacuumed
   -  Vacuumed tables
   -  Number of tables failed to be vacuumed
   -  Total number of vacuumed tables
   -  Amount of space freed up during the vacuum process
   -  Vacuum details list

   .. note::

      -  You can view up to 100 tables that are currently being vacuumed, have completed the vacuuming process, or have failed to be vacuumed.
      -  If the cluster is read-only, the INSERT statement cannot be executed for intelligent O&M tasks. There may be tasks remaining in the **Running** status. The **Running** status in this case is a historical status, and it indicates that the task is not completed within the specified time. If you manually pause the task and the task is not scheduled, the task may remain in the **Waiting** status. In this case, cancel the cluster read-only state and contact technical support to update the task status.

.. |image1| image:: /_static/images/en-us_image_0000002203426949.png
