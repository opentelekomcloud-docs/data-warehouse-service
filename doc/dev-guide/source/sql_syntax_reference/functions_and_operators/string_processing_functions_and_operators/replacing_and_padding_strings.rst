:original_name: dws_06_0414.html

.. _dws_06_0414:

Replacing and Padding Strings
=============================

lpad(string text, length int [, fill text])
-------------------------------------------

Description: Fills up the string to length by appending the characters fill (a space by default). If the string is already longer than length, it is truncated (on the right).

Return type: text

Example:

::

   SELECT lpad('data', 8, 'xy');
      lpad
   ----------
    xyxydata
   (1 row)

overlay(string placing string FROM int [for int])
-------------------------------------------------

Description: Replaces the part of a string starting from a specified position with a specified string. It can flexibly replace substring.

Parameters:

**FROM int**: starts the replacement from the *n*\ th character of the first string.

**for int**: (optional) number of characters to be replaced in the first string. If this parameter is not specified, all characters starting from **from** are replaced by default.

Return type: text

Example: Replace part of **abcdef** starting at position 3 with **xyz**. The result is **abxyzf**.

::

   SELECT overlay('abcdef' placing 'xyz' FROM 3);
    overlay
   ---------
    abxyzf
   (1 row)

Replace three characters starting at position 2 in **hello** with **world** to get **hworldo**.

::

   SELECT overlay('hello' placing 'world' from 2 for 3 );
    overlay
   ---------
    hworldo
   (1 row)

replace(string text, from text, to text)
----------------------------------------

Description: Replaces all parts that match the **from** parameter in the original string with the content specified by the **to** parameter.

.. note::

   The case is sensitive, so you need to ensure that the case of **from text** and **to text** matches that of the original string.

Return type: text

Example:

::

   SELECT replace('databasedata', 'data', 'xxx');
     replace
   ------------
    xxxbasexxx
   (1 row)

rpad(string text, length int [, fill text])
-------------------------------------------

Description: Fills up **string** to **length** by appending the characters **fill**. If the string is longer than the specified length, the string is truncated on the right.

Parameters:

-  **string**: original string to be processed.
-  **length**: length of the target string.
-  **fill** (optional): character or string used for filling. The default value is a space.

Return type: text

Example:

Fill the original string (**data**) of length 4 to a target length of 10 using the fill characters **xy**.

::

   SELECT rpad('data', 10, 'xy');
       rpad
   ------------
    dataxyxyxy
   (1 row)

Fill the original string **database** using the fill characters **xy**. The string **database** is longer than the target length **4**, and the string is truncated from the end. The returned string is **data**.

::

   SELECT rpad('database',4,'xy');
    rpad
   ------
    data
   (1 row)

rtrim(string text [, characters text])
--------------------------------------

Description: Removes the specified characters from the right (end) of a string.

Parameters:

-  **string**: original string to be processed.
-  **characters** (optional): set of characters to be removed from the right of the string. By default, all blank characters (for example, spaces) are removed.

Return type: text

Example:

If the parameter **characters** is not specified, all blank characters on the right of the string **Database** are removed by default.

::

   SELECT rtrim('Database    ');
     rtrim
   ----------
    Database
   (1 row)

Remove the specified character **x** from the string.

::

   SELECT rtrim('trimxxxx', 'x');
    rtrim
   -------
    trim
   (1 row)

rpad(string varchar, length int [, fill varchar])
-------------------------------------------------

Description: Adds specified characters to the right of a string until the string reaches the specified length. If the string is longer than the specified length, the string is truncated on the right.

Parameters:

-  **string**: original string to be processed.
-  **length**: length of the target string. **length** in DWS indicates the character length. One Chinese character is counted as one character.
-  **fill** (optional): character or string used for filling. The default value is a space.

Return type: varchar

Example:

::

   SELECT rpad('data',10,'xyz');
    rpad
   ------------
    dataxyzxyz
   (1 row)

::

   SELECT rpad('hi',5,'abcdefg');
    rpad
   -------
    hiabc
   (1 row)

replace(string varchar, search_string varchar, replacement_string varchar)
--------------------------------------------------------------------------

Description: Searches for a specific substring (**search_string**) in a string and replaces it with another string (**replacement_string**).

Parameters:

-  **string**: original string to be processed.
-  **search_string**: substring to be replaced.
-  **replacement_string**: new substring used to replace **search_string**.

Return type: varchar

Example:

Replace all substrings **ab** in the string **ababab** **and mabab** with **cd**.

::

   SELECT replace('ababab and mabab','ab','cd');
        replace
   ------------------
    cdcdcd and mcdcd
   (1 row)

lpad(string varchar, length int[, repeat_string varchar])
---------------------------------------------------------

Description: Adds specified characters to the left of a string until the string reaches the specified length. If the length of the original string is greater than the target length (**length parameter**), the target string is truncated from the right to the string of the **length**.

Parameters:

