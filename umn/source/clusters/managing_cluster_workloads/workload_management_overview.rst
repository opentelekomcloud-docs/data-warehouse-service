:original_name: dws_01_07231.html

.. _dws_01_07231:

Workload Management Overview
============================

Overview
--------

When multiple database users query jobs at the same time, some complex queries may occupy cluster resources for a long time, affecting the performance of other queries. For example, a group of database users continuously submit complex and time-consuming queries, while another group of users frequently submit short queries. In this case, short queries may have to wait in the queue for the time-consuming queries to complete.

To improve efficiency, you can use the GaussDB(DWS) workload management function to handle such problems. GaussDB(DWS) workload management uses workload queues as resource bearers. You can create different workload queues for different service types and configure different resource ratios for these queues. Then, add database users to the corresponding queues to restrict their resource usages. For example, classify database users who frequently submit complex query jobs into one type, create a workload queue for these users, allocate more resources to the queue, and then add these users to the queue. In this case, the complex jobs submitted by these users can use only the resources of the created queue. Also, create a queue that occupies fewer resources and allocate it to users who submit short queries. In this way, the two types of jobs can be executed at the same time without affecting each other.

.. important::

   -  If a resource pool has been created on the backend in the database of the earlier version, delete it and create a new one on the frontend. For details, contact technical support.

Page Overview
-------------

On the **Workload Management** page, you can modify the global configurations of workload management, add, create, and modify workload queues, add database users to queues, and remove database users from queues.

Short Query Configuration
-------------------------

In the **Short Query Configuration** area, you can enable or disable the short query acceleration function. To change the number of concurrent short queries (**-1** by default. **0** or **-1** indicates that the concurrent short queries are not controlled), you can enable short query acceleration.

|image1|

Resource Configuration
----------------------

In the **Resource Configuration** area, you can view the resource configuration of the current workload queue, including the CPU resource (%), memory resource (%), storage resource (MB), and query concurrency.

|image2|

Exception Rule
--------------

In the **Exception Rule** area, you can view the exception rule settings of the current workload queue. Exception rules allow you to control exceptions of jobs executed by associated users in the queue.

|image3|

Associated User
---------------

In the **Associated User** list, you can view the associated users of the current workload queue, and the memory and disk usage of each user at the current time, as shown in the following figure.

|image4|

Entering Workload Management Page
---------------------------------

#. Log in to the GaussDB(DWS) management console.
#. On the displayed **Clusters** page, click the name of the target cluster.
#. Switch to the **Workload Management** tab page.

Enabling/Disabling Workload Management
--------------------------------------

The **Workload Management Configuration** area includes the **Workload Switch** and **Maximum Concurrency** parameters. **Maximum Concurrency** refers to the maximum concurrent queries on a single CN. If you disable **Workload Switch**, all workload management functions will be unavailable.

|image5|

.. |image1| image:: /_static/images/en-us_image_0000001134400908.png
.. |image2| image:: /_static/images/en-us_image_0000001134560694.png
.. |image3| image:: /_static/images/en-us_image_0000001180440273.png
.. |image4| image:: /_static/images/en-us_image_0000001180440275.png
.. |image5| image:: /_static/images/en-us_image_0000001180440271.png
