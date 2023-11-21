:original_name: DWS_DS_29.html

.. _DWS_DS_29:

Data Studio Right-Click Menus
=============================

This section describes the right-click menus of Data Studio.

Object Browser Pane
-------------------

The following figure shows the **Object Browser** pane.

|image1|

Right-clicking the connection name allows you to select **Rename Connection**, **Edit Connection**, **Remove Connection**, and **Properties** along with **Refresh** options.

================= ============ =========================================
Menu Item         Shortcut Key Description
================= ============ =========================================
Rename Connection ``-``        Renames a connection.
Edit Connection   ``-``        Modifies connection details.
Remove Connection ``-``        Removes the existing database connection.
Properties        ``-``        Shows the details of a connection.
Refresh           F5           Refreshes a connection.
================= ============ =========================================

Right-clicking the **Databases** tab allows you to select **Create Database**, **Disconnect All**, and **Refresh** options.

+-----------------+--------------+---------------------------------------------------+
| Menu Item       | Shortcut Key | Description                                       |
+=================+==============+===================================================+
| Create Database | ``-``        | Creates a database of this connection.            |
+-----------------+--------------+---------------------------------------------------+
| Disconnect All  | ``-``        | Disconnects all the databases of this connection. |
+-----------------+--------------+---------------------------------------------------+
| Refresh         | F5           | Refreshes a database group.                       |
+-----------------+--------------+---------------------------------------------------+

Right-clicking the active database allows you to select **Disconnect from DB**, **Open Terminal**, **Properties**, and **Refresh** options.

================== ============ ======================================
Menu Item          Shortcut Key Description
================== ============ ======================================
Disconnect from DB Ctrl+Shift+D Disconnects from a database.
Open Terminal      Ctrl+T       Opens a terminal of this connection.
Properties         ``-``        Displays the properties of a database.
Refresh            F5           Refreshes a database.
================== ============ ======================================

Right-clicking the inactive database allows you to select **Connect to DB**, **Rename Database**, and **Drop Database** options.

=============== ============ =======================
Menu Item       Shortcut Key Description
=============== ============ =======================
Connect to DB   ``-``        Connects to a database.
Rename Database ``-``        Renames a database.
Drop Database   ``-``        Drops a database.
=============== ============ =======================

Right-clicking the **Catalogs** tab allows you to select the **Refresh** option.

========= ============ ===============================
Menu Item Shortcut Key Description
========= ============ ===============================
Refresh   F5           Refreshes a function/procedure.
========= ============ ===============================

Right-clicking the **Schemas** tab allows you to select **Create Schema**, **Grant/Revoke**, and **Refresh** options.

+---------------+--------------+---------------------------------------------------+
| Menu Item     | Shortcut Key | Description                                       |
+===============+==============+===================================================+
| Create Schema | ``-``        | Creates a schema.                                 |
+---------------+--------------+---------------------------------------------------+
| Grant/Revoke  | ``-``        | Grants or revokes permissions for a schema group. |
+---------------+--------------+---------------------------------------------------+
| Refresh       | F5           | Refreshes a schema.                               |
+---------------+--------------+---------------------------------------------------+

Right-clicking the schema allows you to select **Export DDL**, **Export DDL and Data**, **Rename Schema**, **Drop Schema**, **Grant/Revoke**, and **Refresh** options.

+---------------------+--------------+---------------------------------------------+
| Menu Item           | Shortcut Key | Description                                 |
+=====================+==============+=============================================+
| Export DDL          | ``-``        | Exports DDL of a schema.                    |
+---------------------+--------------+---------------------------------------------+
| Export DDL and Data | ``-``        | Exports DDL and data of a schema.           |
+---------------------+--------------+---------------------------------------------+
| Rename Schema       | ``-``        | Renames a schema.                           |
+---------------------+--------------+---------------------------------------------+
| Drop Schema         | ``-``        | Drops a schema.                             |
+---------------------+--------------+---------------------------------------------+
| Grant/Revoke        | ``-``        | Grants or revokes permissions for a schema. |
+---------------------+--------------+---------------------------------------------+
| Refresh             | F5           | Refreshes a schema.                         |
+---------------------+--------------+---------------------------------------------+

Right-clicking **Functions/Procedures** allows you to select **Refresh** and **Create Function/Procedure** and **Grant/Revoke** options.

+-------------------------+--------------+---------------------------------------------------------+
| Menu Item               | Shortcut Key | Description                                             |
+=========================+==============+=========================================================+
| Create PL/SQL Function  | ``-``        | Creates a PL/SQL function.                              |
+-------------------------+--------------+---------------------------------------------------------+
| Create PL/SQL Procedure | ``-``        | Creates a PL/SQL procedure.                             |
+-------------------------+--------------+---------------------------------------------------------+
| Create SQL Function     | ``-``        | Creates an SQL function.                                |
+-------------------------+--------------+---------------------------------------------------------+
| Create C Function       | ``-``        | Creates a C function.                                   |
+-------------------------+--------------+---------------------------------------------------------+
| Grant/Revoke            | ``-``        | Grants or revokes permissions for a function/procedure. |
+-------------------------+--------------+---------------------------------------------------------+
| Refresh                 | F5           | Refreshes a function/procedure.                         |
+-------------------------+--------------+---------------------------------------------------------+

Right-clicking **Tables** allows you to select **Create table**, **Create partitioned table**, **Grant/Revoke**, and **Refresh** options.

