:original_name: DWS_DS_74.html

.. _DWS_DS_74:

Managing Constraints
====================

Creating, dropping, and renaming constraints for a created table.

.. note::

   Constraints cannot be added to column-store tables.

Creating a Constraint
---------------------

#. Right-click the selected constraint of the table and select **Create constraint**.

   The **Add New Constraint** dialog box is displayed prompting you to add information about the new constraint.

   There are three options of constraint type: CHECK, PRIMARY KEY, and UNIQUE. For details, see :ref:`Defining Table Constraints <en-us_topic_0000001860199097__en-us_topic_0185264992_section440110125279>`.

   |image1|

#. Enter the **Constraint Name**, **Check Expression**, and click **Add**. You can view the added constraint in the corresponding table.

   Data Studio displays the status of the operation in the status bar.

   .. note::

      The status bar will show the name of the constraint if it has been provided in the **Constraint Name** field, or else the constraint name will not be displayed as it is created by database server.

Renaming a Constraint
---------------------

Follow the steps below to rename a constraint:

#. Right-click the selected constraint and select **Rename Constraint**.

   The **Rename Constraint** dialog box is displayed prompting you to provide the new name.

#. Enter the constraint name and click **OK**. Data Studio displays the status of the operation in the status bar.

Dropping a Constraint
---------------------

Follow the steps below to drop a constraint:

#. Right-click the selected constraint and select **Drop Constraint**.

   The **Drop Constraint** dialog box is displayed.

#. Click **OK** to complete the operation successfully. Data Studio displays the status of the operation in the status bar.

.. |image1| image:: /_static/images/en-us_image_0000001813599132.png
