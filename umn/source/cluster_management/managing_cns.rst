:original_name: dws_01_7115.html

.. _dws_01_7115:

Managing CNs
============

Purpose
-------

After a cluster is created, the number of required CNs varies with service requirements. The CN management function enables you to adjust the number of CNs in the cluster. The operations are as follows:

-  :ref:`Adding CNs <en-us_topic_0000001707293917__en-us_topic_0000001423119293_en-us_topic_0000001083058054_section941730165216>`
-  :ref:`Deleting CNs <en-us_topic_0000001707293917__en-us_topic_0000001423119293_en-us_topic_0000001083058054_section7292342175210>`

.. note::

   -  This feature is supported only in cluster version 8.1.1 or later.
   -  Only cluster versions 8.1.3.300 and later (excluding 8.2.0) support online CN addition, deletion, and concurrent addition of multiple CNs.

Constraints and Limitations
---------------------------

-  During resource provisioning, the default number of CNs is 3. You can adjust the number of CNs based on the number of provisioned nodes. The number of CNs ranges from 2 to 20.
-  Do not perform other O&M operations when adding or deleting a CN.
-  Adding CNs consumes lots of CPU and I/O resources, which will greatly impact job performance. You are advised to perform this operation during off-peak hours or after services are stopped.
-  If a fault occurs when you add a CN node and the rollback fails, try adding the CN again. The deletion of a CN node cannot be rolled back.
-  For a CN that fails to be added, you can only retry the addition. For a CN that fails to be deleted, you can only retry the deletion. Other O&M operations are not allowed for such CNs.
-  If DDL operations, such as schema and function creation, are performed during CN deletion, an error may be reported because the deleted CN cannot be found. In this case, try again.
-  If one of your CNs is abnormal, you can only delete this abnormal CN. If two or more CNs are abnormal, you can delete CNs only after the CNs are recovered from faults.

.. _en-us_topic_0000001707293917__en-us_topic_0000001423119293_en-us_topic_0000001083058054_section941730165216:

Adding CNs
----------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster to which you want to add CNs.

#. In the **Operation** column of the specified cluster, choose **More** > **Manage CN** > **Add CN Node**.

   |image1|

#. In the displayed dialog box, determine whether to add CNs to a specified node. If you select **No**, set the number of CNs after adjustment and click **OK**. If you select **Yes**, select a node and click **OK**.

   |image2|

   |image3|

   .. important::

      -  Before adding a CN, ensure that the cluster is in the **Available** or **Unbalanced** state.
      -  The number of CNs after adjustment cannot exceed the number of deployed CNs. It must be less than or equal to the number of nodes, and less than or equal to 20.

.. _en-us_topic_0000001707293917__en-us_topic_0000001423119293_en-us_topic_0000001083058054_section7292342175210:

Deleting CNs
------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** > **Dedicated Clusters** page, locate the cluster from which you want to delete CNs.

#. In the **Operation** column of the specified cluster, choose **More** > **Manage CN** > **Delete CN Node**.

   |image4|

#. On the displayed page, select the CN to be deleted and click **OK**.

   |image5|

   .. important::

      -  At least two CN must be retained.
      -  When you delete a CN, the cluster must be in the **Available**, **Degraded**, or **Unbalanced** state.
      -  If an elastic IP address has been bound to a CN, the CN cannot be deleted.
      -  If abnormal nodes exist, only the abnormal CNs can be deleted.

         -  If one CN is faulty, only this CN can be deleted.
         -  If two or more CNs are faulty, no CN can be deleted.

.. |image1| image:: /_static/images/en-us_image_0000001711599848.png
.. |image2| image:: /_static/images/en-us_image_0000001759519269.png
.. |image3| image:: /_static/images/en-us_image_0000001759359401.png
.. |image4| image:: /_static/images/en-us_image_0000001711440376.png
.. |image5| image:: /_static/images/en-us_image_0000001711599880.png
