:original_name: dws_16_0063.html

.. _dws_16_0063:

.. _en-us_topic_0000001772536436:

CREATE TABLE
============

The Teradata **CREATE TABLE** (:ref:`short key <en-us_topic_0000001772696108>` CT) statements are used to create new tables.

**Example:**

**Input: CREATE TABLE**

::

   CT tab1 (
        id INT
   );

**Output**

::

   CREATE
        TABLE
             tab1 (
                  id INTEGER
             )
   ;

When **CREATE tab2 AS tab1** is executed, the structure copied from **tab1** is used to create table tab2. If the CREATE TABLE statement includes WITH DATA operator, then the data from *tab1* is also copied into *tab2*. When CREATE AS is used, the CONSTRAINT row in the source table is retained in the new table.

-  If :ref:`•session_mode <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li9493135323214>` = *Teradata*, the default table type is **SET** in which duplicate records must be removed. This is done by adding the **MINUS** operator in the migrated scripts.
-  If :ref:`•session_mode <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li9493135323214>` = *ANSI*, the default table type is **MULTISET** in which duplicate records must be allowed.

If the source table has a PRIMARY KEY or a UNIQUE CONSTRAINT, then it will not contain any duplicate records. In this case, the MINUS operator is not required or added to remove duplicate records.

**Example:**

**Input: CREATE TABLE AS with DATA (session_mode=Teradata)**

::

   CREATE TABLE tab2
       AS tab1 WITH DATA;

**Output**

::

   BEGIN
       CREATE TABLE tab2 (
               LIKE tab1 INCLUDING ALL EXCLUDING PARTITION EXCLUDING RELOPTIONS
                         );

       INSERT INTO tab2
       SELECT * FROM tab1
               MINUS SELECT * FROM tab2;
   END
   ;
   /

**Example: Input: CREATE TABLE AS with DATA AND STATISTICS**

::

   CREATE SET VOLATILE TABLE tab2025
    AS ( SELECT * from tab2023 )
    WITH DATA AND STATISTICS
    PRIMARY INDEX (LOGTYPE, OPERSEQ);

**Output**

::

   CREATE LOCAL TEMPORARY TABLE tab2025
    DISTRIBUTE BY HASH ( LOGTYPE, OPERSEQ )
    AS ( SELECT * FROM tab2023 );

    ANALYZE tab2025;
