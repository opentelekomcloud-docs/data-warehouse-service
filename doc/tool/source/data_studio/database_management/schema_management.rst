:original_name: DWS_DS_014.html

.. _DWS_DS_014:

Schema Management
=================

This section introduces how to use the database schema. All system schemas are grouped in **Catalogs**, and user schemas are grouped in **Schemas**.

Create Schema
-------------

#. In the **Object Browser** pane, right-click the selected **Schemas** group and select **Create Schema**.

   .. note::

      Only refresh can be performed on **Catalogs** group.

#. Enter a schema name and click **OK**. You can create the schema only if the database connection is active.

   The status bar displays the status of the completed operation.

   You can view the new schema in the **Object Browser** pane.

.. note::

   Data studio displays default schema of the user in the toolbar.

   |image1|

   -  When a CREATE query without mentioning the schema name is executed from SQL Terminal, the corresponding objects are created under the default schema of the user.
   -  When a SELECT query is executed in SQL terminal without mentioning the schema name, the objects are queried based on the default schema.
   -  When Data Studio starts, the default schemas are set to <username>. The public schemas have the same priority.
   -  If another schema is selected in the drop-down, the selected schema will be set as the default schema, overriding previous setting.
   -  The selected schema is set as the default schema for all active connections of the database (selected in database list drop-down).

Exporting Schema DDL and Data
-----------------------------

You can right-click **Export DDL** to export the DDL of functions/procedures, tables, sequences, and views of the schema. To export data, choose **Export DDL and Data**.

#. In the **Object Browser** pane, right-click the selected schema and select **Export DDL**.

   You need to customize the export path. To compress data, select **.zip**.

   |image2|

   You must select **I Agree** under the **Security Disclaimer**, then click **OK**. You can select **Do not show again**. After the disclaimer is disabled, it will not be displayed when you export DDL. For details, see :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>`.

#. Click **OK**. The operation progress is displayed on the status bar in the lower right corner.

   .. note::

      -  If the file name contains characters that are not supported by Windows, the file name will be different from the schema name.
      -  The Microsoft Visual C Runtime file (**msvcrt100.dll**) is required for this operation. For details, see :ref:`Troubleshooting <dws_ds_039>`.
      -  You can select multiple objects and export their DDL. :ref:`Batch Export <en-us_topic_0000001813598528__en-us_topic_0185264547_li1037472864716>` lists the objects whose DDL cannot be exported.

   The **Data Exported Successfully** dialog box and status bar display the status of the completed operation.

   .. table:: **Table 1** The supported DDL encoding formats

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

Renaming a Schema
-----------------

#. In the **Object Browser** pane, right-click the selected schema and select **Rename Schema**.

#. Enter a schema name and click **OK**.

   You can view the renamed schema in **Object Browser**.

   The status bar displays the status of the completed operation.

Granting/Revoking a Privilege
-----------------------------

#. Right-click the schema group and select **Grant/Revoke**.

   The **Grant/Revoke** dialog box is displayed.

#. Open the **Object Selection** tab to select the desired objects, and click **Next**.

#. Select the role from the **Role** drop-down list in the **Privilege Selection** tab. Select the permissions to be granted or revoked.

#. On the **SQL Preview** tab, you can check the automatically generated SQL query. If the result does not meet the expectation, return to the previous step until the result meets the expectation.

#. Click **Finish**.

Dropping a Schema
-----------------

#. In the **Object Browser** pane, right-click the selected schema and select **Drop Schema**. A confirmation dialog is displayed, prompting you to confirm the deletion.

#. Click **OK**. This action will remove the schema from **Object Browser**.

   A pop-up message and the status bar display the status of the completed operation.

Refreshing a Schema
-------------------

Right-click a schema name and choose **Refresh** to refresh all objects in the schema.

.. |image1| image:: /_static/images/en-us_image_0000001813439296.png
.. |image2| image:: /_static/images/en-us_image_0000001860199153.png
