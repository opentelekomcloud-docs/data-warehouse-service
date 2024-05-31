:original_name: dws_16_0071.html

.. _dws_16_0071:

.. _en-us_topic_0000001772536444:

COLUMN STORE
============

The table orientation can be converted from ROW-STORE to COLUMN store using the WITH (ORIENTATION=COLUMN) in the CREATE TABLE statement. This feature can be enabled/disabled using the :ref:`rowstoreToColumnstore <en-us_topic_0000001819416085__en-us_topic_0000001706224349_en-us_topic_0000001432527901_li1639915513325>` configuration parameter.

**Input: CREATE TABLE with change orientation to COLUMN STORE**

::

   CREATE MULTISET VOLATILE TABLE tab1
         ( c1 VARCHAR(30) CHARACTER SET UNICODE
         , c2 DATE
         , ...
         )
    PRIMARY INDEX (c1, c2)
    ON COMMIT PRESERVE ROWS;

**Output**

::

   CREATE LOCAL TEMPORARY TABLE tab1
        ( c1 VARCHAR(30)
        , c2 DATE
        , ...
        ) WITH (ORIENTATION = COLUMN)
     ON COMMIT PRESERVE ROWS
     DISTRIBUTE BY HASH (c1, c2);
