:original_name: dws_01_0818.html

.. _dws_01_0818:

Nodes
=====

Overview
--------

On the **Nodes** tab page, you can view the node list of the current cluster, add new nodes to or remove nodes from it, and view the node usage, status, and flavors.

|image1|

.. note::

   -  This feature is supported only in 8.1.1.200 or later.

Adding Nodes
------------

This function is more suited for large-scale scale-out. Nodes can be added in batches in advance. For example, if 180 more BMS nodes are needed, add them in three batches (60 for each batch). If some nodes fail to be added, add them again. After all the 180 nodes are successfully added, use the nodes for cluster scale-out. Adding nodes does not affect cluster services.

**Precautions**

-  Nodes can be added only when no other task is running on the management side.
-  The storage size of a new node must be the same as that of each of the existing nodes in the cluster.
-  The nodes that are successfully added are called idle nodes, which will be charged regardless of whether they are used or not. Therefore, add nodes when necessary and use the nodes as soon as possible.
-  The anti-affinity rule dictates that the number of nodes to be added at a time must be an integer multiple of the cluster ring size. For example, if the cluster ring size is 3, the number of nodes to be added must be an integer multiple of 3.
-  The anti-affinity rule dictates that, if a node fails to be added and is rolled back, other nodes that are being added in the same server group will also be rolled back.

**Procedure**

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. All clusters are displayed by default.

#. Click the name of the target cluster. On the **Basic Information** page that is displayed, click **Nodes**.

#. Click **Add Node**, enter the number of nodes to be added, and click **Next: Confirm**.

   |image2|

#. Click **Submit**. The nodes will start to be added, as shown in the following figure.

   |image3|

   |image4|

.. note::

   The nodes that fail to be added will be automatically rolled back and recorded in the displayed list, as shown in the following figure.

   |image5|

Removing Nodes
--------------

**Precautions**

-  Nodes can be removed only when no other task is running on the management side.
-  Only nodes whose resource status is **Idle** can be removed. Nodes that are in use cannot be removed.
-  In anti-affinity deployment, nodes are removed by cluster ring. For example, when you remove a node, other nodes in the same ring will be automatically selected and displayed.

**Procedure**

#. Log in to the GaussDB(DWS) management console.

#. Choose **Clusters**. All clusters are displayed by default.

#. Click the name of the target cluster. On the **Basic Information** page that is displayed, click **Nodes**.

#. On the **Nodes** page, select the nodes to be removed, click **Remove**, and click **Yes** to submit the task. The nodes that are successfully removed will not be displayed on the **Nodes** page.

   |image6|

   |image7|

.. |image1| image:: /_static/images/en-us_image_0000001204609361.png
.. |image2| image:: /_static/images/en-us_image_0000001188917631.png
.. |image3| image:: /_static/images/en-us_image_0000001204632331.png
.. |image4| image:: /_static/images/en-us_image_0000001204872339.png
.. |image5| image:: /_static/images/en-us_image_0000001159474436.png
.. |image6| image:: /_static/images/en-us_image_0000001204848125.png
.. |image7| image:: /_static/images/en-us_image_0000001204609685.png
