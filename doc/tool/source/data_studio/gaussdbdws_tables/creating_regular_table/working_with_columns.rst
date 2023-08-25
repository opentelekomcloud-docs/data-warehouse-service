:original_name: DWS_DS_73.html

.. _DWS_DS_73:

Working with Columns
====================

After creating a table, you can add new columns in that table. You can also perform the following operations on the existing column only for a Regular table:

-  :ref:`Creating a New Column <en-us_topic_0000001188202570__en-us_topic_0185264853_section81966120380>`
-  :ref:`Renaming a Column <en-us_topic_0000001188202570__en-us_topic_0185264853_section619781253814>`
-  :ref:`Toggle Not Null <en-us_topic_0000001188202570__en-us_topic_0185264853_section12198131211384>`
-  :ref:`Dropping a Column <en-us_topic_0000001188202570__en-us_topic_0185264853_section719971210384>`
-  :ref:`Setting the Default Value of a Column <en-us_topic_0000001188202570__en-us_topic_0185264853_section319911210384>`
-  :ref:`Changing the Data Type <en-us_topic_0000001188202570__en-us_topic_0185264853_section1200161218389>`

.. _en-us_topic_0000001188202570__en-us_topic_0185264853_section81966120380:

Creating a New Column
---------------------

Follow the steps below to add a new column to the existing table:

#. Right-click **Columns** and select **Create column**.

   The **Add New Column** dialog box is displayed prompting you to add information about the new column.

#. Enter the details and click **Add**. You can view the added column in the corresponding table.

   Data Studio displays the status of the operation in the status bar.

.. _en-us_topic_0000001188202570__en-us_topic_0185264853_section619781253814:

Renaming a Column
-----------------

Follow the steps below to rename a column:

#. Right-click the selected column and select **Rename Column**.

   A **Rename Column** dialog box is displayed prompting you to provide the new name.

#. Enter the name and click **OK**. Data Studio displays the status of the operation in the status bar.

.. _en-us_topic_0000001188202570__en-us_topic_0185264853_section12198131211384:

Toggle Not Null
---------------

Follow the steps below to set or reset the Not Null option:

#. Right-click the selected column and select **Toggle Not Null**.

   A **Toggle Not Null Property** dialog box is displayed prompting you to set or reset the Not Null option.

#. In the confirmation dialog box, click **OK** to complete the operation successfully. Data Studio displays the status of the operation in the status bar.

.. _en-us_topic_0000001188202570__en-us_topic_0185264853_section719971210384:

Dropping a Column
-----------------

Follow the steps below to drop a column:

#. Right-click the selected column and select **Drop Column**. This operation deletes the column from the table.

   A **Drop Column** dialog box is displayed.

#. Click **OK** to complete the operation successfully. Data Studio displays the status of the operation in the status bar.

.. _en-us_topic_0000001188202570__en-us_topic_0185264853_section319911210384:

Setting the Default Value of a Column
-------------------------------------

Follow the steps below to set the default value for a column:

#. Right-click the selected column and select **Set Column Default Value**.

   A dialog box with the current default value (if it is set) is displayed, prompting you to provide the default value.

#. Enter the value and click **OK**. Data Studio displays the status of the operation in the status bar.

.. _en-us_topic_0000001188202570__en-us_topic_0185264853_section1200161218389:

Changing the Data Type
----------------------

Follow the steps below to change the data type of a column:

#. Right-click the selected column and select **Change Data Type**.

   **Change Data Type** dialog box is displayed.

   .. note::

      The existing data type will show as Unknown while modifying complex data types.

#. Select the **Data type Schema** and **Data Type**. If the **Precision/Size** spin box is enabled, enter the required details and click **OK**. Data Studio displays the status of the operation in the status bar.
