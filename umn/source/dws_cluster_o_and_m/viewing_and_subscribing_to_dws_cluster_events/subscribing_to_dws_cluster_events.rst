:original_name: dws_01_0101.html

.. _dws_01_0101:

Subscribing to DWS Cluster Events
=================================

DWS uses the Simple Message Notification (SMN) service to send notifications of DWS events. The SMN function is only available by subscription. In a subscription, you need to specify one or more event filtering conditions. When an event that matches all filtering conditions occurs, DWS sends a notification based on the subscription. The filter conditions include the **Event Type** (for example, **Management**, **Monitoring**, or **Security**), **Event Severity** (for example, **Normal** or **Warning**), and **Event Source Category** (for example, **Cluster** or **Snapshot**).

:ref:`Figure 1 <en-us_topic_0000002356808521__en-us_topic_0000001423119165_fig115581629102618>` shows the basic process architecture of event subscription. After subscribing to DWS event notifications, you will receive notifications by text message, email, or application when management, monitoring, or security events occur in a specific cluster or snapshot.

.. _en-us_topic_0000002356808521__en-us_topic_0000001423119165_fig115581629102618:

.. figure:: /_static/images/en-us_image_0000002357295705.png
   :alt: **Figure 1** Event subscription architecture

   **Figure 1** Event subscription architecture

Prerequisites
-------------

Before creating a subscription, you have created a notification topic. For details, see "Topic Management" > "Creating a Topic" in the *Simple Message Notification User Guide*.

Creating a Subscription
-----------------------

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Dedicated Clusters** > **Management** > **Events**.

#. On the **Events** page, click the **Subscriptions** tab and then **Create Subscription**.

#. .. _en-us_topic_0000002356808521__en-us_topic_0000001423119165_li18772435125416:

   In the **Subscription Settings** area, set basic subscription information and event filtering conditions.

   The **Subscribed Event** area displays the events filtered by the system based on the subscription settings.

   .. table:: **Table 1** Subscription parameters

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                            |
      +===================================+========================================================================================================================================+
      | Notification                      | Enable or disable event subscription.                                                                                                  |
      |                                   |                                                                                                                                        |
      |                                   | After notification is disabled, the system stops sending notifications of subscribed events but does not delete the subscription.      |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Subscription Name                 | Enter the name of a subscription.                                                                                                      |
      |                                   |                                                                                                                                        |
      |                                   | -  The name can contain letters (upper or lower case), digits, hyphens (-), and underscores (_) and must start with a letter or digit. |
      |                                   | -  The name must be between 1 and 256 characters in length.                                                                            |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Event Type                        | Select the type of the event to be subscribed. Possible values are **Management**, **Monitoring**, and **Security**.                   |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Event Severity                    | Select the alarm severity of the event. Possible values are **Normal** and **Warning**.                                                |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Event Source Category             | Select the event source category: cluster or snapshot.                                                                                 |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+

#. Select an SMN topic from the **Message Notification Topic** drop-down list.

   -  The selected topic must have granted DWS the permission for publishing messages to the topic.

      To grant permissions, configure topic policies on the SMN management console. For details, see **Topic** **Management** > **Configuring Topic Policies** in the *Simple Message Notification User Guide*. When configuring the topic policy, select **DWS** for **Services that can publish messages to this topic**.

   -  To create a topic, click **Create Topic**. The SMN console is displayed. For details, see **Topic Management > Creating a Topic** in the *Simple Message Notification User Guide*.

#. .. _en-us_topic_0000002356808521__en-us_topic_0000001423119165_li164751612342:

   Click **OK** to complete the subscription.

Modifying the Subscription
--------------------------

#. On the DWS console, choose **Dedicated Clusters** > **Management**. On the displayed page, click the **Subscriptions** tab.
#. Locate a specified subscription and click **Edit** in the **Operation** column.
#. On the **Edit Subscription** page, modify parameters by referring to :ref:`4 <en-us_topic_0000002356808521__en-us_topic_0000001423119165_li18772435125416>` to :ref:`6 <en-us_topic_0000002356808521__en-us_topic_0000001423119165_li164751612342>`.

Deleting the Subscription
-------------------------

#. On the DWS console, choose **Dedicated Clusters** > **Management**. On the displayed page, click the **Subscriptions** tab.
#. Locate a specified subscription and click **Delete** in the **Operation** column.
#. In the displayed dialog box, click **Yes** to delete the subscription.
