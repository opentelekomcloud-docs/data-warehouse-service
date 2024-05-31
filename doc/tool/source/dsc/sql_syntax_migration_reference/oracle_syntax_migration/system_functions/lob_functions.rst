:original_name: dws_mt_0136.html

.. _dws_mt_0136:

LOB Functions
=============

This section describes the following LOB functions:

-  :ref:`DBMS_LOB.APPEND <en-us_topic_0000001772696288__en-us_topic_0000001706104693_en-us_topic_0238518397_en-us_topic_0237362225_en-us_topic_0202727205_section176241039144113>`
-  :ref:`DBMS_LOB.COMPARE <en-us_topic_0000001772696288__en-us_topic_0000001706104693_en-us_topic_0238518397_en-us_topic_0237362225_en-us_topic_0202727205_section6217151214426>`
-  :ref:`DBMS_LOB.CREATETEMPORARY <en-us_topic_0000001772696288__en-us_topic_0000001706104693_en-us_topic_0238518397_en-us_topic_0237362225_en-us_topic_0202727205_section1178018136439>`
-  :ref:`DBMS_LOB.INSTR <en-us_topic_0000001772696288__en-us_topic_0000001706104693_en-us_topic_0238518397_en-us_topic_0237362225_en-us_topic_0202727205_section47241940124312>`
-  :ref:`DBMS_LOB.SUBSTR <en-us_topic_0000001772696288__en-us_topic_0000001706104693_en-us_topic_0238518397_en-us_topic_0237362225_en-us_topic_0202727205_section1399115814436>`

.. _en-us_topic_0000001772696288__en-us_topic_0000001706104693_en-us_topic_0238518397_en-us_topic_0237362225_en-us_topic_0202727205_section176241039144113:

DBMS_LOB.APPEND
---------------

**DBMS_LOB.APPEND** function appends the content of a source LOB to a specified LOB.

**Input - DBMS_LOB.APPEND**

.. code-block::

   [sys.]dbms_lob.append(o_menuxml, to_clob('DSJKSDAJKSFDA'));

**Output**

.. code-block::

   o_menuxml := CONCAT(o_menuxml, CAST('DSJKSDAJKSFDA' AS CLOB));

**Input - DBMS_LOB.APPEND**

::

   CREATE
        OR REPLACE PROCEDURE append_example IS clobSrc CLOB ;
        clobDest CLOB ;
   BEGIN
        SELECT
                  clobData INTO clobSrc
             FROM
                  myTable
             WHERE
                  id = 2 ;
                 SELECT
                       clobData INTO clobDest
                  FROM
                       myTable
                  WHERE
                       id = 1 ;
                       readClob ( 1 ) ;
                  DBMS_LOB.APPEND ( clobDest ,clobSrc ) ;
             readClob ( 1 ) ;
   END append_example ;
   /

**Output**

::

   CREATE
        OR REPLACE PROCEDURE append_example IS clobSrc CLOB ;
        clobDest CLOB ;
   BEGIN
        SELECT
                  clobData INTO clobSrc
            FROM
                  myTable
             WHERE
                  id = 2 ;
                  SELECT
                       clobData INTO clobDest
                  FROM
                       myTable
                  WHERE
                       id = 1 ;
                       readClob ( 1 ) ;
                  clobDest := CONCAT( clobDest ,clobSrc ) ;
             readClob ( 1 ) ;
   end ;
   /

.. _en-us_topic_0000001772696288__en-us_topic_0000001706104693_en-us_topic_0238518397_en-us_topic_0237362225_en-us_topic_0202727205_section6217151214426:

DBMS_LOB.COMPARE
----------------

**DBMS_LOB.COMPARE** is an Oracle system function and is not implicitly supported by GaussDB(DWS).

This function is used to compare the full/partial content of two LOBs. To support this feature, use DSC to create a **COMPARE** function in the **MIG_ORA_EXT** schema. The migrated statements will use the new function **MIG_ORA_EXT.MIG_CLOB_COMPARE**, and the examples of using functions in SQL statements are shown as follows.

**COMPARE in SQL**

**Input - DBMS_LOB.COMPARE** **in SQL**

::

   SELECT a.empno ,dbms_lob.compare ( col1 ,col2 ) FROM emp a ,emp b ;

**Output**

::

   SELECT a.empno ,MIG_ORA_EXT.MIG_CLOB_COMPARE ( col1 ,col2 ) FROM emp a ,emp b ;

**Input - DBMS_LOB.COMPARE** **in SQL with CREATE TABLE using 5 parameters**

::

   CREATE TABLE abc nologging AS SELECT dbms_lob.compare ( col1 ,col2 ,3 ,5 ,4 ) FROM emp a ,emp b ;

**Output**

::

   CREATE UNLOGGED TABLE abc AS ( SELECT MIG_ORA_EXT.MIG_CLOB_COMPARE ( col1 ,col2 ,3 ,5 ,4 ) FROM emp a ,emp b ) ;

**Input - DBMS_LOB.COMPARE** **in SQL of a function (NVL2)**

::

   SELECT REPLACE( NVL2( DBMS_LOB.COMPARE ( ENAME ,Last_name ) ,'NO NULL' ,'ONE NULL' ) ,'NULL' ) FROM emp ;

**Output**

::

   SELECT REPLACE( DECODE ( MIG_ORA_EXT.MIG_CLOB_COMPARE ( ENAME ,Last_name ) ,NULL ,'ONE NULL' ,'NO NULL' ) ,'NULL' ,'' ) FROM emp ;

**COMPARE in PL/SQL**

**Input - DBMS_LOB.COMPARE** **in PL/SQL**

