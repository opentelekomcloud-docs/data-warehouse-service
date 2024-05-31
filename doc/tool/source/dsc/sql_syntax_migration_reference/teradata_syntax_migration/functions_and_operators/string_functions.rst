:original_name: dws_16_0045.html

.. _dws_16_0045:

.. _en-us_topic_0000001772696100:

String Functions
================

.. _en-us_topic_0000001772696100__en-us_topic_0000001658025014_en-us_topic_0000001384550456_section98336268713:

CHAR Function
-------------

**Input: CHAR**

::

   CHAR( expression1 )

**Output**

::

   LENGTH( expression1 )

.. _en-us_topic_0000001772696100__en-us_topic_0000001658025014_en-us_topic_0000001384550456_section18359116786:

CHARACTERS
----------

**Input: CHARACTERS**

::

   CHARACTERS( expression1 )

**Output**

::

   LENGTH( expression1 )

.. _en-us_topic_0000001772696100__en-us_topic_0000001658025014_en-us_topic_0000001384550456_section5834203015820:

INDEX
-----

**Input: INDEX**

::

   SELECT INDEX(expr1/string, substring)
     FROM tab1
   WHERE ... ;

**Output**

::

    SELECT INSTR(expr1/string, substring)
     FROM tab1
   WHERE ... ;

.. _en-us_topic_0000001772696100__en-us_topic_0000001658025014_en-us_topic_0000001384550456_section156724412105:

STRREPLACE
----------

**Input: STRREPLACE**

::

   SELECT STRREPLACE(c2, '.', '')
     FROM tab1
    WHERE ...;

**Output**

::

   SELECT REPLACE(c2, '.', '')
     FROM tab1
    WHERE ...;

.. _en-us_topic_0000001772696100__en-us_topic_0000001658025014_en-us_topic_0000001384550456_section11574199161010:

OREPLACE
--------

**Input: OREPLACE**

::

   SELECT OREPLACE (c2, '.', '')
     FROM tab1
   WHERE ... ;

**Output**

::

    SELECT REPLACE(c2, '.', '')
     FROM tab1
   WHERE ... ;

STRTOK
------

+------------------------------------------------------+--------------------------------------------------------------+
| Input                                                | Output                                                       |
+======================================================+==============================================================+
| .. code-block::                                      | .. code-block::                                              |
|                                                      |                                                              |
|    LENGTH(STRTOK(STRTOK(JOB_NAME_TADD,'-',4),'_',2)) |    LENGTH(split_part(split_part(JOB_NAME_TADD,'-',4),'_',2)) |
+------------------------------------------------------+--------------------------------------------------------------+
