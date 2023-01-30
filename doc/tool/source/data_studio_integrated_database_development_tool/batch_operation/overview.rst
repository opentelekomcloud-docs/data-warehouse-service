:original_name: DWS_DS_132.html

.. _DWS_DS_132:

Overview
========

You can view accessible database objects in the navigation tree in **Object Browser**. Schema are displayed under databases, and tables are displayed under schemas.

**Object Browser** displays only the objects that meet the following minimum permission requirements of the current user.

================== =======================================
Object Type        Permissions displayed in Object Browser
================== =======================================
Database           Connect
Schema             Use
Table              Select
Column             Select
Sequence           Use
Function/Procedure Execute
================== =======================================

The child objects of the objects accessible to you do not need to be displayed in **Object Browser**. For example, if you have the permission to access a table but does not have the permission to access a column in the table, **Object Browser** only displays the columns you can access. If access to an object is revoked during an operation on the object, an error message will be displayed, indicating that you do not have permissions to perform the operation. After you refresh **Object Browser**, the object will not be displayed.

The following objects can be displayed in the navigation tree:

-  Schemas
-  Functions/Procedures
-  Tables
-  Sequences
-  Views
-  Columns, constraints, and indexes

All default created schemas, except for the **public** schema, are grouped under **Catalogs**. User schemas are displayed under their databases in **Schemas**.

.. note::

   The filter option in **Object Browser** opens a new tab, where you can specify the search scope. Press **Enter** to start the search. **Object Browser** also provides a search bar. You can search for an object by name. In an expanded navigation tree, only the objects that match the filter criteria are displayed.

   In a collapsed navigation tree, the filtering rule takes effect when a node is expanded.
