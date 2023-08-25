:original_name: DWS_DS_51.html

.. _DWS_DS_51:

Exporting Schema DDL
====================

Exporting the schema DDL exports the DDLs of functions/procedures, tables, sequences and views of the schema.

Follow the steps to export the schema DDL:

#. In the **Object Browser** pane, right-click the selected schema and select **Export DDL**.

   The **Data Studio Security Disclaimer** dialog box is displayed. You can close this security disclaimer message. For details, see :ref:`Security Disclaimer <en-us_topic_0000001234042179__en-us_topic_0185264975_section8272203611226>`.

#. Click **OK**.

   The **Save As** dialog box is displayed.

   |image1|

#. In the **Save As** dialog box, select the location to save the DDL and click **Save**. The status bar displays the progress of the operation.

   .. note::

      -  To cancel the export operation, double-click the status to open the **Progress View** tab and click |image2|. For details, see :ref:`Canceling the Table Data Export Operation <en-us_topic_0000001188521136__en-us_topic_0185264892_ref471314989>`.
      -  If the file name contains characters that are not supported by Windows, the file name will be different from the schema name.
      -  MS Visual C runtime file (msvcrt100.dll) is required to complete this operation. For details, see :ref:`Troubleshooting <dws_ds_145>`.

   The **Export** message and the status bar display the status of the completed operation.

   ================= ============= ===========================
   Database Encoding File Encoding Support for Exporting a DDL
   ================= ============= ===========================
   UTF-8             UTF-8         Yes
   \                 GBK           Yes
   \                 LATIN1        Yes
   GBK               GBK           Yes
   \                 UTF-8         Yes
   \                 LATIN1        No
   LATIN1            LATIN1        Yes
   \                 GBK           No
   \                 UTF-8         Yes
   ================= ============= ===========================

   .. note::

      You can select multiple objects and export their DDL. :ref:`Batch Export <en-us_topic_0000001188681066__en-us_topic_0185264547_li1037472864716>` lists the objects whose DDL cannot be exported.

.. |image1| image:: /_static/images/en-us_image_0000001188521248.png
.. |image2| image:: /_static/images/en-us_image_0000001234042443.jpg
