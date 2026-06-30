:original_name: dws_01_1243.html

.. _dws_01_1243:

Alarm Subscriptions
===================

You can subscribe to DWS alarm notifications to receive notifications by SMS message, email, or application when an alarm of a specified severity is generated.

Creating a Subscription
-----------------------

#. Log in to the DWS console.

#. In the navigation pane on the left, choose **Monitoring** > **Alarm** and click **Subscriptions**.

#. Click **Create Subscription** in the upper left corner of the page.

#. .. _en-us_topic_0000002235494432__li18772435125416:

   In the **Subscription Settings** area, configure the basic information and alarm severity of the subscription.

   .. table:: **Table 1** Subscription parameters

      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                            |
      +===================================+========================================================================================================================================+
      | Status                            | Whether to enable the alarm subscription.                                                                                              |
      |                                   |                                                                                                                                        |
      |                                   | When you disable a subscription, you will not receive the corresponding alarm notifications, but the subscription will not be deleted. |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Subscription Name                 | Name of the alarm subscription. It can                                                                                                 |
      |                                   |                                                                                                                                        |
      |                                   | -  Contain only letters, digits, hyphens (-), and underscores (_), and must start with a letter or digit.                              |
      |                                   | -  Contain 1 to 256 characters.                                                                                                        |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+
      | Cluster                           | Select the cluster to subscribe to. Note that only one cluster can be subscribed to multiple alarms.                                   |
      +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------+

#. In the alarm list, select the alarms to be subscribed to. For details about the alarms that can be subscribed to, see :ref:`Table 1 <en-us_topic_0000002235494312__table1380210278592>`.

#. .. _en-us_topic_0000002235494432__li125355552147:

   Select an SMN topic from the drop-down list.

   .. note::

      The selected topic must have granted DWS the permission for publishing messages to the topic. To grant permissions, configure topic policies on the SMN management console. When configuring the topic policy, select **DWS** for services that can publish messages to this topic.

#. (Optional) To create a topic, click **Create Topic**. The SMN console is displayed.

#. Confirm the information and click **OK**.

Modifying a Subscription
------------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Monitoring** > **Alarm** and click **Subscriptions**.
#. In the **Operation** column of the target subscription, click **Edit**.
#. On the **Edit Subscription** page displayed, modify the parameters. For details, see :ref:`4 <en-us_topic_0000002235494432__li18772435125416>` to :ref:`5 <en-us_topic_0000002235494432__li125355552147>`.
#. Click **OK**.

Deleting a Subscription
-----------------------

#. Log in to the DWS console.
#. In the navigation pane on the left, choose **Monitoring** > **Alarm** and click **Subscriptions**.
#. In the **Operation** column of the target subscription, click **Delete**. A confirmation dialog box is displayed.
#. Click **Yes** to delete the subscription.
