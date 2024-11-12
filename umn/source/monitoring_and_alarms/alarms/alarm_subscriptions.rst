:original_name: dws_01_1243.html

.. _dws_01_1243:

Alarm Subscriptions
===================

You can subscribe to GaussDB(DWS) alarm notifications to receive notifications by SMS message, email, or application when an alarm of a specified severity is generated.

Creating a Subscription
-----------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **Alarms** > **Subscriptions**.

#. Click **Create Subscription** in the upper left corner of the page.

#. .. _en-us_topic_0000001924728724__li18772435125416:

   In the **Subscription Settings** area, configure the basic information and alarm severity of the subscription.

   |image1|

   .. table:: **Table 1** Subscription parameters

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                            |
      +===================================+========================================================================================================================================+
      | Status                            | Whether to enable the alarm subscription.                                                                                              |
      |                                   |                                                                                                                                        |
      |                                   | When you disable a subscription, you will not receive the corresponding alarm notifications, but the subscription will not be deleted. |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Subscription Name                 | Name of the alarm subscription:                                                                                                        |
      |                                   |                                                                                                                                        |
      |                                   | -  Contains only letters, digits, hyphens (-), and underscores (_), and must start with a letter or digit.                             |
      |                                   | -  Contains 1 to 256 characters.                                                                                                       |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Alarm Severity                    | Severity of the alarm you want to subscribe to: **Urgent**, **Important**, **Minor**, or **Prompt**                                    |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+

#. .. _en-us_topic_0000001924728724__li125355552147:

   The **Subscribed Alarms** area displays the subscribed alarms by subscription settings. Select an SMN topic from the drop-down list.

   To create a topic, click **Create Topic** to switch to the SMN console page. For details, .

   .. note::

      The selected topic must have granted GaussDB(DWS) the permission for publishing messages to the topic. To grant permissions, configure topic policies on the SMN management console. When configuring the topic policy, select **DWS** as the service that can publish messages to this topic.

#. Confirm the information and click **OK**.

Modifying a Subscription
------------------------

#. Log in to the GaussDB(DWS) management console.
#. In the navigation pane on the left, choose **Alarms** > **Subscriptions**.
#. In the **Operation** column of the target subscription, click **Edit**.
#. On the **Edit Subscription** page displayed, modify the parameters. For details, see :ref:`4 <en-us_topic_0000001924728724__li18772435125416>` to :ref:`5 <en-us_topic_0000001924728724__li125355552147>`.
#. Click **OK**.

Deleting a Subscription
-----------------------

#. Log in to the GaussDB(DWS) management console.
#. In the navigation pane on the left, choose **Alarms** > **Subscriptions**.
#. In the **Operation** column of the target subscription, click **Delete**. A confirmation dialog box is displayed.
#. Click **Yes** to delete the subscription.

.. |image1| image:: /_static/images/en-us_image_0000001924569824.png
