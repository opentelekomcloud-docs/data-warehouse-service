:original_name: dws_16_0161.html

.. _dws_16_0161:

.. _en-us_topic_0000001819336217:

RENAME (Table Renaming)
=======================

The statement for renaming a table in MySQL is slightly different from that in GaussDB(DWS). DSC will perform adaptation based on GaussDB(DWS) features during migration.

.. caution::

   Currently, DSC does not support original table names prefixed with **DATABASE/SCHEMA.**

#. MySQL uses the **RENAME TABLE** statement to change a table name.

   **Input**

   .. code-block::

      # Rename a single table.
      RENAME TABLE DEPARTMENT TO NEWDEPT;

       # Rename multiple tables.
      RENAME TABLE NEWDEPT TO NEWDEPT_02,PEOPLE TO PEOPLE_02;

   **Output**

   .. code-block::

      -- Rename a single table.
      ALTER TABLE "public"."department" RENAME TO "newdept";

      -- Rename multiple tables.
      ALTER TABLE "public"."newdept" RENAME TO "newdept_02";
      ALTER TABLE "public"."people" RENAME TO "people_02";

#. In MySQL, the **ALTER TABLE RENAME** statement is used to change a table name. When this statement is migrated by DSC, the keyword **AS** is converted to **TO**.

   **Input**

   .. code-block::

      ## A.
      ALTER TABLE runoob_alter_test RENAME TO runoob_alter_testnew;

      ## B.
      ALTER TABLE runoob_alter_testnew RENAME AS runoob_alter_testnewnew;

   **Output**

   .. code-block::

      -- A.
      ALTER TABLE "public"."runoob_alter_test" RENAME TO "runoob_alter_testnew";

      -- B.
      ALTER TABLE "public"."runoob_alter_testnew" RENAME TO "runoob_alter_testnewnew";
