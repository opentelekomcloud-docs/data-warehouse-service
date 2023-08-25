:original_name: DWS_DS_23.html

.. _DWS_DS_23:

Edit
====

The **Edit** menu contains clipboard, **Format**, **Find** **and Replace**, and **Search Objects** operations to use in the **PL/SQL Viewer** and **SQL Terminal** tab. Press **Alt**\ +\ **E** to open the **Edit** menu.

+-------------------------+--------------+------------------------------------------------------------+
| Function                | Shortcut Key | Description                                                |
+=========================+==============+============================================================+
| Cut                     | Ctrl+X       | Cuts the selected text                                     |
+-------------------------+--------------+------------------------------------------------------------+
| Copy                    | Ctrl+C       | Copies the selected text or qualified object name          |
+-------------------------+--------------+------------------------------------------------------------+
| Paste                   | Ctrl+V       | Pastes the selected text or qualified object name          |
+-------------------------+--------------+------------------------------------------------------------+
| Format                  | Ctrl+Shift+F | Formats all SQL statements and functions/procedures.       |
+-------------------------+--------------+------------------------------------------------------------+
| Select All              | Ctrl+A       | Selects all the text in SQL Terminal                       |
+-------------------------+--------------+------------------------------------------------------------+
| Find and Replace        | Ctrl+F       | Finds and replaces text in SQL Terminal                    |
+-------------------------+--------------+------------------------------------------------------------+
| Search Objects          | Ctrl+Shift+S | Searches for objects within a connected database.          |
+-------------------------+--------------+------------------------------------------------------------+
| Undo                    | Ctrl+Z       | Undoes the previous operation                              |
+-------------------------+--------------+------------------------------------------------------------+
| Redo                    | Ctrl+Y       | Redoes the previous operation                              |
+-------------------------+--------------+------------------------------------------------------------+
| UPPERCASE               | Ctrl+Shift+U | Changes the case of the selected text to uppercase         |
+-------------------------+--------------+------------------------------------------------------------+
| lowercase               | Ctrl+Shift+L | Changes the case of the selected text to lowercase         |
+-------------------------+--------------+------------------------------------------------------------+
| Go To Line              | Ctrl+G       | Skips to a specific line in the Terminal or PL/SQL Viewer  |
+-------------------------+--------------+------------------------------------------------------------+
| Comment/Uncomment Lines | Ctrl+/       | Comments/Uncomments each selected line                     |
+-------------------------+--------------+------------------------------------------------------------+
| Comment/Uncomment Block | Ctrl+Shift+/ | Comments/Uncomments all selected lines or a selected block |
+-------------------------+--------------+------------------------------------------------------------+

Copy
----

Copy can also be used to copy objects from Object Browser.

The format of copied object name is:

.. table:: **Table 1**

   +------------------------+-------------------------------------------------------+
   | Object Type            | Copied Format                                         |
   +========================+=======================================================+
   | Functions/Procedures   | schema.object name(parameter name parameter type,...) |
   +------------------------+-------------------------------------------------------+
   | Databases              | object name                                           |
   +------------------------+-------------------------------------------------------+
   | Schemas                | object name                                           |
   +------------------------+-------------------------------------------------------+
   | Columns                | object name                                           |
   +------------------------+-------------------------------------------------------+
   | Constraints            | object name                                           |
   +------------------------+-------------------------------------------------------+
   | Partition names        | object name                                           |
   +------------------------+-------------------------------------------------------+
   | All other object types | schema.object name                                    |
   +------------------------+-------------------------------------------------------+

Search Objects
--------------

The Searching Objects option allows you to search for object(s) from the Object Browser based on the search criteria entered. The search operation can be executed either from **Edit > Search Objects** menu or by clicking |image1| from the Object Browser toolbar. The result of search displays tree structure similar to Object Browser. Right-click operations except for **Refresh** can be performed on the search result objects. Modified objects as a result of drop, set schema, rename, and so on can be viewed only from the main Object Browser after refresh. Right-click options on group names (tables, schema, views and so on) are not allowed on search result objects. Only objects to which you have access can be searched. Objects that you do not have access do not appear in the **Search Scope**.

