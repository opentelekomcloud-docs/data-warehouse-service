:original_name: DWS_DS_92.html

.. _DWS_DS_92:

Viewing Table Properties
========================

Perform the following operations to view the table properties:

#. Right-click the selected table and select **Properties**.

   Data Studio displays the properties (General, Columns, Constraints, and Index) of the selected table in different tabs.

   The following table lists the operations that can be performed on each tab page, as well as editing and refreshing operations. You can double-click a cell to edit it.

   +-----------------------------------+----------------------------------------------------------+
   | Tab                               | Operation                                                |
   +===================================+==========================================================+
   | General                           | Save, Cancel, and Copy                                   |
   |                                   |                                                          |
   |                                   | .. note::                                                |
   |                                   |                                                          |
   |                                   |    Only the **Table Description** field can be modified. |
   +-----------------------------------+----------------------------------------------------------+
   | Column                            | Add, Delete, Save, Cancel, and Copy                      |
   +-----------------------------------+----------------------------------------------------------+
   | CONSTRAINT                        | Add, Delete, Save, Cancel, and Copy                      |
   +-----------------------------------+----------------------------------------------------------+
   | Indexes                           | Add, Delete, Save, Cancel, and Copy                      |
   +-----------------------------------+----------------------------------------------------------+

   For more information about Edit, Save, Cancel, Copy, and Refresh operations, see :ref:`Editing Table Data <dws_ds_102>`.

   .. important::

      When viewing table data, Data Studio automatically adjusts the column widths for table view. Users can resize the columns as needed. If the text content of a cell exceeds the total available display area, then resizing the cell column may cause Data Studio to become unresponsive.

   .. note::

      -  A property window is displayed for each table.
      -  If the property of a table is modified for a table that is already opened, refresh and open the properties of the table again to view the updated information on the same opened window.
      -  If the text of a column contains spaces, word wrapping is applied to fit the column width. Word wrapping is not applied to columns without spaces.
      -  The size of the column is determined by the maximum content length column.
      -  After you refresh (click |image1|) the **Properties** tab, the changes made to the table properties on the Object Browser are displayed.
      -  Pasting operation is not allowed in the **Data Type** column.

.. |image1| image:: /_static/images/en-us_image_0000001188202732.jpg
