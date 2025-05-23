:original_name: dws_01_1331.html

.. _dws_01_1331:

Node Monitoring
===============


Node Monitoring
---------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** and locate the cluster to be monitored.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**. The database monitoring page is displayed.

#. In the navigation pane on the left, choose **Monitoring** > **Node Monitoring**.

   On the page that is displayed, view the real-time consumption of nodes, memory, disks, disk I/O, and network I/O.

Overview
--------

On the **Overview** tab page, you can view the key resources of a specified node based on the node name, including:

-  Node Name
-  CPU Usage (%)
-  Memory Usage (%)
-  Average Disk Usage (%)
-  IP Address
-  Disk I/O (KB/s)
-  TCP Protocol Stack Retransmission Rate (%)
-  Network I/O (KB/s)
-  Status
-  Monitoring: You can click |image1| in the **Monitoring** column to view the overall performance metric topology of the node in the last 1 hour, last 3 hours, last 12 hours, last 24 hours, last 7 days, or last 15 days.

|image2|

Disks
-----

On the **Disks** tab page, view the real-time disk resource consumption of a node by node name and disk name, including:

-  Node Name
-  Disk Name
-  Disk Type

   -  System disk
   -  Data disk
   -  Log disk

-  Disk Capacity (GB)
-  Disk Usage (%)
-  Disk Read Rate (KB/s)
-  Disk Write Rate (KB/s)
-  I/O Wait Time (await, ms)
-  I/O Service Time (svctm, ms)
-  IOPS
-  Monitoring: You can click |image3| in the **Monitoring** column to view the disk performance metric topology of the node in the last 1 hour, 3 hours, 12 hours, 24 hours, 7 days, or 15 days.

|image4|

.. note::

   The sum of the used disk space and available disk space is not equal to the total disk space. This is because a small amount of space is reserved in each default partition for system administrators to use. Even if common users have run out of space, system administrators can log in to the system and use their space required for solving problems.

   Run the Linux **df** command to collect the disk capacity information, as shown in the following figure.

   |image5|

   /dev/sda4: Used(5757444) + Available(540228616) != Total(569616888)

   -  **Filesystem**: path name of the device file corresponding to the file system. Generally, it is a hard disk partition.
   -  **IK-blocks**: number of data blocks (1024 bytes) in a partition.
   -  **Used**: number of data blocks used by the disk.
   -  **Available**: number of available data blocks on the disk.
   -  **Use%**: percentage of the space used by common users. Even if the space is used up, the partition still reserves the space for system administrators.
   -  **Mounted on**: mount point of the file system.

Network
-------

On the **Network** tab page, view the real-time network resource consumption of a node by node name and NIC name, including:

-  Node Name
-  NIC Name
-  NIC Status
-  NIC Speed (Mbps)
-  Received Packets
-  Sent Packets
-  Lost Packets Received
-  Receive Rate (KB/s)
-  Transmit Rate (KB/s)
-  Monitoring: You can click |image6| in the **Monitoring** column to view the network performance metric topology of the node in the last 1 hour, 3 hours, 12 hours, 24 hours, 7 days, or 15 days.

|image7|

.. |image1| image:: /_static/images/en-us_image_0000002168066584.png
.. |image2| image:: /_static/images/en-us_image_0000002203312389.png
.. |image3| image:: /_static/images/en-us_image_0000001328622480.png
.. |image4| image:: /_static/images/en-us_image_0000002168065860.png
.. |image5| image:: /_static/images/en-us_image_0000002203426845.png
.. |image6| image:: /_static/images/en-us_image_0000001328622480.png
.. |image7| image:: /_static/images/en-us_image_0000002167906156.png
