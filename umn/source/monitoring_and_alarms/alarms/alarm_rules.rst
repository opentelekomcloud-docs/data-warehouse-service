:original_name: dws_01_1242.html

.. _dws_01_1242:

Alarm Rules
===========

Overview
--------

-  Concepts related to threshold alarms

   -  Alarm rule: consists of the alarm rule name, rule description, clusters associated with the rule, alarm policy triggering relationship, and alarm policy. An alarm rule can apply to one or all clusters, and can consist of one or more policies. The relationship between alarm policies can be selected in **Triggered Policies**. Each alarm policy consists of the triggers and constraint of each alarm rule.
   -  Alarm policy: consists of the triggers, constraint, and alarm severity for an alarm metric.
   -  Alarm metric: indicates a database cluster metric, which is generally time series data, for example, node CPU usage and amount of data flushed to disks.

-  Alarm rule types

   -  Default rule: best practices of GaussDB(DWS) threshold alarms.
   -  User-defined rule: personalized alarm rules by configuring or combining monitoring metrics. (The current version supports only user-defined alarm rules of schema usage.)

-  Alarm rule operations

   -  Modify: modifies an alarm rule. All alarm rules apply (all items of user-defined alarm rules but only some items of the default alarm rules).
   -  Enable/Disable: enables or disables an alarm rule. All alarm rules apply. When an alarm rule is enabled, it is added to the check list of the alarm engine and can be triggered normally. Disabled rules are not in the check list.
   -  Delete: deletes an alarm rule. You can delete only user-defined rules. Default alarm rules cannot be deleted.

Viewing Alarm Rules
-------------------

#. Log in to the GaussDB(DWS) management console.
#. In the navigation pane on the left, choose **Alarms**.
#. Click **View Alarm Rule** in the upper left corner. On the page that is displayed, you can see the threshold alarm rules of database cluster monitoring metrics, as shown in the following figure.\ |image1|

Modifying an Alarm Rule
-----------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, click **Alarms**.

#. Click **View Alarm Rule** in the upper left corner.

#. On the **Alarm Rules** page that is displayed, click **Modify** in the **Operation** column of the target alarm rule.

   -  **Alarm Rule**
   -  **Description**
   -  **Associated Cluster**: From the drop-down list, select the current tenant's clusters to which the alarm rule applies.
   -  **Triggered Policies**

      -  **Independent**: Alarm policies are triggered independently of each other.
      -  **Priority**: Alarm policies are triggered by priority. Policies of a lower priority will be automatically triggered after those of a higher priority.

   -  **Alarm Policy**

      -  **Metric**: GaussDB(DWS) monitoring metric, which is the data source used by the alarm engine for threshold determination.
      -  **Trigger**: calculation rule for threshold determination of a monitoring metric. Select the average value within a period of time of a metric to reduce the probability of alarm oscillation.
      -  **Constraint**: suppresses the repeated triggering and clearance of alarms of the same type within the specified period.
      -  **Alarm Severity**: includes **Urgent**, **Important**, **Minor**, and **Prompt**.

   |image2|

   .. note::

      You can modify only some items of the default rules (associated cluster, alarm policy threshold, time period, and alarm constraint). User-defined rules support modification of all items.

#. Confirm the information and click **OK**.

Creating an Alarm Rule
----------------------

#. Log in to the GaussDB(DWS) management console.
#. In the navigation pane on the left, click **Alarms**.
#. Click **View Alarm Rule** in the upper left corner.
#. Click **Create Alarm Rule** in the upper right corner. You can configure items, such as the alarm rule name, rule description, clusters associated with the rule, and alarm policy.

   -  **Alarm Rule**
   -  **Description**
   -  **Associated Cluster**: From the drop-down list, select the current tenant's clusters to which the alarm rule applies.
   -  **Triggered Policies**

      -  **Independent**: Alarm policies are triggered independently of each other.
      -  **Priority**: Alarm policies are triggered by priority. Policies of a lower priority will be automatically triggered after those of a higher priority.

   -  **Alarm Policy**

      -  **Metric**: GaussDB(DWS) monitoring metric, which is the data source used by the alarm engine for threshold determination.

      -  **Alarm Object**: databases in the selected cluster and schemas in the selected databases.

      -  **Trigger**: calculation rule for threshold determination of a monitoring metric. Select the average value within a period of time of a metric to reduce the probability of alarm oscillation.

      -  **Constraint**: suppresses the repeated triggering and clearance of alarms of the same type within the specified period.

      -  **Alarm Severity**: includes **Urgent**, **Important**, **Minor**, and **Prompt**.


         .. figure:: /_static/images/en-us_image_0000001214965259.png
            :alt: **Figure 1** Creating an alarm rule

            **Figure 1** Creating an alarm rule

         .. note::

            Currently, only alarm rules of schema usage metrics can be created on GaussDB(DWS).

.. |image1| image:: /_static/images/en-us_image_0000001152679414.png
.. |image2| image:: /_static/images/en-us_image_0000001198758917.png