-  **string**: original string to be padded.
-  **length**: length of the target string.
-  **repeat_string**: (optional) characters used for padding. By default, spaces are used for padding.

Return type: varchar

Example:

Set original string length (**base**) to **4** and the target length to **8**, and use asterisks (``*``) to pad four characters. After the padding, the string becomes **\****base**.

::

   SELECT lpad('base',8,'*');
      lpad
   ----------
    ****base
   (1 row)

Set original string length (**database**) to **8** and the target length to **6**, and truncate the string **database** from the right to a 6-character string. **databa** is returned.

::

   SELECT lpad('database',6,'data');
     lpad
   --------
    databa
   (1 row)

.. _en-us_topic_0000002515797145__section1287153982819:

regexp_replace(string, pattern, replacement [,flags ])
------------------------------------------------------

Description: Replaces substring matching POSIX regular expression. The source string is returned unchanged if there is no match to the pattern. If a match is found, the matching substring in the returned string is replaced with the **replacement** string.

The **replacement** string can contain **\\n**, where **\\n** ranges from 1 to 9, indicating that the substring of the *n*\ th parenthesized subexpression in the matching pattern within the string should be inserted. Additionally, it can contain **\\&**, indicating that the substring matching the entire pattern should be inserted.

**flags** (optional parameter) contains zero or multiple single-letter flags that change function behavior. The following table lists the options of **flags**.

.. table:: **Table 1** Options of the flags parameter

   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Option                            | Description                                                                                                                                                                                                                                                                                                                       |
   +===================================+===================================================================================================================================================================================================================================================================================================================================+
   | g                                 | Replaces all the matched substrings. (By default, only the first matched substring is replaced.)                                                                                                                                                                                                                                  |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | B                                 | By default, uses the Henry Spencer regex library and its regex syntax. Specifying the B option prioritizes using the Boost.Regex library and its regex syntax.                                                                                                                                                                    |
   |                                   |                                                                                                                                                                                                                                                                                                                                   |
   |                                   | In the following cases, the Henry Spencer's regular expression library and its regular expression syntax will be used even if this option is specified:                                                                                                                                                                           |
   |                                   |                                                                                                                                                                                                                                                                                                                                   |
   |                                   | -  Any combination of **p**, **q**, **w**, and **x** characters is included in the **flags**.                                                                                                                                                                                                                                     |
   |                                   | -  The **string** or **pattern** parameters contain multibyte characters.                                                                                                                                                                                                                                                         |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | b                                 | Indicates POSIX Basic Regular Expressions (BREs) for matching.                                                                                                                                                                                                                                                                    |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | c                                 | Indicates the case-sensitive matching.                                                                                                                                                                                                                                                                                            |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | e                                 | Indicates POSIX Extended Regular Expressions (EREs) for matching. When neither **b** nor **e** is specified, if the Henry Spencer regex library is chosen, it follows the Advanced Regular Expressions (AREs), similar to Perl Compatible Regular Expressions (PCREs). If the Boost.Regex library is chosen, it adheres to PCREs. |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | i                                 | Indicates the case-insensitive matching.                                                                                                                                                                                                                                                                                          |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | m                                 | Indicates the newline-sensitive matching, synonymous with the **n** option.                                                                                                                                                                                                                                                       |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n                                 | Indicates the newline-sensitive matching. When this option is active, the newline character influences the matching of metacharacters (., ^, $, and [^).                                                                                                                                                                          |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | p                                 | Indicates the partially newline-sensitive matching. When this option is active, the newline character influences the matching of metacharacters (. and [^), partially compared to the **n** option.                                                                                                                               |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | q                                 | Resets the regular expression to a double-quoted text string, treating all characters as literals.                                                                                                                                                                                                                                |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | s                                 | Indicates the non-newline-sensitive matching.                                                                                                                                                                                                                                                                                     |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | t                                 | Indicates the tight syntax (default). When this option takes effect, all characters matter.                                                                                                                                                                                                                                       |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | w                                 | Indicates the inverse partially newline-sensitive matching. When this option is active, the newline character influences the matching of metacharacters (^ and $), inversely partially compared to the **n** option.                                                                                                              |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | x                                 | Indicates the expanded syntax. In contrast to the tight syntax, whitespace characters in regular expressions are ignored in the expanded syntax. Whitespace characters include spaces, horizontal tabs, newlines, and any characters belonging to the space character set.                                                        |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return type: varchar

Example:

::

   SELECT regexp_replace('Thomas', '.[mN]a.', 'M');
    regexp_replace
   ----------------
    ThM
   (1 row)
   SELECT regexp_replace('foobarbaz','b(..)', E'X\\1Y', 'g') AS RESULT;
      result
   -------------
    fooXarYXazY
   (1 row)

SPACE(n int)
------------

Description: Returns a string consisting of **n** spaces. If the parameter contains NULL, NULL will be returned.

Return type: text

Example:

::

   SELECT SPACE(2) as result;
    result
   --------

   (1 row)
