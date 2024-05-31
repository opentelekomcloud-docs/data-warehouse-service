:original_name: dws_16_0039.html

.. _dws_16_0039:

.. _en-us_topic_0000001819336093:

Constraints and Limitations
===========================

The restrictions on using DSC to migrate data from TD are as follows:

-  DSC is used only for syntax migration and not for data migration.

-  If the **SELECT** clause of a subquery contains an aggregate function when the **IN** or **NOT IN** operator is converted to **EXISTS** or **NOT EXISTS**, errors may occur during script migration.

Teradata
--------

-  If a **case** statement containing **FORMAT** is not enclosed in parentheses, this statement will not be processed.

   Examples are as follows:

   ::

      case when column1='0' then column1='value' end (FORMAT 'YYYYMMDD')as alias1

   In this example, **case when column1= "0", column1= "value" end** is not enclosed in parentheses and it will not be processed.

-  If **SELECT \*** and **QUALIFY** clauses are both used in an input query, the migrated query returns an additional column for the **QUALIFY** clause.

   An example is as follows:

   Teradata query

   ::

      SELECT * FROM dwQErrDtl_mc.C03_CORP_TIME_DPSIT_ACCT
      WHERE 1 = 1
      AND Data_Dt = CAST( '20150801' AS DATE FORMAT 'YYYYMMDD' )
      QUALIFY ROW_NUMBER( ) OVER( PARTITION BY Agt_Num, Agt_Modif_Num ORDER BY NULL ) = 1;

   Query after migration

   ::

      SELECT * FROM (
      SELECT *, ROW_NUMBER( ) OVER( PARTITION BY Agt_Num, Agt_Modif_Num ORDER BY NULL ) AS ROW_NUM1
      FROM dwQErrDtl_mc.C03_CORP_TIME_DPSIT_ACCT
      WHERE 1 = 1
      AND Data_Dt = CAST( '20150801' AS DATE )
      ) Q1
      WHERE Q1.ROW_NUM1 = 1;

   In the migrated query, the **ROW_NUMBER() OVER(PARTITION BY Agt_Num and Agt_Modif_Num ORDER BY NULL) AS ROW_NUM1** column is returned additionally.

-  Named references to a table in a query cannot be migrated from subqueries or functions.

   For example, if the input query contains a table named **foo**, DSC will not migrate any named references to the table from a subquery (**foo.fooid**) or when called from a function (**getfoo(foo.fooid)**).

   ::

      SELECT * FROM foo
       WHERE foosubid IN (
                           SELECT foosubid
                             FROM getfoo(foo.fooid) z
                            WHERE z.fooid = foo.fooid
                            );

-  The database with the schema name should be changed to **SET SESSION CURRENT_SCHEMA**.

   +-----------------------------------+------------------------------------------+
   | TD Syntax                         | Syntax After Migration                   |
   +===================================+==========================================+
   | .. code-block::                   | .. code-block::                          |
   |                                   |                                          |
   |    DATABASE SCHTERA               |    SET SESSION CURRENT_SCHEMA TO SCHTERA |
   +-----------------------------------+------------------------------------------+

-  The input file contains the table-specific keyword **MULTISET VOLATILE**, but GaussDB T, GaussDB A, and GaussDB(DWS) do not support it. Therefore, the tool replaces it with the **LOCAL TEMPORARY/UNLOGGED** keyword during the migration process. Use the :ref:`session_mode <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li9493135323214>` configuration parameter to set the default table type (SET/MULTISET) for CREATE TABLE.
