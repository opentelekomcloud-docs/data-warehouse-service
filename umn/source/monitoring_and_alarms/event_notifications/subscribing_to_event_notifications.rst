:original_name: dws_01_0101.html

.. _dws_01_0101:

Subscribing to Event Notifications
==================================

After subscribing to GaussDB(DWS) event notification, you will receive notifications by text message, email, or application when management, monitoring, or security events occur in a specific cluster or snapshot.

Creating a Subscription
-----------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation tree on the left, click **Event Management**.

#. On the **Event Management** page, choose **Subscription** > **Create Subscription**.

#. .. _en-us_topic_0000001466754514__li18772435125416:

   In the **Subscription Settings** area, set basic subscription information and event filtering.

   The **Subscribed Event List** area displays the events filtered by the system based on the subscription settings.

   .. table:: **Table 1** Subscription parameters

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                                           |
      +===================================+=======================================================================================================================================================================================================================================================================================+
      | Notification                      | Enable or disable event subscription.                                                                                                                                                                                                                                                 |
      |                                   |                                                                                                                                                                                                                                                                                       |
      |                                   | |image1| indicates that event subscription is enabled. |image2| indicates that event subscription is disabled. This function is enabled by default. After the function is disabled, the system stops sending notifications of subscribed events but does not delete the subscription. |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Subscription Name                 | Enter the name of a subscription.                                                                                                                                                                                                                                                     |
      |                                   |                                                                                                                                                                                                                                                                                       |
      |                                   | -  The name can contain letters (upper or lower case), digits, hyphens (-), and underscores (_) and must start with a letter or digit.                                                                                                                                                |
      |                                   | -  The name must be between 1 and 256 characters in length.                                                                                                                                                                                                                           |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Event Type                        | Select the type of the event to be subscribed. Possible values are **Management**, **Monitoring**, and **Security**.                                                                                                                                                                  |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Event Severity                    | Select the alarm severity of the event. Possible values are **Normal** and **Warning**.                                                                                                                                                                                               |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Event Source Category             | Select the event source category: cluster, snapshot,.                                                                                                                                                                                                                                 |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Select a message notification topic from the **Message Notification Topic** drop-down list.

   -  The selected topic must have granted GaussDB(DWS) the permission to publish messages to the topic.

      If GaussDB(DWS) has not been authorized to publish messages to the selected topic, go to the topic management page of the SMN console to configure topic authorization. For details, see **Topic** **Management** > **Configuring Topic Policies** in the *Simple Message Notification User Guide*. When configuring the topic policy, select **GaussDB(DWS)** for **Services that can publish messages to this topic**.

   -  To create a topic, click **Create Topic**. The SMN console is displayed. For details, see **Topic Management > Creating a Topic** in the *Simple Message Notification User Guide*.

#. .. _en-us_topic_0000001466754514__li164751612342:

   Click **OK** to complete the subscription.

Modifying the Subscription
--------------------------

#. On the **Event Management** page of the GaussDB(DWS) management console, click the **Subscription** tab.
#. In the **Operation** column of the row containing the specified subscription, click **Edit** to enter the **Edit Subscription** page.
#. On the **Edit Subscription** page, set the parameters to be modified. For details, see :ref:`4 <en-us_topic_0000001466754514__li18772435125416>` to :ref:`6 <en-us_topic_0000001466754514__li164751612342>` in section "Creating a Subscription".

Deleting the Subscription
-------------------------

#. On the **Event Management** page of the GaussDB(DWS) management console, click the **Subscription** tab.
#. In the **Operation** column of the row containing the specified subscription, click **Delete**. The **Delete Subscription** dialog box is displayed.
#. Click **Yes** to delete the subscription.

.. |image1| image:: /_static/images/en-us_image_0000001517914005.png
.. |image2| image:: /_static/images/en-us_image_0000001517754413.jpg
