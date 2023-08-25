:original_name: DWS_DS_99.html

.. _DWS_DS_99:

Show DDL
========

To display a DDL query for a table, perform the following steps:

#. Right-click the table, and then select **Show DDL**.

   Data Studio displays the DDL of the selected table.

   .. note::

      -  A new terminal window is opened each time you select to show DDL.
      -  MS Visual C runtime file (msvcrt100.dll) is required to complete this operation. For details, see :ref:`Troubleshooting <dws_ds_145>`.

   ================= ============= ========
   Database Encoding File Encoding Show DDL
   ================= ============= ========
   UTF-8             UTF-8         Yes
   \                 GBK           Yes
   \                 LATIN1        Yes
   GBK               GBK           Yes
   \                 UTF-8         Yes
   \                 LATIN1        No
   LATIN1            LATIN1        Yes
   \                 GBK           No
   \                 UTF-8         Yes
   ================= ============= ========
