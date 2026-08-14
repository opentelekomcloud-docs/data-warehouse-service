:original_name: dws_04_0717.html

.. _dws_04_0717:

GS_VIEW_INVALID
===============

**GS_VIEW_INVALID** queries all unavailable views visible to the current user.

If the basic table, function, or synonym on which the view depends is abnormal, the **validtype** column of the view is displayed as **invalid**. If the system object on which the view depends changes during an upgrade, the **validtype** column of the view is displayed as **invalidInUpgrade**.

.. table:: **Table 1** GS_VIEW_INVALID columns

   +-------------------+--------------------------+-------------------------------------------------------------------------------------------------------+
   | Column            | Type                     | Description                                                                                           |
   +===================+==========================+=======================================================================================================+
   | OID               | OID                      | OID of the view                                                                                       |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------------------+
   | schemaname        | Name                     | View space name                                                                                       |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------------------+
   | viewname          | Name                     | Name of the view                                                                                      |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------------------+
   | viewowner         | Name                     | Owner of the view                                                                                     |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------------------+
   | definition        | Text                     | Definition of the view                                                                                |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------------------+
   | validtype         | Text                     | View validity flag                                                                                    |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------------------+
   | last_invalid_time | Timestamp with time zone | Time when a view is invalid. This column is available only in clusters of version 9.1.0.200 or later. |
   +-------------------+--------------------------+-------------------------------------------------------------------------------------------------------+
