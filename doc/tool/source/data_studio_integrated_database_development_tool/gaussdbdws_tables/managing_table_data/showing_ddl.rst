:original_name: DWS_DS_99.html

.. _DWS_DS_99:

Showing DDL
===========

Follow the steps below to show the DDL query of a table:

#. Right-click the selected table and select **Show DDL**.

   The DDL of the selected table is displayed.

   .. note::

      -  A new terminal is opened each time the **Show DDL** operation is executed.
      -  Microsoft Visual C runtime file (msvcrt100.dll) is required to complete this operation. Refer to :ref:`Troubleshooting <en-us_topic_0000001145913163__en-us_topic_0185264982_li1161793674918>` for more information.

   ================= ============= =================
   Database Encoding File Encoding Supports Show DDL
   ================= ============= =================
   UTF-8             UTF-8         Yes
   \                 GBK           Yes
   \                 LATIN1        Yes
   GBK               GBK           Yes
   \                 UTF-8         Yes
   \                 LATIN1        No
   LATIN1            LATIN1        Yes
   \                 GBK           No
   \                 UTF-8         Yes
   ================= ============= =================
