:original_name: dws_01_00084.html

.. _dws_01_00084:

DR Management
=============

.. _en-us_topic_0000001707293893__en-us_topic_0000001423159725_section4432124194612:

Starting a DR Task
------------------

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane on the left, choose **DR Tasks**.

#. Click **Start** in the **Operation** column of the target DR task.

   |image1|

#. In the dialog box that is displayed, click **OK**.

   The DR status will change to **Starting**. The process will take some time. After the task is started, the DR status will change to **Running**.

   .. note::

      -  You can start a DR task that is in the **Not started**/**Startup failed**/**Stopped** state.
      -  After you start the DR task, you cannot perform operations, including restoration, scale-out, upgrade, restart, node replacement, and password update, on the production cluster or DR cluster. Backup is also not allowed on the DR cluster. Exercise caution when performing this operation.

Stopping the DR Task
--------------------

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane on the left, choose **DR Tasks**.

#. Click **Stop** in the **Operation** column of the target DR task.

   |image2|

#. In the dialog box that is displayed, click **OK**.

   The DR status will change to **Stopping**. The process will take some time. After the DR task is stopped, the status will change to **Stopped**.

   .. note::

      -  Only DR tasks in the **Running** or **Stop failed** state can be stopped.
      -  Data cannot be synchronized after a DR task is stopped.

Switching to the DR Cluster
---------------------------

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane on the left, choose **DR Tasks**.

#. Click **Switch to DR Cluster** in the **Operation** column of the target DR task.

   |image3|

#. In the dialog box that is displayed, click **OK**.

   The DR status will change to **DR switching**.

   After the switchover is successful, the DR status will change to the original status.

   .. note::

      -  To perform a switchover when the DR cluster is running properly, click **Switch to DR Cluster**.
      -  You can perform a DR switchover when the DR task is in the **Running** state.
      -  During a switchover, the original production cluster is not available.
      -  Recovery Point Object (RPO) refers to the point in time to which a system and data must be restored after a disaster occurs. Its value varies by cluster status.

         -  Production cluster in the **Available** state: RPO = 0
         -  Production cluster in the **Unavailable** state: A zero RPO may not be achieved, but data can at least be restored to that of the latest successful DR synchronization (**Last DR Succeeded**). For details, see :ref:`Viewing DR Information <dws_01_00083>`.

Exception Switchover
--------------------

**Scenario**

The production cluster is unavailable, the DR cluster is normal, and the DR status is **Abnormal**.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane on the left, choose **DR Tasks**.

#. Choose **More** > **Exception Switchover** in the **Operation** column of the target DR task.

   |image4|

#. In the dialog box that is displayed, click **OK**.

   The **Status** will change to **Switchover in progress**.

   After the switchover is successful, the DR status will change to the original status. In this procedure, the DR status will change back to **Abnormal**.

   .. note::

      -  To perform a switchover when the DR cluster is abnormal or the production cluster is faulty, click **Exception Switchover**.
      -  DR exception switchover is supported only by clusters of version 8.1.2 or later.
      -  Before a switchover, check the latest synchronization time in the DR cluster. The DR cluster will serve as a production cluster after an abnormal switchover, but the data that failed to be synchronized from the original production cluster to the DR cluster will not exist in the DR cluster.
      -  If the DR type is **Cross-region DR**, the switchover can be performed only in the region where the standby cluster is located.

Performing a DR Switchback
--------------------------

**Scenario**

After abnormal switchover, if you have confirmed that the original production cluster was recovered, you can perform a switchback.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane on the left, choose **DR Tasks**.

#. Click **DR Recovery** in the **Operation** column of a DR task.

   |image5|

#. In the displayed dialog box, set **Synchronization Mode** to **Incremental** or **Full**.

   .. note::

      You are advised to set **Synchronization Mode** to **Incremental** when updating a DR creation task.

#. Click **OK**.

   The **Status** will change to **Recovering**.

   After the DR recovery is successful, the **Status** will change to **Running**.

   .. note::

      -  DR is supported only by clusters of 8.1.2 or later.
      -  During DR recovery, data in the DR cluster will be deleted, and the DR relationship will be re-established with the new production cluster.
      -  If the DR type is **Cross-region DR**, the recovery can be performed only in the region where the standby cluster is located.

Updating DR Configurations
--------------------------

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane on the left, choose **DR Tasks**.

#. In the DR list, click the DR name to go to the DR information page.

#. In the **DR Configurations** area, click **Modify**.

   |image6|

   .. note::

      -  Only DR tasks in the **Not started** or **Stopped** state can be modified.
      -  The new configuration takes effect after DR is restarted.

.. _en-us_topic_0000001707293893__en-us_topic_0000001423159725_section1631535174714:

Deleting DR Tasks
-----------------

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane on the left, choose **DR Tasks**.

#. Click **Delete** in the **Operation** column of the target DR task.

   |image7|

#. In the dialog box that is displayed, click **OK**.

   The DR status will change to **Deleting**.

   .. note::

      -  You can delete a DR task when **DR Status** is **Creation failed**, **Not started**, **Startup failed**, **Stopped**, **Stop failed**, or **Abnormal**.
      -  Data cannot be synchronized after a DR task is deleted, and the deleted task cannot be restored.

.. |image1| image:: /_static/images/en-us_image_0000001711599192.png
.. |image2| image:: /_static/images/en-us_image_0000001711439700.png
.. |image3| image:: /_static/images/en-us_image_0000001759518593.png
.. |image4| image:: /_static/images/en-us_image_0000001759358737.png
.. |image5| image:: /_static/images/en-us_image_0000001711599196.png
.. |image6| image:: /_static/images/en-us_image_0000001759518597.png
.. |image7| image:: /_static/images/en-us_image_0000001759358741.png
