:original_name: DWS_DS_032.html

.. _DWS_DS_032:

Selecting a DB Object in the SQL Terminal
=========================================

Data Studio suggests a list of possible schema names, table names and column names, and views in the SQL Terminal.

Follow the steps below to select a DB object:

#. Press **Ctrl** and **Space** and enter the required parent DB object name. The DB objects list is refined as you continue typing the DB object name. The DB objects list displays all DB objects of the database connected to the SQL Terminal.

   |image1|

#. To select the parent DB object, use the **Up** or **Down** arrow keys and press **Enter** on the keyboard, or double-click the parent DB object.

#. Press **.** to list all child DB objects.

   |image2|

#. To select the child DB object, use the **Up** or **Down** arrow keys and press **Enter** on the keyboard, or double-click the child DB object.

   On selection, the child DB object will be appended to the parent DB object (with a period '.').

   .. note::

      -  Auto-suggest also works on keywords, data types, schema names, table names, views, and table name aliases in the same way as shown above for all schema objects that you have access.

         Following is a sample query with alias objects:

         ::

            SELECT
              table_alias.<auto-suggest>
            FROM test.t1 AS table_alias
              WHERE
                table_alias.<auto-suggest> = 5
            GROUP BY table_alias.<auto-suggest>
            HAVING table_alias.<auto-suggest> = 5
            ORDER BY table alias.<auto-suggest>

      -  A loading message may be displayed on the **SQL Terminal** in the following scenarios:

         -  The object is not loaded due to the value mentioned in the **Load Limit** field. For details, see :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>`.
         -  The objects that are already added to **Exclude** will not be loaded. For details, see :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>`.
         -  There is a delay in fetching the object from the server.

      -  If there are objects with the same name in different case, then auto-suggest will display child objects of both parent objects.

         Example: If there are two schemas with the name **public** and **PUBLIC**, then all child objects for both these schemas will be displayed.

.. |image1| image:: /_static/images/en-us_image_0000001860199285.jpg
.. |image2| image:: /_static/images/en-us_image_0000001860319125.jpg
