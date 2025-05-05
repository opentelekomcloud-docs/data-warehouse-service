:original_name: DWS_DS_023.html

.. _DWS_DS_023:

Sequence Management
===================

Creating a Sequence
-------------------

#. In the **Object Browser** pane, right-click **Sequences** under the particular schema where you want to create the sequence and select **Create Sequence**. The **Create New Sequence** dialog box is displayed.

   |image1|

#. Provide information to create a sequence:

   a. Enter a name in the **Sequence Name** field.

   .. note::

      Select the **Case** check box to retain the capitalization of the text entered in **Sequence Name** field. For example, if the entered sequence name is **Employee**, then the sequence name is created as "Employee".

   b. Enter the minimum value in the **Minimum Value** field.
   c. Enter the increase step value in the **Increment By** field.
   d. Enter maximum value in the **Maximum Value** field.

      .. note::

         The maximum and minimum value should be between -9,223,372,036,854,775,808 and 9,223,372,036,854,775,807.

   e. Enter the start value of the sequence in **Start Value** field.
   f. Enter the cache information in **Cache** field. The cache value denotes the number of sequences stored in the memory for quick access.
   g. Select the **Cycle** field to recycle sequences after the number of sequences reaches either the maximum or minimum value.

      .. note::

         The schema name auto-populates in the **Schema** field.

   h. Select the table from the **Table** drop-down list.
   i. Select the column from the **Column** drop-down list.

#. Click **Finish**.

   The status bar displays the status of the completed operation.

   .. note::

      On the **SQL Preview** tab, you can view the SQL query automatically generated for the inputs provided.

Support for Sequence DDL
------------------------

Data Studio allows you to display sequence DDL and export sequence DDL, including **Show DDL**, **Export DDL**, and **Export DDL and Data**.

In the **Object Browser** pane, right-click the selected table.

To check DDL statement, choose **Show DDL**.

To export DDL statements, choose **Export DDL**.

To export DDL and SELECT statements, choose **Export DDL and Data**

The procedure is shown in the following figure.

|image2|

.. note::

   Only the sequence owner, system administrator, or a user who has the select permission on the sequence can perform the preceding operations.

Granting/Revoking a Privilege
-----------------------------

#. Right-click selected sequence and select **Grant/Revoke**. The **Grant/Revoke** dialog box is displayed.
#. Select the objects to grant/revoke privilege from the **Object Selection** tab and click **Next**.
#. Select the **role** from the Role drop-down list in the **Privilege Selection** tab. Select **Grant/Revoke** in the **Privilege Selection** tab.
#. On the **SQL Preview** tab, you can check the automatically generated SQL query.
#. Click **Finish**.

Dropping a Sequence
-------------------

Right-click the selected sequence and select **Drop Sequence**. The **Drop Sequence** dialog box is displayed. Click **Yes** to drop the sequence.

Right-click the selected sequence and select **Drop Sequence Cascade**.

.. |image1| image:: /_static/images/en-us_image_0000001813599152.png
.. |image2| image:: /_static/images/en-us_image_0000001813439364.png
