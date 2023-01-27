:original_name: DWS_DS_105.html

.. _DWS_DS_105:

Creating Sequence
=================

Follow the steps below to create a sequence:

#. In the **Object Browser** pane, right-click **Sequences** under the particular schema where you want to create the sequence and select **Create Sequence**.

   The **Create New Sequence** dialog box is displayed.

#. Provide information to create a sequence:

   a. Enter a name in the **Sequence Name** field.

      .. note::

         Select the **Case** check box to retain the capitalization of the text entered in **Sequence Name** field. For example, if the sequence name entered is "Employee", then the sequence name is created as "Employee".

   b. Enter the minimum value in the **Minimum Value** field.
   c. Enter the increase step value in the **Increment By** field.
   d. Enter maximum value in the **Maximum Value** field.

      .. note::

         The minimum and maximum value should be between -9223372036854775808 and 9223372036854775807.

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

      In the **SQL Preview** tab, you can view the SQL query automatically generated for the inputs provided.
