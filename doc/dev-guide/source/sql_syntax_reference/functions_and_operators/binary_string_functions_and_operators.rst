:original_name: dws_06_0031.html

.. _dws_06_0031:

Binary String Functions and Operators
=====================================

SQL defines some binary string functions that use keywords, rather than commas, to separate arguments. GaussDB(DWS) also provides the common syntax used for invoking functions.

octet_length(string)
--------------------

Description: Returns the number of bytes in the given binary string.

Return type: integer

Examples:

::

   SELECT octet_length(E'jo\\000se'::bytea) AS RESULT;
    result
   --------
         5
   (1 row)

overlay(string placing string from int [for int])
-------------------------------------------------

Description: Replaces the given substring.

Return type: bytea

Examples:

::

   SELECT overlay(E'Th\\000omas'::bytea placing E'\\002\\003'::bytea from 2 for 3) AS RESULT;
        result
   ----------------
    \x5402036d6173
   (1 row)

position(substring in string)
-----------------------------

Description: Returns the location of the given substring.

Return type: integer

Examples:

::

   SELECT position(E'\\000om'::bytea in E'Th\\000omas'::bytea) AS RESULT;
    result
   --------
         3
   (1 row)

substring(string [from int] [for int])
--------------------------------------

Description: Truncates the given substring.

Return type: bytea

Examples:

::

   SELECT substring(E'Th\\000omas'::bytea from 2 for 3) AS RESULT;
     result
   ----------
    \x68006f
   (1 row)

Truncate the time and obtain the number of hours.

::

   SELECT substring('2022-07-18 24:38:15',12,2)AS RESULT;
    result
   -----------
    24
   (1 row)

trim([both] bytes from string)
------------------------------

Description: Removes the longest string containing only bytes from **bytes** from the start and end of **string**.

Return type: bytea

Examples:

::

   SELECT trim(E'\\000'::bytea from E'\\000Tom\\000'::bytea) AS RESULT;
     result
   ----------
    \x546f6d
   (1 row)

substring_index(string, delim, count)
-------------------------------------

Description: Searches for delimiters in a case-sensitive manner and returns the substring before the delimiter that appears for the **count** time. If **count** is a negative number, count for the delimiter reversely from the end. If the parameter contains NULL, NULL will be returned. This function is supported by version 8.2.0 or later clusters.

Return type: text

Example: In the string **www.wWw.cloud.wWw.com**, find the delimiter **.wWw.** that appears the second time in a case-sensitive manner. Return the substring before it: **www.wWw.cloud**.

::

   SELECT SUBSTRING_INDEX('www.wWw.cloud.wWw.com', '.wWw.', 2) AS RESULT;
       result
   ---------------
    www.wWw.cloud
   (1 row)

btrim(string bytea,bytes bytea)
-------------------------------

Description: Removes the longest string containing only bytes from **bytes** from the start and end of **string**.

Return type: bytea

Examples:

::

   SELECT btrim(E'\\000trim\\000'::bytea, E'\\000'::bytea) AS RESULT;
      result
   ------------
    \x7472696d
   (1 row)

get_bit(string, offset)
-----------------------

Description: Returns the number of bits in the given string.

Return type: integer

Examples:

::

   SELECT get_bit(E'Th\\000omas'::bytea, 45) AS RESULT;
    result
   --------
         1
   (1 row)

get_byte(string, offset)
------------------------

Description: Returns the number of bytes in the given string.

Return type: integer

Examples:

::

   SELECT get_byte(E'Th\\000omas'::bytea, 4) AS RESULT;
    result
   --------
       109
   (1 row)

set_bit(string,offset, newvalue)
--------------------------------

Description: Sets bits in the given string.

Return type: bytea

Examples:

::

   SELECT set_bit(E'Th\\000omas'::bytea, 45, 0) AS RESULT;
         result
   ------------------
    \x5468006f6d4173
   (1 row)

set_byte(string,offset, newvalue)
---------------------------------

Description: Sets bytes in the given string.

Return type: bytea

Examples:

::

   SELECT set_byte(E'Th\\000omas'::bytea, 4, 64) AS RESULT;
         result
   ------------------
    \x5468006f406173
   (1 row)
