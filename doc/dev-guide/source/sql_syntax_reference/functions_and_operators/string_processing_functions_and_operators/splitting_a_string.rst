:original_name: dws_06_0416.html

.. _dws_06_0416:

Splitting a String
==================

.. _en-us_topic_0000002515796863__section17325142812322:

regexp_split_to_array(string text, pattern text [, flags text ])
----------------------------------------------------------------

Description: Splits **string** using a POSIX regular expression as the delimiter. It is similar to **regexp_split_to_table**, but it returns the result as a text array.

Return type: text[]

Example:

::

   SELECT regexp_split_to_array('hello world', E'\\s+');
    regexp_split_to_array
   -----------------------
    {hello,world}
   (1 row)

.. _en-us_topic_0000002515796863__section9656102314320:

regexp_split_to_table(string text, pattern text [, flags text])
---------------------------------------------------------------

Description: Splits **string** using a POSIX regular expression as the delimiter. If there is no match to the pattern, the function returns the string. If there is at least one match, for each match it returns the text from the end of the last match (or the beginning of the string) to the beginning of the match. When there are no more matches, it returns the text from the end of the last match to the end of the string.

The **flags** parameter includes zero or more single-character flags that alter the function's behavior. Flag **i** specifies case-insensitive matching, while flag **g** specifies replacement of each matching substring rather than only the first one.

Return type: setof text

Example:

::

   SELECT regexp_split_to_table('hello world', E'\\s+');
    regexp_split_to_table
   -----------------------
    hello
    world
   (2 rows)

.. caution::

   Without a subquery, the **regexp_split_to_table** function removes rows that do not match any data. To keep the original rows, avoid using this function.

::

   SELECT * FROM tab;
    c1  | c2
   -----+-----
    dws |
   (1 row)

   SELECT c1, regexp_split_to_table(c2, E'\\s+') FROM tab;
    c1 | regexp_split_to_table
   ----+-----------------------
   (0 rows)

   SELECT c1, (select regexp_split_to_table(c2, E'\\s+')) FROM tab;
    c1  | regexp_split_to_table
   -----+-----------------------
    dws |
   (1 row)

split_part(string text, delimiter text, field int)
--------------------------------------------------

Description: Splits the original string into several parts based on the specified delimiter (**delimiter** parameter) and returns the given field (**field** parameter). This function is used to extract specific fields from composite strings.

Parameters:

-  **string**: original string to be split.
-  **delimiter**: delimiter used to split strings.
-  **field**: given field returned after the split. The value starts from 1.

Return type: text

Example:

Split the string **abc~@~def~@~ghi** by **~@~** to obtain three parts (**abc**, **def**, and **ghi**), and return the second part, that is, **def**.

::

   SELECT split_part('abc~@~def~@~ghi', '~@~', 2);
    split_part
   ------------
    def
   (1 row)