+--------------------------+--------------+--------------------------------------------+
| Menu Item                | Shortcut Key | Description                                |
+==========================+==============+============================================+
| Create table             | ``-``        | Creates a common table.                    |
+--------------------------+--------------+--------------------------------------------+
| Create partitioned table | ``-``        | Creates a partitioned table.               |
+--------------------------+--------------+--------------------------------------------+
| Grant/Revoke             | ``-``        | Grants or revokes permissions for a table. |
+--------------------------+--------------+--------------------------------------------+
| Refresh                  | F5           | Refreshes a table.                         |
+--------------------------+--------------+--------------------------------------------+

Right-clicking **Views** allows you to select **Create View**, **Grant/Revoke**, and **Refresh** options.

============ ============ =====================================
Menu Item    Shortcut Key Description
============ ============ =====================================
Create View  ``-``        Creates a view.
Grant/Revoke ``-``        Grant/revokes permissions for a view.
Refresh      F5           Refreshes a view.
============ ============ =====================================

Right-clicking the **PL/SQL Viewer** tab allows you to select **Cut**, **Copy**, **Paste**, **Select All**, **Comment/Uncomment Lines**, **Comment/Uncomment Block**, **Compile**, **Execute**, **Add Variable To Monitor**, **Debug with Rollback**, and **Debug** options.

+-------------------------+------------------------+-------------------------------------------------------------------------------------+
| Right-Click Option      | Shortcut Key           | Description                                                                         |
+=========================+========================+=====================================================================================+
| Cut, Copy, Paste        | Ctrl+X, Ctrl+C, Ctrl+V | Specifies clipboard operations.                                                     |
+-------------------------+------------------------+-------------------------------------------------------------------------------------+
| Select All              | Ctrl+A                 | Selects options in the **PL/SQL Viewer** tab.                                       |
+-------------------------+------------------------+-------------------------------------------------------------------------------------+
| Comment/Uncomment Lines | ``-``                  | Comments or uncomments all selected rows.                                           |
+-------------------------+------------------------+-------------------------------------------------------------------------------------+
| Comment/Uncomment Block | ``-``                  | Comments or uncomments all selected rows or blocks.                                 |
+-------------------------+------------------------+-------------------------------------------------------------------------------------+
| Compile                 | ``-``                  | Compiles a function/procedure.                                                      |
+-------------------------+------------------------+-------------------------------------------------------------------------------------+
| Execute                 | ``-``                  | Executes a function/procedure.                                                      |
+-------------------------+------------------------+-------------------------------------------------------------------------------------+
| Add Variable To Monitor | ``-``                  | Adds variables to the monitor window.                                               |
+-------------------------+------------------------+-------------------------------------------------------------------------------------+
| Debug with Rollback     | ``-``                  | Debugs a function/procedure and rolls back changes after the debugging is complete. |
+-------------------------+------------------------+-------------------------------------------------------------------------------------+
| Debug                   | ``-``                  | Debugs a function/procedure.                                                        |
+-------------------------+------------------------+-------------------------------------------------------------------------------------+

Right-clicking the **SQL Terminal** tab allows you to select **Cut, Copy, Paste**, **Select All**, **Execute Statement**, **Open**, **Save**, **Find** **and Replace**, **Execution Plan**, **Comment/Uncomment**, **Save As**, **Format** , and **Cancel** options.

+-------------------------+------------------------+------------------------------------------------------------------------------+
| Right-Click Option      | Shortcut Key           | Description                                                                  |
+=========================+========================+==============================================================================+
| Cut, Copy, Paste        | Ctrl+X, Ctrl+C, Ctrl+V | Specifies clipboard operations.                                              |
+-------------------------+------------------------+------------------------------------------------------------------------------+
| Select All              | ``-``                  | Selects all text.                                                            |
+-------------------------+------------------------+------------------------------------------------------------------------------+
| Execute Statement       | ``-``                  | Executes a query.                                                            |
+-------------------------+------------------------+------------------------------------------------------------------------------+
| Open                    | ``-``                  | Opens a file.                                                                |
+-------------------------+------------------------+------------------------------------------------------------------------------+
| Save                    | ``-``                  | Saves a query.                                                               |
+-------------------------+------------------------+------------------------------------------------------------------------------+
| Find and Replace        | ``-``                  | Finds and replaces text in the **SQL Terminal**\ tab.                        |
+-------------------------+------------------------+------------------------------------------------------------------------------+
| Execution Plan          | ``-``                  | Executes a query.                                                            |
+-------------------------+------------------------+------------------------------------------------------------------------------+
| Comment/Uncomment Lines | Ctrl+/                 | Comments or uncomments all selected rows.                                    |
+-------------------------+------------------------+------------------------------------------------------------------------------+
| Comment/Uncomment Block | Ctrl+Shift+/           | Comments or uncomments all selected rows or blocks.                          |
+-------------------------+------------------------+------------------------------------------------------------------------------+
| Cancel                  | ``-``                  | Cancels the execution.                                                       |
+-------------------------+------------------------+------------------------------------------------------------------------------+
| Save As                 | CTRL+ALT+S             | Saves the query to a new file.                                               |
+-------------------------+------------------------+------------------------------------------------------------------------------+
| Format                  | CTRL+SHIFT+F           | Formats the selected SQL statements using the rules configured in the query. |
+-------------------------+------------------------+------------------------------------------------------------------------------+

Right-clicking the **Messages** tab allows you to select **Copy**, **Select All**, and **Clear** options.

================== ============ =================
Right-Click Option Shortcut Key Description
================== ============ =================
Copy               Ctrl+C       Copies the text.
Select All         Ctrl+A       Selects all text.
Clear              ``-``        Clears the text.
================== ============ =================

.. |image1| image:: /_static/images/en-us_image_0000001234042255.jpg
