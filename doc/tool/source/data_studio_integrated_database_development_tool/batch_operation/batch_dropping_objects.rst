:original_name: DWS_DS_133.html

.. _DWS_DS_133:

Batch Dropping Objects
======================

The batch drop operation allows you to drop multiple objects. This operation also applies to searched objects.

.. note::

   -  Batch drop is allowed only within a database.
   -  An error is reported on batch dropping system objects, which cannot be dropped.

Perform the following steps to batch drop objects:

#. Press **Ctrl+left-click** (select objects one by one) or **Shift+left-click** (select objects in a bunch) to select the objects to be dropped.

#. Right-click and select **Drop Objects**.

   The **Drop Objects** tab displays the list of objects to be dropped.

   +-----------------------+---------------------------------------------------------------------+--------------------------------------------+
   | Column Name           | Description                                                         | Example                                    |
   +=======================+=====================================================================+============================================+
   | Type                  | Displays information about the object type.                         | Table, view                                |
   +-----------------------+---------------------------------------------------------------------+--------------------------------------------+
   | Name                  | Displays the object name.                                           | public.bs_operation_201804                 |
   +-----------------------+---------------------------------------------------------------------+--------------------------------------------+
   | Query                 | Displays the query that will be executed to drop objects.           | DROP TABLE IF EXISTS public.a123           |
   +-----------------------+---------------------------------------------------------------------+--------------------------------------------+
   | Status                | Displays the status of the drop operation.                          | -  To start                                |
   |                       |                                                                     | -  In progress                             |
   |                       | -  |image1| To start: The drop operation has not been started.      | -  Completed                               |
   |                       | -  |image2| In progress: The object is being dropped.               | -  Error                                   |
   |                       | -  |image3| Completed: The drop operation has been completed.       |                                            |
   |                       | -  |image4| Error: The object has not been dropped due to an error. |                                            |
   +-----------------------+---------------------------------------------------------------------+--------------------------------------------+
   | Error Message         | Displays the failure cause of a drop operation.                     | The table **abc** does not exist. Skip it. |
   +-----------------------+---------------------------------------------------------------------+--------------------------------------------+

#. Select the required parameters.

   +--------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Option       | Description                                                                                                                                                                                            |
   +==============+========================================================================================================================================================================================================+
   | Cascade      | The cascade drop operation is performed to drop dependent objects and attributes. The dropped dependent objects will be removed from **Object Browser** only after the refresh operation is performed. |
   +--------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Atomic       | The atomic drop operation is performed to drop all objects. If the operation fails, no objects will be dropped.                                                                                        |
   +--------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No selection | If neither **Cascade** nor **Atomic** is selected, no dependent objects are dropped.                                                                                                                   |
   +--------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **Start**.

   **Runs**: displays the number of objects that are dropped from the object list

   **Errors**: displays the number of objects that are not dropped due to errors

#. Click **Stop** or close the **Drop Objects** dialog box to stop the drop operation.

   For details about copy, advanced copy, show/hide search bar, sort, and column reorder options, see :ref:`Executing SQL Queries <en-us_topic_0000001145833051__en-us_topic_0185264856_section16147111413113>`.

   .. note::

      -  Select part of a cell and press **Ctrl+C** or click |image5| to copy selected text in the cell.
      -  When you select multiple objects in **Object Browser** to drop, a batch drop window is displayed and the object icons are enabled in the menu bar. If you disconnect the database, the icons will remain disabled even after reconnection. In this case, you need to reselect the objects to drop and the selected objects will be displayed in the new batch drop window.

.. |image1| image:: /_static/images/en-us_image_0000001145513345.jpg
.. |image2| image:: /_static/images/en-us_image_0000001145513343.jpg
.. |image3| image:: /_static/images/en-us_image_0000001145913303.jpg
.. |image4| image:: /_static/images/en-us_image_0000001098833340.jpg
.. |image5| image:: /_static/images/en-us_image_0000001145913229.jpg