::

   DECLARE v_clob clob;
           v_text varchar(1000);
           v_compare_res INT;
   BEGIN
       v_clob := TO_CLOB('abcddedf');
       v_text := '123454';
       v_compare_res := dbms_lob.compare(v_clob, TO_CLOB(v_text));
       DBMS_OUTPUT.PUT_LINE(v_compare_res);
   end;
   /

**Output**

::

   DECLARE v_clob clob;
           v_text varchar(1000);
           v_compare_res INT;
   BEGIN
       v_clob := CAST('abcddedf' AS CLOB);
       v_text := '123454';
       v_compare_res := MIG_ORA_EXT.MIG_CLOB_COMPARE(v_clob,cast(v_text as CLOB));
       DBMS_OUTPUT.PUT_LINE(v_compare_res);
   end;
   /

.. _en-us_topic_0000001772696288__en-us_topic_0000001706104693_en-us_topic_0238518397_en-us_topic_0237362225_en-us_topic_0202727205_section1178018136439:

DBMS_LOB.CREATETEMPORARY
------------------------

The DBMS_LOB.CREATETEMPORARY function creates a temporary LOB and its corresponding index in the default temporary tablespace. DBMS_LOB.FREETEMPORARY is used to delete the temporary LOB and its index.

**Input - DBMS_LOB.CREATETEMPORARY with DBMS_LOB.FREETEMPORARY**

::

   DECLARE v_clob clob;
   BEGIN
       DBMS_LOB.CREATETEMPORARY(v_clob, TRUE, DBMS_LOB.SESSION);
       v_clob := TO_CLOB('abcddedf');
       DBMS_OUTPUT.PUT_LINE(v_clob);
       DBMS_LOB.FREETEMPORARY(v_clob);
   end;
   /

**Output**

::

   DECLARE v_clob clob;
   BEGIN
       -- DBMS_LOB.CREATETEMPORARY(v_clob, TRUE, DBMS_LOB.SESSION);
       v_clob := CAST('abcddedf' AS CLOB);
       DBMS_OUTPUT.PUT_LINE(CAST(v_clob AS TEXT));
       -- DBMS_LOB.FREETEMPORARY(v_clob);
       NULL;
   end;
   /

DBMS_LOB.FREETEMPORARY
----------------------

The DBMS_LOB.FREETEMPORARY function frees the temporary BLOB or CLOB in the default temporary tablespace. After the call to FREETEMPORARY, the LOB locator that is freed is marked as invalid.

**Input - DBMS_LOB.CREATETEMPORARY and DBMS_LOB.FREETEMPORARY**

::

   DECLARE v_clob clob;
   BEGIN
       DBMS_LOB.CREATETEMPORARY(v_clob, TRUE, DBMS_LOB.SESSION);
       v_clob := TO_CLOB('abcddedf');
       DBMS_OUTPUT.PUT_LINE(v_clob);
       DBMS_LOB.FREETEMPORARY(v_clob);
   end;
   /

**Output**

::

   DECLARE v_clob clob ;
   BEGIN
             /*DBMS_LOB.CREATETEMPORARY(v_clob, TRUE, DBMS_LOB.SESSION);*/
             v_clob := cast( 'abcddedf' as CLOB ) ;
             DBMS_OUTPUT.PUT_LINE ( v_clob ) ;
             /* DBMS_LOB.FREETEMPORARY(v_clob); */
             null ;
        end ;
        /

.. _en-us_topic_0000001772696288__en-us_topic_0000001706104693_en-us_topic_0238518397_en-us_topic_0237362225_en-us_topic_0202727205_section47241940124312:

DBMS_LOB.INSTR
--------------

DBMS_LOB.INSTR function returns the matching position of the n\ :sup:`th` occurrence of the pattern in the LOB, starting from the offset specified.

**Input - DBMS_LOB.INSTR** **in SQL**

::

   SELECT expr1, …, DBMS_LOB.INSTR(str, septr, 1, 5)
     FROM tab1
    WHERE …;

**Output**

::

   SELECT expr1, …, INSTR(str, septr, 1, 5)
     FROM tab1
    WHERE …

**Input - DBMS_LOB.INSTR** **in PL/SQL**

::

   BEGIN
     …
          pos := DBMS_LOB.INSTR(str,septr,1, i);
     ...
   END;
   /

**Output**

::

   BEGIN
     …
          pos := INSTR(str,septr,1, i);
     ...
   END;
   /

.. _en-us_topic_0000001772696288__en-us_topic_0000001706104693_en-us_topic_0238518397_en-us_topic_0237362225_en-us_topic_0202727205_section1399115814436:

DBMS_LOB.SUBSTR
---------------

You can specify whether to migrate this function by configuring parameter **MigDbmsLob**.

**Input - DBMS_LOB.SUBSTR** **when MigDbmsLob is set to true**

If the value of **MigDbmsLob** is **true**, then migration happens. If the value is **false**, then migration does not happen.

**Input**

.. code-block::

   SELECT dbms_lob.substr('!2d3d4dd!',1,5);

**Output**

.. code-block::

   If the config param is true, it should be migrated as below:
   select substr('!2d3d4dd!',5,1);

   If false, it should be retained as it is:
   select dbms_lob.substr('!2d3d4dd!',1,5);

**Input**

.. code-block::

   SELECT dbms_lob.substr('!2d3d4dd!',5);

**Output**

.. code-block::

   If the config param is true, it should be migrated as below:
   select substr('!2d3d4dd!',1,5);

   If false, it should be retained as it is:
   select dbms_lob.substr('!2d3d4dd!',5);
