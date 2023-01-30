:original_name: DWS_DS_23.html

.. _DWS_DS_23:

Edit
====

The **Edit** menu contains clipboard, **Format**, **Find** **and Replace**, and **Search Objects** operations to use in the **PL/SQL Viewer** and **SQL Terminal** tab. Press **Alt+E** to open the **Edit** menu.

+--------------------------+--------------+-----------------------------------------------------------------------+
| Function                 | Shortcut Key | Description                                                           |
+==========================+==============+=======================================================================+
| Cut                      | Ctrl+X       | Cuts the selected text.                                               |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Copy                     | Ctrl+C       | Copies the selected text or object name.                              |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Paste                    | Ctrl+V       | Pastes the selected text or object name.                              |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Format                   | Ctrl+Shift+F | Formats all SQL statements and functions/procedures.                  |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Select all               | Ctrl+A       | Selects all the text in **SQL Terminal**.                             |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Find and replace         | Ctrl+F       | Finds and replaces text in **SQL Terminal**.                          |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Search for objects       | Ctrl+Shift+S | Searches for objects within a connected database.                     |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Undo                     | Ctrl+Z       | Undoes the previous operation.                                        |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Redo                     | Ctrl+Y       | Redoes the previous operation.                                        |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Uppercase                | Ctrl+Shift+U | Changes the selected text to uppercase.                               |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Lowercase                | Ctrl+Shift+L | Changes the selected text to lowercase.                               |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Go to row                | Ctrl+G       | Redirects to a specific row in **SQL Terminal** or **PL/SQL Viewer**. |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Comment/Uncomment lines  | Ctrl+/       | Comments or uncomments all selected rows.                             |
+--------------------------+--------------+-----------------------------------------------------------------------+
| Comment/Uncomment blocks | Ctrl+Shift+/ | Comments or uncomments all selected rows or blocks.                   |
+--------------------------+--------------+-----------------------------------------------------------------------+

Search Objects
--------------

You can choose **Search Objects** to search for objects from **Object Browser** based on the search criteria. Specifically, you can choose **Edit** > **Search Objects** or click |image1| in the **Object Browser** toolbar. The search result is displayed in a tree structure, similar to that in **Object Browser**. Operations in the right-click menu, except for **Refresh**, can be performed on objects in the search result. After the page is refreshed, objects that have been deleted or renamed or whose schemas have been set can be viewed only from the primary object browser. Right-click options on group names, such as tables, schemas, and views, cannot be performed on objects in the search result. Only objects that you have the permission to access will be displayed in **Search Scope**.

.. note::

   You can view newly added objects in the **Search** window by clicking **Refresh** at the end of the object type.

|image2|

Supported search options

+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Search Option                     | Search Behavior                                                                                                                                                                                                                                        |
+===================================+========================================================================================================================================================================================================================================================+
| Contains                          | Text that contains the searched content will be displayed.                                                                                                                                                                                             |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Starts With                       | Text that starts with the searched content will be displayed.                                                                                                                                                                                          |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Exact Word                        | Text that matches exactly with the searched content will be displayed.                                                                                                                                                                                 |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Regular Expression                | When regular expression text is used, text that meets the search criteria is searched for in **Object Browser**. Select **Regular Expression** from the **Search Criteria** drop-down list. For more information, see POSIX Regular Expressions rules. |
|                                   |                                                                                                                                                                                                                                                        |
|                                   | Examples are as follows:                                                                                                                                                                                                                               |
|                                   |                                                                                                                                                                                                                                                        |
|                                   | -  Enter **^a** to search for all objects that start with **a**.                                                                                                                                                                                       |
|                                   | -  Enter **^[^A-Za-z]+$** to search for objects that do not contain letters.                                                                                                                                                                           |
|                                   | -  Enter **^[^0-9]+$** to search for objects that do not contain digits.                                                                                                                                                                               |
|                                   | -  Enter **^[a-t][^r-z]+$** to search for objects that start with a letter from **a** to **t** and do not contain letters from **r** to **z**.                                                                                                         |
|                                   | -  Enter **^e.*a$** to search for objects that start with **e** and end with **a**.                                                                                                                                                                    |
|                                   | -  Enter **^[a-z]+$** and select **Match Case** to search for objects that contain only lowercase letters.                                                                                                                                             |
|                                   | -  Enter **^[A-Z]+$** and select **Match Case** to search for objects that contain only uppercase letters.                                                                                                                                             |
|                                   | -  Enter **^[A-Za-z]+$** and select **Match Case** to search for objects that contain uppercase and lowercase letters.                                                                                                                                 |
|                                   | -  Enter **^[A-Za-z0-9]+$** and select **Match Case** to search for objects that contain uppercase and lowercase letters and digits.                                                                                                                   |
|                                   | -  Enter **^".*"$** to search for objects that are put in double quotation marks (").                                                                                                                                                                  |
+-----------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Search with underscores (_) or percentage (%)

+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Search Value                      | Search Behavior                                                                                                                                                                                                            |
+===================================+============================================================================================================================================================================================================================+
| \_                                | The underscore (_) in text is considered the wildcard of a single character. Search criteria such as **Regular Expression**, **Starts With**, and **Exact Word** are not applicable to text that contains underscores (_). |
|                                   |                                                                                                                                                                                                                            |
|                                   | Examples are as follows:                                                                                                                                                                                                   |
|                                   |                                                                                                                                                                                                                            |
|                                   | -  Enter **\_ed** to search for objects that start with a single character followed by **ed**.                                                                                                                             |
|                                   | -  Enter **D_t_e** to search for objects that contain **d**, followed by a single character, **t**, single character, and **e**.                                                                                           |
+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| %                                 | The percentage (%) in text is considered the wildcard of multiple characters. Search criteria such as **Regular Expression**, **Starts With**, and **Exact Word** are not applicable to text that contains percentage (%). |
|                                   |                                                                                                                                                                                                                            |
|                                   | Examples are as follows:                                                                                                                                                                                                   |
|                                   |                                                                                                                                                                                                                            |
|                                   | -  Enter **\_%ed** to search for objects that contain **ed**.                                                                                                                                                              |
|                                   | -  Enter **D%t%e** to search for objects that contain **d**, followed by any number of characters, **t**, any number of characters, and **e**.                                                                             |
+-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

If you select **Match Case** and perform the search, the system searches for the content that matches the case of the search text.

.. |image1| image:: /_static/images/en-us_image_0000001145913231.png
.. |image2| image:: /_static/images/en-us_image_0000001145833123.png
