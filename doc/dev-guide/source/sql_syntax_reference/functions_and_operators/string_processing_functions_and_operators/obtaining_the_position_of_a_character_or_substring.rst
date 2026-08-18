:original_name: dws_06_0419.html

.. _dws_06_0419:

Obtaining the Position of a Character or Substring
==================================================

instr(text,text,int,int)
------------------------

Description: Obtains the position of a substring in the string. If no match is found, **0** is returned.

-  First **text**: (mandatory) target string.
-  Second **text**: (mandatory) substring to be matched.
-  First **int**: start position of the matching. If this parameter is not set, the matching starts from the beginning by default.
-  Second **int**: which occurrence of the substring the function should return the position for. If this parameter is not set, the first occurrence is used by default.

Return type: integer

Example:

Obtain the position of the 2nd occurrence of **bcd** starting from position 2 in the main string **abcdabcdabcd**.

::

   SELECT instr( 'abcdabcdabcd', 'bcd', 2, 2 );
    instr
   -------
        6
   (1 row)

   SELECT instr( 'abcdabcdabcd', 'lmk', 2);
    instr
   -------
        0
   (1 row)

position(substring in string)
-----------------------------

Description: Returns the position of a specified substring in the string. If the string does not contain substrings, **0** is returned.

Return type: integer

Example:

::

   SELECT position('ing' in 'string');
    position
   ----------
           4
   (1 row)

   SELECT position('ing' in 'strin');
    position
   ----------
          0
   (1 row)

strpos(string, substring)
-------------------------

Description: Obtains the position of the first occurrence of the specified substring in the string. If the substring does not exist in the string, **0** is returned.

Return type: integer

Example:

::

   SELECT strpos('source', 'rc');
    strpos
   --------
         4
   (1 row)

   SELECT strpos('source', 'data');
    strpos
   --------
         0
   (1 row)

instr(string,substring[,position,occurrence])
---------------------------------------------

Description: Queries and returns the value of the substring **position** that occurs the **occurrence** (first by default) times from the **position** (1 by default) of the **string**.

-  If the value of **position** is **0**, **0** is returned.
-  If the value of **position** is negative, searches backwards from the last *n*\ th character in the string, in which *n* indicates the absolute value of **position**.

The unit is character. One Chinese character is one character.

Return type: integer

Example:

::

   SELECT instr('corporate floor','or', 3);
    instr
   -------
        5
   (1 row)

::

   SELECT instr('corporate floor','or',-3,2);
    instr
   -------
        2
   (1 row)

locate(substring,string[,position])
-----------------------------------

Description: Queries and returns the value of the substring **position** that appears for the first time from the position specified by **position** (1 by default) of **string**. The unit is character. If no substring exists in the **string**, **0** is returned.

Return type: integer

Example:

::

   SELECT locate('ball','football');
    locate
   --------
        5
   (1 row)

::

   SELECT locate('er','soccerplayer','6');
    locate
   --------
       11
   (1 row)
