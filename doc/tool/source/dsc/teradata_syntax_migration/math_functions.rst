:original_name: dws_mt_0100.html

.. _dws_mt_0100:

Math Functions
==============

\*\*
----

**Input: \*\***

::

   expr1 ** expr2

**Output**

::

   expr1 ^ expr2

MOD
---

**Input: MOD**

::

   expr1 MOD expr2

**Output**

::

    expr1 % expr2

.. _en-us_topic_0000001188362556__en-us_topic_0238518367_en-us_topic_0237362349_section5422047392:

NULLIFZERO
----------

Use the :ref:`tdMigrateNULLIFZERO <en-us_topic_0000001233922159__en-us_topic_0218440346_li1551601339>` configuration parameter to configure migration of NULLIFZERO.

**Input: NULLIFZERO**

::

   SELECT NULLIFZERO(expr1) FROM tab1
   WHERE … ;

**Output**

::

   SELECT NULLIF(expr1, 0) FROM tab1
    WHERE … ;

.. _en-us_topic_0000001188362556__en-us_topic_0238518367_en-us_topic_0237362349_section95621584112:

ZEROIFNULL
----------

Use the :ref:`tdMigrateZEROIFNULL <en-us_topic_0000001233922159__en-us_topic_0218440346_li195242216333>` configuration parameter to configure migration of ZEROIFNULL.

**Input: ZEROIFNULL**

::

   SELECT ZEROIFNULL(expr1) FROM tab1
   WHERE … ;

**Output**

::

   SELECT COALESCE(expr1, 0) FROM tab1
    WHERE … ;

Declaring a Hexadecimal Character Literal Value
-----------------------------------------------

**Input**

.. code-block::

   SELECT
    (COALESCE(TRIM(BOTH FROM VTX_D_RPT_0017_WMSE12_01_01.ID),''))
    ||'7E'xc||(COALESCE(TRIM(BOTH FROM VTX_D_RPT_0017_WMSE12_01_01.Code),''))
    ||'7E'xc||(COALESCE(TRIM(BOTH FROM VTX_D_RPT_0017_WMSE12_01_01.Description),''))
    ||'7E'xc||(COALESCE(TRIM(BOTH FROM VTX_D_RPT_0017_WMSE12_01_01.Name),''))
    ||'7E'xc||(COALESCE(TRIM(BOTH FROM VTX_D_RPT_0017_WMSE12_01_01.Host_Product_Id),''))
   FROM DP_VTXEDW.VTX_D_RPT_0017_WMSE12_01_01   VTX_D_RPT_0017_WMSE12_01_01
   WHERE 1=1
   ;

**Output**

.. code-block::

   SELECT
    (COALESCE(TRIM(BOTH FROM VTX_D_RPT_0017_WMSE12_01_01.ID),''))
    ||E'\x7E'||(COALESCE(TRIM(BOTH FROM VTX_D_RPT_0017_WMSE12_01_01.Code),''))
    ||E'\x7E'||(COALESCE(TRIM(BOTH FROM VTX_D_RPT_0017_WMSE12_01_01.Description),''))
    ||E'\x7E'||(COALESCE(TRIM(BOTH FROM VTX_D_RPT_0017_WMSE12_01_01.Name),''))
    ||E'\x7E'||(COALESCE(TRIM(BOTH FROM VTX_D_RPT_0017_WMSE12_01_01.Host_Product_Id),''))
   FROM DP_VTXEDW.VTX_D_RPT_0017_WMSE12_01_01   VTX_D_RPT_0017_WMSE12_01_01
   WHERE 1=1
   ;

Declaring a Hexadecimal Binary Literal Value
--------------------------------------------

**Input**

.. code-block::

   CREATE MULTISET TABLE bvalues (IDVal INTEGER, CodeVal BYTE(2));
   INSERT INTO bvalues VALUES (112193, '7879'XB) ;
   SELECT IDVal, CodeVal FROM bvalues WHERE CodeVal = '7879'XB ;

**Output**

.. code-block::

   CREATE TABLE bvalues (IDVal INTEGER, CodeVal BYTEA);
   INSERT INTO bvalues VALUES (112193, BYTEA '\x7879') ;
   SELECT IDVal, CodeVal FROM bvalues WHERE CodeVal = BYTEA '\x7879' ;
