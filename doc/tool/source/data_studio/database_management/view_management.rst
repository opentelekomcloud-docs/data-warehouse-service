:original_name: DWS_DS_022.html

.. _DWS_DS_022:

View Management
===============

Creating a View
---------------

Right-click the **Views** and select **Create View**. The DDL template for the view is displayed in the **SQL Terminal** tab.

|image1|

Granting/Revoking a Privilege
-----------------------------

#. Right-click the views group and select **Grant/Revoke**. The **Grant/Revoke** dialog box is displayed.
#. Select the objects to grant/revoke a privilege from the **Object Selection** tab and click **Next**.
#. Select a role from the **Role** drop-down list in the **Privilege Selection** tab. Select **Grant/Revoke** in the **Privilege Selection** tab.
#. On the **SQL Preview** tab, you can check the automatically generated for the inputs provided.

Viewing the DDL
---------------

Right-click the selected view and select **Show DDL**.

The DDL is displayed in a new **SQL Terminal** tab. You must refresh the **Object Browser** to view the latest DDL.

Exporting the View DDL
----------------------

Right-click the selected view and select **Export DDL**.

#. In the **Object Browser** pane, right-click the selected schema and select **Export DDL**.

   You need to customize the export path. To compress data, select **.zip**

   |image2|

   You must click **I agree** under **Security Disclaimer**, then click **OK**. You can disable the security disclaimer. After the disclaimer is disabled, it will not be displayed when you export the DDL. For details, see :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>`.

#. Click **OK**. The operation progress is displayed on the status bar in the lower right corner.

   The **Export** message and status bar display the status of the completed operation.

   .. note::

      -  To cancel the export operation, double-click the status to open the **Progress View** tab and click |image3|.
      -  If the view name contains characters that are not supported by Windows, the name of the exported file is different from the view name.

   ================= ============= ===========================
   Database Encoding File Encoding Support for Exporting a DDL
   ================= ============= ===========================
   UTF-8             UTF-8         Yes
   \                 GBK           Yes
   \                 LATIN1        Yes
   GBK               GBK           Yes
   \                 UTF-8         Yes
   \                 LATIN1        No
   LATIN1            LATIN1        Yes
   \                 GBK           No
   \                 UTF-8         Yes
   ================= ============= ===========================

Setting the Schema for a View
-----------------------------

#. Right-click the selected view and select **Set Schema**. The **Set Schema** dialog box is displayed.

   |image4|

#. Select the required schema from the drop-down list and click **OK**.

   The status bar displays the status of the completed operation.

   If the required schema contains a view with the same name as the current view, then Data Studio does not allow setting the schema for the view.

Viewing the Properties of a View
--------------------------------

Right-click the selected View and select **Properties**.

The properties (**General** and **Columns**) of the selected view is displayed in different tabs.

|image5|

|image6|

.. note::

   If you modify the properties of a view that is already opened, then refresh and open the properties of the view again to check the updated information on the same opened window.

Renaming a View
---------------

#. Right-click the selected view and select **Rename View**. The **Rename View** dialog box is displayed.

   |image7|

#. Enter the required name for the view and click **OK**. You can view the renamed view in the **Object Browser**.

   The status bar displays the status of the completed operation.

Dropping a View
---------------

Right-click the selected view and select **Drop View**. The **Drop View** dialog box is displayed. Click **Yes** to delete the view.

Right-click the selected view and select **Drop View Cascade**.

.. |image1| image:: /_static/images/en-us_image_0000001860318985.png
.. |image2| image:: /_static/images/en-us_image_0000001860199145.png
.. |image3| image:: /_static/images/en-us_image_0000001813439284.jpg
.. |image4| image:: /_static/images/en-us_image_0000001860318989.png
.. |image5| image:: /_static/images/en-us_image_0000001860199133.png
.. |image6| image:: /_static/images/en-us_image_0000001813599072.png
.. |image7| image:: /_static/images/en-us_image_0000001813599068.png
