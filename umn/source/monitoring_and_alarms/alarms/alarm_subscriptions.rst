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

#. .. _en-us_topic_0000001467074026__li18772435125416:

   In the **Subscription Settings** area, configure the basic information and alarm severity of the subscription.

   |image1|

   .. table:: **Table 1** Subscription parameters

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                                                                                   |
      +===================================+===============================================================================================================================================================================================================================================================+
      | Status                            | Whether to enable the alarm subscription.                                                                                                                                                                                                                     |
      |                                   |                                                                                                                                                                                                                                                               |
      |                                   | |image2| indicates that the alarm subscription is enabled. |image3| indicates that the alarm subscription is disabled. When you disable a subscription, you will not receive the corresponding alarm notifications, but the subscription will not be deleted. |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Subscription Name                 | Name of the alarm subscription:                                                                                                                                                                                                                               |
      |                                   |                                                                                                                                                                                                                                                               |
      |                                   | -  Contains only letters, digits, hyphens (-), and underscores (_), and must start with a letter or digit.                                                                                                                                                    |
      |                                   | -  Contains 1 to 256 characters.                                                                                                                                                                                                                              |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Alarm Severity                    | Severity of the alarm you want to subscribe to: **Urgent**, **Important**, **Minor**, or **Prompt**                                                                                                                                                           |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. .. _en-us_topic_0000001467074026__li125355552147:

   The **Subscribed Alarms** area displays the subscribed alarms by subscription settings. Select an SMN topic from the drop-down list.

   To create a topic, click **Create Topic** to switch to the SMN console page. For details, .

   |image4|

   .. note::

      The selected topic must have granted GaussDB(DWS) the permission for publishing messages to the topic. To grant permissions, configure topic policies on the SMN management console. For details, see . When configuring the topic policy, select **DWS** for services that can publish messages to this topic.

#. Confirm the information and click **OK**.

Modifying a Subscription
------------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **Alarms** > **Subscriptions**.

#. In the **Operation** column of the target subscription, click **Edit**.

   |image5|

#. On the **Edit Subscription** page displayed, modify the parameters. For details, see :ref:`4 <en-us_topic_0000001467074026__li18772435125416>` to :ref:`5 <en-us_topic_0000001467074026__li125355552147>`.

   |image6|

#. Click **OK**.

Deleting a Subscription
-----------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, choose **Alarms** > **Subscriptions**.

#. In the **Operation** column of the target subscription, click **Delete**. A confirmation dialog box is displayed.

   |image7|

#. Click **Yes** to delete the subscription.

.. |image1| image:: /_static/images/en-us_image_0000001517355421.png
.. |image2| image:: /_static/images/en-us_image_0000001466914370.png
.. |image3| image:: /_static/images/en-us_image_0000001466914366.png
.. |image4| image:: /_static/images/en-us_image_0000001517754437.png
.. |image5| image:: /_static/images/en-us_image_0000001467074226.png
.. |image6| image:: /_static/images/en-us_image_0000001466595098.png
.. |image7| image:: /_static/images/en-us_image_0000001466754746.png
