:original_name: dws_01_1333.html

.. _dws_01_1333:

Session Monitoring
==================


Session Monitoring
------------------

#. Log in to the GaussDB(DWS) management console.

#. On the **Clusters** page, locate the target cluster.

#. In the **Operation** column of the target cluster, click **Monitoring Panel**.

#. In the navigation pane on the left, choose **Monitoring** > **Session Monitoring**.

   The **Session Monitoring** page displays the session-level real-time database query statistics. You can also select and terminate a session.

Query Statistics
----------------

You can browse the query statistics of a specified session based on the session ID, including:

-  Session ID
-  User Name
-  Database Name
-  Session Duration (ms)
-  Application Name
-  Queries
-  Last Query Duration (ms)
-  Client IP Address
-  Connected CN
-  Session Status

|image1|

Terminating a Session
---------------------

Select a session to be terminated, click **Terminate a Session**, and confirm your operation.

.. note::

   The fine-grained permission control function is added. Only users with the operate permission are able to terminate sessions. For users with the read-only permission, the **Terminate a Session** button is grayed out.

.. |image1| image:: /_static/images/en-us_image_0000001134560668.png
