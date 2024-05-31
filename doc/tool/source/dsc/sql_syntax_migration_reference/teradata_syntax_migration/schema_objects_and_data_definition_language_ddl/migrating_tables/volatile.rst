:original_name: dws_16_0065.html

.. _dws_16_0065:

.. _en-us_topic_0000001819336113:

VOLATILE
========

The table-specific keyword **VOLATILE** is provided in the input file, but the keyword is not supported by GaussDB(DWS). The tool replaces it with the **LOCAL TEMPORARY** keyword during the migration process. Volatile tables are migrated as local temporary or unlogged based on the config input.

**Input: CREATE VOLATILE TABLE**

::

    CREATE VOLATILE TABLE T1 (c1 int ,c2 int);

**Output**

::

   CREATE
       LOCAL TEMPORARY TABLE
       T1 (
            c1 INTEGER
           ,c2 INTEGER
          )
   ;

**Input: CREATE VOLATILE TABLE AS WITH DATA** (session_mode=Teradata)

If the source table has a PRIMARY KEY or a UNIQUE CONSTRAINT, then it will not contain any duplicate records. In this case, the MINUS operator is not required or added to remove duplicate records.

::

   CREATE VOLATILE TABLE tabV1 (
           C1 INTEGER DEFAULT 99
          ,C2 INTEGER
          ,C3 INTEGER
          ,C4 NUMERIC (20,0) DEFAULT NULL (BIGINT)
          ,CONSTRAINT XX1 PRIMARY KEY ( C1, C2 )
          ) PRIMARY INDEX (C1, C3 );

   CREATE TABLE tabV2 AS tabV1 WITH DATA PRIMARY INDEX (C1)
                ON COMMIT PRESERVE ROWS;

**Output**

::

   CREATE LOCAL TEMPORARY TABLE tabV1 (
           C1 INTEGER DEFAULT 99
          ,C2 INTEGER
          ,C3 INTEGER
          ,C4 NUMERIC (20,0) DEFAULT CAST( NULL AS BIGINT )
          ,CONSTRAINT XX1 PRIMARY KEY ( C1, C2 )
          ) DISTRIBUTE BY HASH (C1);

   BEGIN
       CREATE TABLE tabV2 (
                  LIKE tabV1 INCLUDING ALL EXCLUDING PARTITION EXCLUDING RELOPTIONS EXCLUDING DISTRIBUTION
                          ) DISTRIBUTE BY HASH (C1);
       INSERT INTO tabV2 SELECT * FROM tabV1;
   END
   ;
   /
