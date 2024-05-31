:original_name: dws_01_00085.html

.. _dws_01_00085:

Mutually Exclusive DR Cases
===========================

Case 1: How Do I Scale out a Cluster in the DR State?
-----------------------------------------------------

#. Log in to the GaussDB(DWS) console.

#. In the navigation pane on the left, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, if **Task Information** of the cluster you want to scale out is **DR not started**, perform :ref:`5 <en-us_topic_0000001659054594__en-us_topic_0000001422799521_li18914144545215>` and :ref:`7 <en-us_topic_0000001659054594__en-us_topic_0000001422799521_li51916418241>`.

#. If the **Task Information** is other than **DR not started**, delete the DR task. For details, see :ref:`Deleting DR Tasks <en-us_topic_0000001707293893__en-us_topic_0000001423159725_section1631535174714>`.

#. .. _en-us_topic_0000001659054594__en-us_topic_0000001422799521_li18914144545215:

   In the **Operation** column of the production and DR clusters, choose **More** > **Scale Out**.

   |image1|

#. Create a DR task. For details, see :ref:`Creating a DR Task <dws_01_00082>`.

#. .. _en-us_topic_0000001659054594__en-us_topic_0000001422799521_li51916418241:

   Start the DR task. For details, see :ref:`Starting a DR Task <en-us_topic_0000001707293893__en-us_topic_0000001423159725_section4432124194612>`.

   .. note::

      After scale-out, the number of DNs in the production cluster must be the same as that in the DR cluster.

.. |image1| image:: /_static/images/en-us_image_0000001711599200.png
