:original_name: DWS_DS_97.html

.. _DWS_DS_97:

Exporting Table DDL and Data
============================

The exported table DDL and data include the following:

-  DDL of the table
-  Columns and rows of the table

Perform the following steps to export the table DDL and data:

#. In the **Object Browser** pane, right-click the selected table and select **Export DDL and Data**.

   The **Data Studio Security Disclaimer** dialog box is displayed.

#. Click **OK**.

   The **Save As** dialog box is displayed.

#. In the **Save As** dialog box, select the location to save the DDL and click **Save**. The status bar displays the operation progress.

   .. note::

      -  To cancel the export operation, double-click the status bar to open the **Progress View** tab and click |image1|. For details, see :ref:`Canceling Table Data Export <en-us_topic_0000001098673338__en-us_topic_0185264892_ref471314989>`.
      -  If the file name contains characters that are not supported by Windows, the file name will be different from the schema name.
      -  The Microsoft Visual C Runtime file (**msvcrt100.dll**) is required for this operation. For details, see :ref:`Troubleshooting <dws_ds_145>`.

   The **Data Exported Successfully** dialog box and status bar display the status of the completed operation.

   ================= ============= =========================
   Database Encoding File Encoding Support for Exporting DDL
   ================= ============= =========================
   UTF-8             UTF-8         Yes
   \                 GBK           Yes
   \                 LATIN1        Yes
   GBK               GBK           Yes
   \                 UTF-8         Yes
   \                 LATIN1        No
   LATIN1            LATIN1        Yes
   \                 GBK           No
   \                 UTF-8         Yes
   ================= ============= =========================

   .. note::

      You can select multiple objects from ordinary and partitioned tables to export DDL and data, including columns, rows, indexes, constraints, and partitions. :ref:`Batch Export <en-us_topic_0000001098993156__en-us_topic_0185264547_li1037472864716>` lists the objects whose DDL and data cannot be exported.

.. |image1| image:: /_static/images/en-us_image_0000001145713169.jpg