.. note::

   Newly added objects can be viewed in the **Search** window by clicking the refresh option at the end of the object type.

|image2|

Supported Search Options:

+-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Search Options                    | Search Behavior                                                                                                                                                                                                                                                              |
+===================================+==============================================================================================================================================================================================================================================================================+
| Contains                          | A search text which contains the searched characters will be displayed.                                                                                                                                                                                                      |
+-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Starts With                       | A search text which starts with the searched character will be displayed.                                                                                                                                                                                                    |
+-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Exact Word                        | A search text which matches exactly with searched characters will be displayed.                                                                                                                                                                                              |
+-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Regular Expression                | A search text with regular expression searches for similar pattern in Object Browser that fulfills the search condition. Select Regular Expression from **Search Criteria** drop-down to perform this search. For more information refer to POSIX Regular Expressions rules. |
|                                   |                                                                                                                                                                                                                                                                              |
|                                   | **Example:**                                                                                                                                                                                                                                                                 |
|                                   |                                                                                                                                                                                                                                                                              |
|                                   | -  ^a, this displays all objects that start with the letter a.                                                                                                                                                                                                               |
|                                   | -  ^[^A-Za-z]+$, this displays all objects that do not have alphabets in them.                                                                                                                                                                                               |
|                                   | -  ^[^0-9]+$, this displays all objects that do not have numbers in them.                                                                                                                                                                                                    |
|                                   | -  ^[a-t][^r-z]+$, this displays all objects whose name starts between a and t and excludes those that have characters between r and z in them.                                                                                                                              |
|                                   | -  ^e.*a$, this displays all objects that start with the letter e and end with letter a.                                                                                                                                                                                     |
|                                   | -  ^[a-z]+$ and select **Match Case**, this displays all objects that contain only alphabets in lowercase.                                                                                                                                                                   |
|                                   | -  ^[A-Z]+$ and select **Match Case**, this displays all objects that contain only alphabets in uppercase.                                                                                                                                                                   |
|                                   | -  ^[A-Za-z]+$ and select **Match Case**, this displays all objects that contain only alphabets in lowercase and uppercase.                                                                                                                                                  |
|                                   | -  ^[A-Za-z0-9]+$ and select **Match Case**, this displays all objects that contain only alphabets in lowercase, uppercase and numbers.                                                                                                                                      |
|                                   | -  ^".*"$, this displays all objects that are within in quotes.                                                                                                                                                                                                              |
+-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Underscore and % search:

+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Search Value                      | Search Behavior                                                                                                                                                                                    |
+===================================+====================================================================================================================================================================================================+
| \_                                | A search text with \_ (underscore) in it considers the underscore as a wildcard of single character. This does not apply to regular expression, starts with and exact word search.                 |
|                                   |                                                                                                                                                                                                    |
|                                   | **Example:**                                                                                                                                                                                       |
|                                   |                                                                                                                                                                                                    |
|                                   | -  \_ed, this displays all objects that starts with any single character followed by "ed".                                                                                                         |
|                                   | -  D_t_e, this displays all objects that have character "d", followed by any single character, followed by character "t", followed by any single character, and followed by character "e".         |
+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| %                                 | A search text with % (percentage) in it considers the percentage as a wildcard of multiple characters. This does not apply to regular expression, starts with and exact word search.               |
|                                   |                                                                                                                                                                                                    |
|                                   | **Example:**                                                                                                                                                                                       |
|                                   |                                                                                                                                                                                                    |
|                                   | -  %ed, this displays all objects that have "ed" pattern in it.                                                                                                                                    |
|                                   | -  D%t%e, this displays all objects that have character "d", followed by any number of characters, followed by character "t", followed by any number of characters, and followed by character "e". |
+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Match case runs the search to match with the search text case.

.. |image1| image:: /_static/images/en-us_image_0000001233800803.png
.. |image2| image:: /_static/images/en-us_image_0000001188362658.png
