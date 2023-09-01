:original_name: dws_01_07231.html

.. _dws_01_07231:

Page Overview
=============

Overview
--------

On the resource management page, you can modify global resource management configurations; add, create, and modify resource queues; add database users to a resource pool; and remove a database user from a resource pool. The page consists of the following modules:

-  :ref:`Enabling or Disabling Resource Management <en-us_topic_0000001517355249__section74345085>`
-  :ref:`Short Query Configuration <en-us_topic_0000001517355249__en-us_topic_0000001076801475_s4a09406e8f3349b291f6db0d5a9af329>`
-  :ref:`Resource Configuration <en-us_topic_0000001517355249__en-us_topic_0000001076801475_s5e5d6e352af34816a3d135a1b4e4d4ba>`
-  :ref:`Exception Rule <en-us_topic_0000001517355249__en-us_topic_0000001076801475_se98f1ed737af4df4adb73cd007b1b384>`
-  :ref:`User Association <en-us_topic_0000001517355249__en-us_topic_0000001076801475_sa6c457572f8c403e9929518ebc352785>`

|image1|

After a cluster is converted into a logical cluster, you can create, modify, or delete a resource pool in the logical cluster.

|image2|

.. _en-us_topic_0000001517355249__section74345085:

Enabling or Disabling Resource Management
-----------------------------------------

You can enable or disable resource management, and configure the maximum global concurrency. **Max. Concurrent Queries** refers to the maximum concurrent queries on a single CN. If you disable **Resource Management**, all resource management functions will be unavailable.

|image3|

.. _en-us_topic_0000001517355249__en-us_topic_0000001076801475_s4a09406e8f3349b291f6db0d5a9af329:

Short Query Configuration
-------------------------

In the **Short Query Configuration** area, you can enable or disable the short query acceleration function. To change the number of simple statements (**-1** by default. **0** or **-1** indicates that the concurrent short queries are not controlled), you can enable short query acceleration.

|image4|

.. _en-us_topic_0000001517355249__en-us_topic_0000001076801475_s5e5d6e352af34816a3d135a1b4e4d4ba:

Resource Configuration
----------------------

In the **Resource Configuration** area, you can view the resource configuration of the current workload queue. For example:

|image5|

.. _en-us_topic_0000001517355249__en-us_topic_0000001076801475_se98f1ed737af4df4adb73cd007b1b384:

Exception Rule
--------------

In the **Exception Rule** area, you can view the exception rule settings of the current resource pool. You can configure how job exceptions in the resource pool are handled. For more information, see :ref:`Exception parameters <en-us_topic_0000001467074066__table595493692317>`.

|image6|

.. _en-us_topic_0000001517355249__en-us_topic_0000001076801475_sa6c457572f8c403e9929518ebc352785:

User Association
----------------

In the **Associated User** list, you can view the associated users of the current resource pool, and the memory and disk usage of each user at the current time, as shown in the following figure.

|image7|

.. note::

   If no resource pools are associated with a user, the user will be associated with **default_pool** by default, and its resource usage will be restricted by **default_pool**. The **default_pool** will be automatically created after resource management is enabled.

.. |image1| image:: /_static/images/en-us_image_0000001518034069.png
.. |image2| image:: /_static/images/en-us_image_0000001517754597.png
.. |image3| image:: /_static/images/en-us_image_0000001466754902.png
.. |image4| image:: /_static/images/en-us_image_0000001466595250.png
.. |image5| image:: /_static/images/en-us_image_0000001466914530.png
.. |image6| image:: /_static/images/en-us_image_0000001517355573.png
.. |image7| image:: /_static/images/en-us_image_0000001517914177.png
