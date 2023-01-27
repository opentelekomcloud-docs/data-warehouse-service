:original_name: DWS_DS_90.html

.. _DWS_DS_90:

Setting the Schema
==================

Follow the steps below to set a schema:

#. Right-click the selected table and select **Set Schema**.

   The **Set Schema** dialog box is displayed that prompts you to select the new schema for the selected table.

#. Select the schema name from the drop-down list and click **OK**. The selected table will be moved to the new schema.

   The status bar displays the status of the completed operation.

   .. note::

      -  This operation is not supported for Partition ORC tables.
      -  If the required schema contains a table with the same name as the current table, then Data Studio does not allow setting the schema for the table.
