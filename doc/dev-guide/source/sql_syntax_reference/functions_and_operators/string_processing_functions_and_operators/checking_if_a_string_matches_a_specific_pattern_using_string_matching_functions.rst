:original_name: dws_06_0420.html

.. _dws_06_0420:

Checking If a String Matches a Specific Pattern Using String Matching Functions
===============================================================================

What Are Pattern Matching Functions?
------------------------------------

**LIKE** is a string comparison function used to check whether a string matches a specified pattern rule. It returns a Boolean value (**true**/**false**, represented by **t**/**f** in the example).

The differences between LIKE and equality (=) operators are as follows:

-  **=** matches the complete string exactly, including its length and content.
-  **LIKE** performs a fuzzy match using wildcards to overlook minor character variations while focusing on the main pattern

**Scenarios**: These functions are mainly used for fuzzy query. For example:

-  To find users whose names contain **Zhang**, use **name LIKE '%Zhang%';**
-  To find users whose mobile numbers start with **138**, use **phone LIKE '138%';**
-  To find users whose email addresses end with **@abc.com** and the user name length is **5**, use **email LIKE '_____@abc.com' (five underscores)**.

regexp_like(text,text,text)
---------------------------

Description: Performs a regular expression matching.

Return type: Boolean

Example:

::

   SELECT regexp_like('str','[ac]');
    regexp_like
   -------------
    f
   (1 row)

string [NOT] LIKE pattern [ESCAPE escape-character]
---------------------------------------------------

Description: Specifies the pattern matching function.

If the pattern contains neither percent signs nor underscores, it stands solely for itself, making LIKE behave like the equality operator. An underscore (_) in the pattern matches any single character, while a percent sign (%) matches zero or more characters.

To match an underscore or percent sign literally, the respective character in the pattern must be preceded by an escape character. The default escape character is a backward slash (\\) and can be specified using the **ESCAPE** clause. To match with escape characters, enter two escape characters.

Return type: boolean

Example:

::

   SELECT 'AA_BBCC' LIKE '%A@_B%' ESCAPE '@' AS RESULT;
    result
   --------
    t
   (1 row)

::

   SELECT 'AA_BBCC' LIKE '%A@_B%' AS RESULT;
    result
   --------
    f
   (1 row)

::

   SELECT 'AA@_BBCC' LIKE '%A@_B%' AS RESULT;
    result
   --------
    t
   (1 row)

REGEXP_LIKE(source_string, pattern [, match_parameter])
-------------------------------------------------------

Description: Performs a regular expression matching.

**source_string** indicates the source string and **pattern** indicates the matching pattern of the regular expression. **match_parameter** indicates the matching items and the values are as follows:

-  **i**: case-insensitive.
-  **c**: case-sensitive
-  **n**: allows the metacharacter "." in a regular expression to be matched with a newline chara
-  **m**: allows **source_string** to be regarded as multiple rows.

If **match_parameter** is ignored, case-sensitive will be used by default, meaning that "." will not be matched with a newline character and **source_string** will be regarded as a single row.

Return type: boolean

Example:

::

   SELECT regexp_like('ABC', '[A-Z]');
    regexp_like
   -------------
    t
   (1 row)

::

   SELECT regexp_like('ABC', '[D-Z]');
    regexp_like
   -------------
    f
   (1 row)

::

   SELECT regexp_like('abc', '[A-Z]','i');
    regexp_like
   -------------
    t
   (1 row)

::

   SELECT regexp_like('abc', '[A-Z]');
    regexp_like
   -------------
    f
   (1 row)
