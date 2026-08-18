:original_name: dws_06_0413.html

.. _dws_06_0413:

Concatenating Strings
=====================

rawcat(raw,raw)
---------------

Description: Concatenates the given strings.

Return type: raw

Example:

::

   SELECT rawcat('ab','cd');
    rawcat
   --------
    ABCD
   (1 row)

repeat(string text, number int)
-------------------------------

Description: Outputs a string repeatedly for a specified number of times. If **number int** is set to zero or a negative value, an empty string is returned.

Return type: text

Example:

::

   SELECT repeat('abc', 4);
       repeat
   --------------
    abcabcabcabc
   (1 row)

string \|\| string
------------------

Description: Concatenates strings.

Return type: text

Example:

::

   SELECT 'DA'||'TABASE' AS RESULT;
    result
   ----------
    DATABASE
   (1 row)

string \|\| non-string or non-string \|\| string
------------------------------------------------

Description: Concatenates strings and non-strings.

Return type: text

Example:

::

   SELECT 'Value: '||123 AS RESULT;
      result
   ------------
    Value: 123
   (1 row)

concat(str1,str2)
-----------------

Description: Connects **str1** and **str2** and returns the string.

-  In the ORA- or TD-compatible mode, a combination of all the non-null strings is returned.
-  In the MySQL-compatible mode, **NULL** is returned if an input string is **NULL**.

Return type: varchar

Example:

::

   SELECT concat('Hello', ' World!');
       concat
   --------------
    Hello World!
   (1 row)

concat_ws(sep text, str"any" [, str"any" [, ...] ])
---------------------------------------------------

Description: Concatenates all but the first parameter with separators. The first parameter is used as the separator string.

Return type: text

Example:

::

   SELECT concat_ws(',', 'ABCDE', 2, NULL, 22);
    concat_ws
   ------------
    ABCDE,2,22
   (1 row)
