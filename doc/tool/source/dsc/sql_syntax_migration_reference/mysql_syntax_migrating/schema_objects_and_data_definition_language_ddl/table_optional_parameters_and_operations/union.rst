:original_name: dws_16_0152.html

.. _dws_16_0152:

.. _en-us_topic_0000001819416237:

UNION
=====

**UNION** is a table creation parameter of the MERGE storage engine. Creating a table using this keyword is similar to creating a common view. The created table logically combines the data of multiple tables that are specified by UNION. DSC migrates this feature to the view creation statement in GaussDB.

**Input**

.. code-block::

   CREATE TABLE t1 (
       a INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
       message CHAR(20)
   ) ENGINE=MyISAM;

   CREATE TABLE t2 (
       a INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
       message CHAR(20)
   ) ENGINE=MyISAM;

   CREATE TABLE total (
       a INT NOT NULL AUTO_INCREMENT,
       message CHAR(20))
   ENGINE=MyISAM UNION=(t1,t2) INSERT_METHOD=LAST;

**Output**

.. code-block::

   CREATE TABLE "public"."t1" (
       "a" SERIAL NOT NULL PRIMARY KEY,
       "message" CHAR(80)
   )
   WITH ( ORIENTATION = ROW, COMPRESSION = NO )
   NOCOMPRESS
   DISTRIBUTE BY HASH ("a");

   CREATE TABLE "public"."t2" (
       a SERIAL NOT NULL PRIMARY KEY,
       message CHAR(80)
   )
   WITH ( ORIENTATION = ROW, COMPRESSION = NO )
   NOCOMPRESS
   DISTRIBUTE BY HASH ("a");

   CREATE VIEW "public"."total"(a, message) AS
   SELECT * FROM "public"."t1"
   UNION ALL
   SELECT * FROM "public"."t2";
