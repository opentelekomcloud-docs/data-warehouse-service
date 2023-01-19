:original_name: dws_01_00084.html

.. _dws_01_00084:

DR Management
=============

.. _en-us_topic_0000001180320249__section4432124194612:

Starting a DR Task
------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, click **DR Tasks**.

#. Click **Start** in the **Operation** column of the target DR task.

#. In the dialog box that is displayed, click **OK**.

   The DR status will change to **Starting**. The process will take some time. After the task is started, the DR status will change to **Running**.

   .. note::

      -  You can start a DR task that is in the **Not started**/**Startup failed**/**Stopped** state.
      -  After you start the DR task, you cannot perform operations, including restoration, scale-out, upgrade, restart, and node replacement, on the production cluster and DR cluster. Backup is also not allowed on the DR cluster. Exercise caution when performing this operation.

Stopping the DR Task
--------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, click **DR Tasks**.

#. Click **Stop** in the **Operation** column of the target DR task.

#. In the dialog box that is displayed, click **OK**.

   The DR status will change to **Stopping**. The process will take some time. After the DR task is stopped, the status will change to **Stopped**.

   .. note::

      -  Only DR tasks in the **Running** or **Stop failed** state can be stopped.
      -  Data cannot be synchronized after a DR task is stopped.

Switching to the DR Cluster
---------------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, click **DR Tasks**.

#. Click **Switch to DR Cluster** in the **Operation** column of the target DR task.

#. In the dialog box that is displayed, click **OK**.

   The DR status will change to **DR switching**.

   After the switchover is successful, the DR status will change to the original status.

   .. note::

      -  You can perform a DR switchover when the DR task is in the **Running** or **Abnormal** state.
      -  During a switchover, the original production cluster is not available.
      -  RPO of DR switchover:

         -  Production cluster in the **Available** state: RPO = 0
         -  Production cluster in the **Unavailable** state: A zero RPO may not be achieved, but data can at least be restored to that of the latest successful DR synchronization (**Last DR Succeeded**). For details, see :ref:`Viewing DR Information <dws_01_00083>`.

Updating DR Configurations
--------------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, click **DR Tasks**.

#. In the DR list, click the DR name to go to the DR information page.

#. In the **DR Configurations** area, click **Modify**.

   |image1|

   .. note::

      -  Only DR tasks in the **Not started** or **Stopped** state can be modified.
      -  The new configuration takes effect after DR is restarted.

.. _en-us_topic_0000001180320249__section1631535174714:

Deleting DR Tasks
-----------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, click **DR Tasks**.

#. Click **Delete** in the **Operation** column of the target DR task.

#. In the dialog box that is displayed, click **OK**.

   The DR status will change to **Deleting**.

   .. note::

      -  You can delete a DR task when **DR Status** is **Creation failed**, **Not started**, **Startup failed**, **Stopped**, **Stop failed**, or **Abnormal**.
      -  Data cannot be synchronized after a DR task is deleted, and the deleted task cannot be restored.

.. |image1| image:: /_static/images/en-us_image_0000001134401018.png
