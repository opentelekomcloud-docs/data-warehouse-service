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

Precautions
-----------

After a cluster is migrated, to monitor alarms of the new cluster, change the cluster bound to the alarm rule to the new cluster. You can also create an alarm rule for the new cluster.

Viewing Alarm Rules
-------------------

#. Log in to the GaussDB(DWS) console.
#. In the navigation tree on the left, choose **Monitoring** > **Alarm**.
#. Click **View Alarm Rule** in the upper left corner. On the page that is displayed, you can see the threshold alarm rules of database cluster monitoring metrics, as shown in the following figure.

Modifying an Alarm Rule
-----------------------

#. Log in to the GaussDB(DWS) console.

#. In the navigation tree on the left, choose **Monitoring** > **Alarm**.

#. Click **View Alarm Rule** in the upper left corner.

#. On the **Alarm Rules** page that is displayed, click **Modify** in the **Operation** column of the target alarm rule.

   .. note::

      -  Read-only users (with the DWS ReadOnlyAccess permission) cannot modify alarm rules.
      -  Default rules have limited options that you can modify, such as cluster binding, alarm policy trigger threshold, data capture interval, and alarm suppression conditions. However, custom rules offer more flexibility, allowing you to modify all options.

   .. _en-us_topic_0000002168065520__table5459418131114:

   .. table:: **Table 1** Alarm rule parameters

      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Parameter             | Description                                                                                                                                                                                       | Example Value         |
      +=======================+===================================================================================================================================================================================================+=======================+
      | Alarm Rule            | The rule name contains 6 to 64 characters and must start with a non-digit character.                                                                                                              | ``-``                 |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Description           | User-defined description, which contains a maximum of 490 characters.                                                                                                                             | ``-``                 |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Associated Cluster    | You can select a cluster of the current tenant from the drop-down list box as the monitoring cluster of the alarm module.                                                                         | All                   |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Triggered Policies    | Policy triggering relationships are as follows:                                                                                                                                                   | Independent           |
      |                       |                                                                                                                                                                                                   |                       |
      |                       | -  **Independent**: Alarm policies are triggered independently of each other.                                                                                                                     |                       |
      |                       | -  **Priority**: Alarm policies are triggered by priority. Policies of a lower priority will be automatically triggered after those of a higher priority.                                         |                       |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
      | Alarm Policy          | The alarm policies are as follows:                                                                                                                                                                | ``-``                 |
      |                       |                                                                                                                                                                                                   |                       |
      |                       | -  **Metric**: GaussDB(DWS) monitoring metric, which is the data source used by the alarm engine for threshold determination.                                                                     |                       |
      |                       | -  **Alarm Object**: databases in the selected cluster and schemas in the selected databases.                                                                                                     |                       |
      |                       | -  **Trigger**: calculation rule for threshold determination of a monitoring metric. Select the average value within a period of time of a metric to reduce the probability of alarm oscillation. |                       |
      |                       | -  **Constraint**: suppresses the repeated triggering and clearance of alarms of the same type within the specified period.                                                                       |                       |
      |                       | -  **Alarm Severity**: includes **Urgent**, **Important**, **Minor**, and **Prompt**.                                                                                                             |                       |
      +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

#. Confirm the information and click **OK**.

Creating an Alarm Rule
----------------------

#. Log in to the GaussDB(DWS) console.
#. In the navigation tree on the left, choose **Monitoring** > **Alarm**.
#. Click **View Alarm Rule** in the upper left corner.
#. Click **Create Alarm Rule** in the upper right corner. You can configure items, such as the alarm rule name, rule description, associated cluster, and alarm policy. For details, see :ref:`Table 1 <en-us_topic_0000002168065520__table5459418131114>`.

   .. note::

      Currently, only alarm rules of schema usage metrics can be created on GaussDB(DWS).
