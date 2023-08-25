:original_name: DWS_DS_111.html

.. _DWS_DS_111:

Working with Views
==================

Views can be created to restrict access to specific rows or columns of a table. A view can be created from one or more tables and is determined by the query used to create the view.

You can perform the following operations on an existing view:

-  :ref:`Exporting the View DDL <en-us_topic_0000001233800703__en-us_topic_0185264932_section1062545514243>`
-  :ref:`Dropping a View <en-us_topic_0000001233800703__en-us_topic_0185264932_section2626755202419>`
-  :ref:`Dropping a View Cascade <en-us_topic_0000001233800703__en-us_topic_0185264932_section8627185518244>`
-  :ref:`Renaming a View <en-us_topic_0000001233800703__en-us_topic_0185264932_section3628195562418>`
-  :ref:`Setting the Schema for a View <en-us_topic_0000001233800703__en-us_topic_0185264932_section5630655122413>`
-  :ref:`Viewing the Show DDL <en-us_topic_0000001233800703__en-us_topic_0185264932_section156311655172418>`
-  :ref:`Setting the Default Value for the View Column <en-us_topic_0000001233800703__en-us_topic_0185264932_section1363213551240>`
-  :ref:`Viewing the Properties of a View <en-us_topic_0000001233800703__en-us_topic_0185264932_section230303218508>`
-  :ref:`Granting/Revoking a Privilege <en-us_topic_0000001233800703__en-us_topic_0185264932_section203211220143514>`

.. _en-us_topic_0000001233800703__en-us_topic_0185264932_section1062545514243:

Exporting the View DDL
----------------------

Follow the steps below to export view the DDL:

#. Right-click the selected view and select **Export DDL**.

   The **Data Studio Security Disclaimer** dialog box is displayed.

#. Click **OK**.

   The **Save As** dialog box is displayed.

#. In the **Save As** dialog box, select the location to save the DDL and click **Save**. The status bar displays the progress of the operation.

   .. note::

      -  To cancel the export operation, double-click the status to open the **Progress View** tab and click |image1|.
      -  The exported file name will not be the same as the view name, if the view name contains characters which are not supported by Windows.
      -  Multiple objects can be selected to export the view DDL. Refer to :ref:`Batch Export <en-us_topic_0000001188681066__en-us_topic_0185264547_li1037472864716>` for the list of the objects that are not supported for exporting view DDL.

   The **Export** message and status bar display the status of the completed operation.

   ================= ============= ======================
   Database Encoding File Encoding Supports Exporting DDL
   ================= ============= ======================
   UTF-8             UTF-8         Yes
   \                 GBK           Yes
   \                 LATIN1        Yes
   GBK               GBK           Yes
   \                 UTF-8         Yes
   \                 LATIN1        No
   LATIN1            LATIN1        Yes
   \                 GBK           No
   \                 UTF-8         Yes
   ================= ============= ======================

.. _en-us_topic_0000001233800703__en-us_topic_0185264932_section2626755202419:

Dropping a View
---------------

Individual or batch dropping can be performed on views. Refer to :ref:`Dropping a Batch of Objects <dws_ds_133>` for batch dropping.

Follow the steps below to drop the view:

#. Right-click the selected view and select **Drop View**.

   The **Drop View** dialog box is displayed.

#. Click **Yes** to drop the view.

   The status bar displays the status of the completed operation.

.. _en-us_topic_0000001233800703__en-us_topic_0185264932_section8627185518244:

Dropping a View Cascade
-----------------------

Follow the steps below to drop a view and its dependent database objects:

#. Right-click the selected view and select **Drop View Cascade**.

   The **Drop View** dialog box is displayed.

#. Click **Yes** to drop the view and its dependent database objects.

   The status bar displays the status of the completed operation.

.. _en-us_topic_0000001233800703__en-us_topic_0185264932_section3628195562418:

Renaming a View
---------------

Follow the steps below to rename a view:

#. Right-click the selected view and select **Rename View**.

   The **Rename View** dialog box is displayed.

#. Enter the required name for the view and click **OK**. You can view the renamed view in the **Object Browser**.

   The status bar displays the status of the completed operation.

.. _en-us_topic_0000001233800703__en-us_topic_0185264932_section5630655122413:

Setting the Schema for a View
-----------------------------

Follow the steps below to set the schema for a view:

#. Right-click the selected view and select **Set Schema**.

   The **Set Schema** dialog box is displayed.

#. Select the required schema from the drop-down list and click **OK**.

   The status bar displays the status of the completed operation.

   If the required schema contains a view with the same name as the current view, then Data Studio does not allow setting the schema for the view.

.. _en-us_topic_0000001233800703__en-us_topic_0185264932_section156311655172418:

Viewing the DDL
---------------

Follow the steps below to view the DDL of the view:

#. Right-click the selected view and select **Show DDL**.

   The DDL is displayed in a new **SQL Terminal** tab. You must refresh the **Object Browser** to view the latest DDL.

.. _en-us_topic_0000001233800703__en-us_topic_0185264932_section1363213551240:

Setting the Default Value for the View Column
---------------------------------------------

Follow the steps below to set the default value for a column in the view:

#. Right-click the selected column name under the view and select **Set View Column Default Value**.

   A dialog box with the current default value (if it is set) is displayed which prompts you to provide the default value.

#. Enter the value and click **OK**.

   Data Studio displays the status of the operation in the status bar.

.. _en-us_topic_0000001233800703__en-us_topic_0185264932_section230303218508:

Viewing the Properties of a View
--------------------------------

Follow the steps below to view the properties of the View:

#. Right-click the selected View and select **Properties**.

   The properties (General and Columns) of the selected View is displayed in different tabs.

   .. note::

      If the property of a View is modified that is already opened, then refresh and open the properties of the View again to view the updated information on the same opened window.

.. _en-us_topic_0000001233800703__en-us_topic_0185264932_section203211220143514:

Granting/Revoking a Privilege
-----------------------------

Follow the steps below to grant/revoke a privilege:

#. Right-click the selected view and select **Grant/Revoke**.

   The **Grant/Revoke** dialog box is displayed.

#. Refer to :ref:`Granting/Revoking a Privilege <dws_ds_110>` to grant/revoke privilege.

.. |image1| image:: /_static/images/en-us_image_0000001188202722.jpg
