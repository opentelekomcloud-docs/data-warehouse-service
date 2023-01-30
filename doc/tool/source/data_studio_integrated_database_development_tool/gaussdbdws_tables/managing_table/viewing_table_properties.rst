:original_name: DWS_DS_92.html

.. _DWS_DS_92:

Viewing Table Properties
========================

Follow the steps below to view the properties of a table:

#. Right-click the selected table and select **Properties**.

   Data Studio displays the properties (**General**, **Columns**, **Constraints**, and **Index**) of the selected table in different tabs.

   The following table lists the operations that can be performed on each tab along with data editing and refreshing operation. Edit operation is performed by double-clicking the cell.

   +-----------------------------------+--------------------------------------------------+
   | Tab Name                          | Operations Allowed                               |
   +===================================+==================================================+
   | General                           | Save, Cancel, and Copy                           |
   |                                   |                                                  |
   |                                   | .. note::                                        |
   |                                   |                                                  |
   |                                   |    Only Table Description field can be modified. |
   +-----------------------------------+--------------------------------------------------+
   | Columns                           | Add, Delete, Save, Cancel, and Copy              |
   +-----------------------------------+--------------------------------------------------+
   | Constraints                       | Add, Delete, Save, Cancel, and Copy              |
   +-----------------------------------+--------------------------------------------------+
   | Index                             | Add, Delete, Save, Cancel, and Copy              |
   +-----------------------------------+--------------------------------------------------+

   Refer to :ref:`Editing Table Data <dws_ds_102>` section for more information on edit, save, cancel, copy, paste, refresh operations.

   .. important::

      When viewing table data, Data Studio automatically adjusts the column widths for table view. Users can resize the columns as needed. If the text content of a cell exceeds the total available display area, then resizing the cell column may cause DS to become unresponsive.

   .. note::

      -  Individual property window is displayed for each table.
      -  If the property of a table is modified for the table that is already opened, then refresh and open the properties of the table again to view the updated information on the same opened window.
      -  If the content of the column has spaces between the words, then word wrapping is applied to fit the column within the display area. Word wrapping is not applied if the content does not have any spaces between the words.
      -  The size of a column is determined by the column with the maximum content length.
      -  Any change made to the table properties from Object Browser will be reflected after refreshing (|image1|) the **Properties** tab.
      -  Pasting operation is not allowed in **Data Type** column.

.. |image1| image:: /_static/images/en-us_image_0000001145833069.jpg
