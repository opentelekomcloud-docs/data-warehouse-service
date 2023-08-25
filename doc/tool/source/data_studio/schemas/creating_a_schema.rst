:original_name: DWS_DS_50.html

.. _DWS_DS_50:

Creating a Schema
=================

In relational database technology, schemas provide a logical classification of objects in the database. Some of the objects that a schema may contain include functions/procedures, tables, sequences, views, and indexes.

Follow the steps below to define a schema:

#. In the **Object Browser** pane, right-click the selected **Schemas** group and select **Create Schema**.

   .. note::

      Only refresh can be performed on **Catalogs** group.

#. Enter the schema name and click **OK**. You can create the schema only if the database connection is active.

   You can view the new schema in the **Object Browser** pane.

   The status bar displays the status of the completed operation.

You can perform the following actions on a schema:

-  Refreshing a schema - To refresh a schema, right-click the selected **Schema Name** and select **Refresh Schema**. All the objects under that schema will be refreshed.
-  Renaming a schema (Refer to :ref:`Renaming a Schema <dws_ds_53>` for more details)
-  Dropping a schema (Refer to :ref:`Dropping a Schema <dws_ds_55>` for more details)
-  Exporting DDL (Refer to :ref:`Exporting Schema DDL <dws_ds_51>` for more details)
-  Exporting DDL and data (Refer to :ref:`Exporting Schema DDL and Data <dws_ds_52>` for more details)
-  Grant/Revoke privilege (Refer to :ref:`Granting/Revoking a Privilege <dws_ds_54>` for more details)

Displaying the Default Schema
-----------------------------

Data studio displays default schema of the user in the toolbar.

|image1|

When a create query without mentioning the schema name is executed from SQL Terminal, the corresponding objects are created under the default schema of the user.

When a select query is executed in SQL terminal without mentioning the schema name, the default schemas are searched to find these objects.

When Data Studio starts, the default schemas are set to **<username>**, public schemas in same priority.

If another schema is selected in the drop-down, the selected schema will be set as the default schema, overriding previous setting.

The selected schema is set as the default schema for all active connections of the database (selected in database list drop-down).

.. note::

   This feature is not available for OLTP database.

.. |image1| image:: /_static/images/en-us_image_0000001234042245.png
