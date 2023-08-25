:original_name: DWS_DS_52.html

.. _DWS_DS_52:

Exporting Schema DDL and Data
=============================

The exported schema DDL and data include the following:

-  DDLs of functions/procedures of the schema.
-  DDLs and data of tables of the schema.
-  DDLs of views of the schema.
-  DDLs of sequences of the schema.

Follow steps below to export the schema DDL and data:

#. In the **Object Browser** pane, right-click the selected schema and select **Export DDL and Data**.

   The **Data Studio Security Disclaimer** dialog box is displayed.

   You can close this security disclaimer message. For details, see :ref:`Security Disclaimer <en-us_topic_0000001234042179__en-us_topic_0185264975_section8272203611226>`.

#. Click **OK**.

   The **Save As** dialog box is displayed.

#. In the **Save As** dialog box, select the location to save the DDL and data and click **Save**. The status bar displays the progress of the operation.

   .. note::

      -  To cancel the export operation, double-click the status to open the **Progress View** tab and click |image1|. For details, see :ref:`Canceling the Table Data Export Operation <en-us_topic_0000001188521136__en-us_topic_0185264892_ref471314989>`.
      -  The exported file name will not be the same as schema name, if the schema name contains characters which are not supported by Windows.
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

      You can select multiple objects and export their DDL and data. :ref:`Batch Export <en-us_topic_0000001188681066__en-us_topic_0185264547_li1037472864716>` lists the objects whose DDL and data cannot be exported.

.. |image1| image:: /_static/images/en-us_image_0000001188681326.jpg
