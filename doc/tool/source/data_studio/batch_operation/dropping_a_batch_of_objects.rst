:original_name: DWS_DS_133.html

.. _DWS_DS_133:

Dropping a Batch of Objects
===========================

The batch drop operation allows you select multiple objects to drop. You can also perform batch drop operation on searched objects.

.. note::

   -  Batch drop is allowed only within a database.
   -  Batch drop of system objects will result in error, since system objects cannot be dropped.

Follow the steps below to drop objects in a batch:

#. Press **Ctrl+left-click** (select objects one by one) or **Shift+left-click** (select objects in a bunch) to select the objects to be dropped.

#. Right-click and select **Drop Objects**.

   **Drop Objects** tab displays with the list of objects to be dropped.

   +-----------------------+-----------------------------------------------------------------------+--------------------------------------+
   | Column Name           | Description                                                           | Example                              |
   +=======================+=======================================================================+======================================+
   | Type                  | Displays information on the object type.                              | table, views                         |
   +-----------------------+-----------------------------------------------------------------------+--------------------------------------+
   | Name                  | Displays the name of the object.                                      | public.bs_operation_201804           |
   +-----------------------+-----------------------------------------------------------------------+--------------------------------------+
   | Query                 | Displays the query that will be executed to drop the object.          | DROP TABLE IF EXISTS public.a123     |
   +-----------------------+-----------------------------------------------------------------------+--------------------------------------+
   | Status                | Displays the status of the drop operation.                            | -  To start                          |
   |                       |                                                                       | -  In progress                       |
   |                       | -  |image1| - To start: The drop operation yet to be initiated.       | -  Completed                         |
   |                       | -  |image2| - In progress: The object is currently being dropped.     | -  Error                             |
   |                       | -  |image3| - Completed: The drop operation has been completed.       |                                      |
   |                       | -  |image4| - Error: The object has not been dropped due to an error. |                                      |
   +-----------------------+-----------------------------------------------------------------------+--------------------------------------+
   | Error Message         | Displays the failure reason of the drop operation.                    | Table "abc" does not exist. Skip it. |
   +-----------------------+-----------------------------------------------------------------------+--------------------------------------+

#. Select the required drop option:

   +--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Option       | Description                                                                                                                                                                                    |
   +==============+================================================================================================================================================================================================+
   | Cascade      | Cascade drop operation drops their dependent objects and attributes. The dependent objects that are dropped will be removed from the Object Browser only after refresh operation is performed. |
   +--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Atomic       | Atomic drop operation drops all objects in case of success or drops none in case of a failure.                                                                                                 |
   +--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | No selection | Un-selection of Atomic or Cascade does not drop dependent objects.                                                                                                                             |
   +--------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

#. Click **Start**.

   **Runs** - Displays the number of objects that are dropped from the total list of objects.

   **Errors** - Displays the number of object that was not dropped due to errors.

#. Click **Stop** or close the **Drop Objects** dialog to stop the drop operation.

   Refer to :ref:`Execute SQL Queries <en-us_topic_0000001234200573__en-us_topic_0185264856_section16147111413113>` section for information on copy, advanced copy, show/hide search bar, sort, and column reorder options.

   .. note::

      -  Select part of cell content and press **Ctrl+C** or click |image5| to copy selected text from a cell.
      -  When you select multiple objects in object browser to drop, a batch drop window is opened and its menu icons are enabled in the menu bar. If you disconnect the database, the icons will remain disabled and will not be enabled even after reconnecting. You need to reselect the objects to drop and the selected objects will be available in the new terminal window.

.. |image1| image:: /_static/images/en-us_image_0000001188521406.jpg
.. |image2| image:: /_static/images/en-us_image_0000001234042439.jpg
.. |image3| image:: /_static/images/en-us_image_0000001188362854.jpg
.. |image4| image:: /_static/images/en-us_image_0000001188681322.jpg
.. |image5| image:: /_static/images/en-us_image_0000001188202728.jpg
