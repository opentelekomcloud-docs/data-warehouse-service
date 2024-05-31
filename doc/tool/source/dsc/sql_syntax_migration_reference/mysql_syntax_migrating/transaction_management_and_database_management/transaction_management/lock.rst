:original_name: dws_16_0207.html

.. _dws_16_0207:

.. _en-us_topic_0000001772536576:

LOCK
====

DSC will perform adaptation based on GaussDB features during the migration of MySQL table locking statements which are used in transaction processing.

**Input**

.. code-block::

   ## A.
   START TRANSACTION;
   LOCK TABLES `mt`.`runoob_tbl` WRITE,`mt`.`runoob_tb2` READ;
   commit;

   ## B.
   START TRANSACTION;
   LOCK TABLES `mt`.`runoob_tbl` WRITE;
   commit;

   ## C.
   START TRANSACTION;
   LOCK TABLES `mt`.`runoob_tbl` READ,`mt`.`runoob_tbl` AS t1 READ;
   commit;

**Output**

.. code-block::

   -- A.
   START TRANSACTION;
   LOCK TABLE "mt"."runoob_tbl" IN ACCESS EXCLUSIVE MODE;
   LOCK TABLE "mt"."runoob_tb2" IN ACCESS SHARE MODE;
   COMMIT WORK;

   -- B.
   START TRANSACTION;
   LOCK TABLE "mt"."runoob_tbl" IN ACCESS EXCLUSIVE MODE;
   COMMIT WORK;

   -- C.
   START TRANSACTION;
   LOCK TABLE "mt"."runoob_tbl" IN ACCESS SHARE MODE;
   COMMIT WORK;
