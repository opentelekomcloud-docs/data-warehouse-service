:original_name: dws_01_7115.html

.. _dws_01_7115:

CNs
===

Purpose
-------

After a cluster is created, the number of required CNs varies with service requirements. The CN management function enables you to adjust the number of CNs in the cluster. The operations are as follows:

-  :ref:`Adding CNs <en-us_topic_0000001134400786__en-us_topic_0000001083058054_section941730165216>`
-  :ref:`Deleting CNs <en-us_topic_0000001134400786__en-us_topic_0000001083058054_section7292342175210>`

   .. note::

      This feature is supported only in cluster version 8.1.1 or later.

Constraints and Limitations
---------------------------

-  During resource provisioning, the default number of CNs is 3. You can adjust the number of CNs based on the number of provisioned nodes. The number of CNs ranges from 2 to 20.
-  Do not perform other O&M operations when adding or deleting a CN.
-  Stop services when adding or deleting a CN.
-  If a fault occurs when you add or delete a CN and the rollback also fails, log in to the background to rectify the fault. For details, see .

.. _en-us_topic_0000001134400786__en-us_topic_0000001083058054_section941730165216:

Adding CNs
----------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the cluster to which you want to add CNs.

#. In the **Operation** column of the specified cluster, choose **More** > **Manage CN** > **Add CN Node**.

   |image1|

#. On the displayed page, set the number of CNs as required and click **OK**.

   |image2|

   .. important::

      -  Before adding a CN, ensure that the cluster is in the **Available**, **Unbalanced**, or **Redistributing** state.
      -  The number of CNs after adjustment must be greater than the number of deployed CNs, less than or equal to the number of nodes, and less than or equal to 20.

.. _en-us_topic_0000001134400786__en-us_topic_0000001083058054_section7292342175210:

Deleting CNs
------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the cluster from which you want to delete CNs.

#. In the **Operation** column of the specified cluster, choose **More** > **Manage CN** > **Delete CN Node**.

   |image3|

#. On the displayed page, select the CN to be deleted and click **OK**.

   |image4|

   .. important::

      -  At least one CN must be reserved when you delete a CN.
      -  When you delete a CN, the cluster must be in the **Available**, **Degraded**, or **Unbalanced** state.
      -  If an elastic IP address has been bound to a CN, the CN cannot be deleted.
      -  If abnormal nodes exist, only the abnormal CNs can be deleted.

         -  If one CN is faulty, only this CN can be deleted.
         -  If two or more CNs are faulty, no CN can be deleted.

.. |image1| image:: /_static/images/en-us_image_0000001432579569.png
.. |image2| image:: /_static/images/en-us_image_0000001180440335.png
.. |image3| image:: /_static/images/en-us_image_0000001382420930.png
.. |image4| image:: /_static/images/en-us_image_0000001134400964.png
