:original_name: dws_06_0030.html

.. _dws_06_0030:

Character Processing Functions and Operators
============================================

String functions and operators provided by GaussDB(DWS) are for concatenating strings with each other, concatenating strings with non-strings, and matching the patterns of strings.

bit_length(string)
------------------

Description: Returns the length of the given string in bits.

Return type: integer

Examples:

::

   SELECT bit_length('world');
    bit_length
   ------------
            40
   (1 row)

btrim(string text [, characters text])
--------------------------------------

Description: Removes the longest string consisting only of characters in **characters** (a space by default) from the start and end of **string**.

Return type: text

Examples:

::

   SELECT btrim('sring' , 'ing');
    btrim
   -------
    sr
   (1 row)

char_length(string) or character_length(string)
-----------------------------------------------

Description: Returns the number of characters in a string.

Return type: integer

Examples:

::

   SELECT char_length('hello');
    char_length
   -------------
              5
   (1 row)

instr(text,text,int,int)
------------------------

Description: **FROM int** indicates the start position of the replacement in the first string. **for int** indicates the number of characters replaced in the first string.

Return type: integer

Examples:

::

   SELECT instr( 'abcdabcdabcd', 'bcd', 2, 2 );
    instr
   -------
        6
   (1 row)

lengthb(text/bpchar)
--------------------

Description: Returns the number of bytes of a specified string.

Return type: integer

Examples:

::

   SELECT lengthb('hello');
    lengthb
   ---------
          5
   (1 row)

.. note::

   -  For a string containing newline characters, for example, a string consisting of a newline character and a space, the value of **length** and **lengthb** in GaussDB(DWS) is 2.
   -  In GaussDB(DWS), *n* of the CHAR(n) type indicates the number of characters. Therefore, for multiple-octet coded character sets, the length returned by the LENGTHB function may be longer than *n*.

left(str text, n int)
---------------------

Description: Returns first **n** characters in the string.

-  In the ORA- or TD-compatible mode, all but the last **\|n\|** characters are returned if **n** is negative.
-  In the MySQL-compatible mode, an empty string is returned if **n** is negative.

Return type: text

Examples:

::

   SELECT left('abcde', 2);
    left
   ------
    ab
   (1 row)

length(string bytea, encoding name)
-----------------------------------

Description: Returns the number of characters in **string** in the given **encoding**. The **string** must be valid in this encoding.

Return type: integer

Examples:

::

   SELECT length('jose', 'UTF8');
    length
   --------
         4
   (1 row)

lpad(string text, length int [, fill text])
-------------------------------------------

Description: Fills up the string to the specified length by appending the characters **fill** (a space by default). If the **string** is already longer than **length** then it is truncated (on the right).

Return type: text

Examples:

::

   SELECT lpad('hi', 5, 'xyza');
    lpad
   -------
    xyzhi
   (1 row)

octet_length(string)
--------------------

Description: Returns the number of bytes in the given string.

Return type: integer

Examples:

::

   SELECT octet_length('jose');
    octet_length
   --------------
               4
   (1 row)

overlay(string placing string FROM int [for int])
-------------------------------------------------

Description: Replaces substring. **FROM int** indicates the start position of the replacement in the first string. **for int** indicates the number of characters replaced in the first string.

Return type: text

Examples:

::

   SELECT overlay('hello' placing 'world' from 2 for 3 );
    overlay
   ---------
    hworldo
   (1 row)

position(substring in string)
-----------------------------

Description: Specifies the position of the substring in the string. If the string does not contain substrings, 0 is returned.

Return type: integer

Examples:

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

pg_client_encoding()
--------------------

Description: Returns the encoded client name.

Return type: name

Examples:

::

   SELECT pg_client_encoding();
    pg_client_encoding
   --------------------
    UTF8
   (1 row)

quote_ident(string text)
------------------------

Description: Returns the given string suitably quoted to be used as an identifier in an SQL statement string (quotation marks are used as required). Quotes are added only if necessary (that is, if the string contains non-identifier characters or would be case-folded). Embedded quotes are properly doubled.

Return type: text

Examples:

::

   SELECT quote_ident('hello world');
    quote_ident
   --------------
    "hello world"
   (1 row)

quote_literal(string text)
--------------------------

Description: Returns the given string suitably quoted to be used as a string literal in an SQL statement string (quotation marks are used as required).

Return type: text

Examples:

::

   SELECT quote_literal('hello');
    quote_literal
   ---------------
    'hello'
   (1 row)

If command similar to the following exists, text will be escaped.

::

   SELECT quote_literal(E'O\'hello');
    quote_literal
   ---------------
    'O''hello'
   (1 row)

If command similar to the following exists, backslash will be properly doubled.

::

   SELECT quote_literal('O\hello');
    quote_literal
   ---------------
    E'O\\hello'
   (1 row)

If the parameter is null, return **NULL**. If the parameter may be null, you are advised to use **quote_nullable**.

::

   SELECT quote_literal(NULL);
    quote_literal
   ---------------

   (1 row)

quote_literal(value anyelement)
-------------------------------

Description: Converts the given value to text and then quotes it as a literal.

Return type: text

Examples:

::

   SELECT quote_literal(42.5);
    quote_literal
   ---------------
    '42.5'
   (1 row)

If command similar to the following exists, the given value will be escaped.

::

   SELECT quote_literal(E'O\'42.5');
    quote_literal
   ---------------
    'O''42.5'
   (1 row)

If command similar to the following exists, backslash will be properly doubled.

::

   SELECT quote_literal('O\42.5');
    quote_literal
   ---------------
    E'O\\42.5'
   (1 row)

quote_nullable(string text)
---------------------------

Description: Returns the given string suitably quoted to be used as a string literal in an SQL statement string (quotation marks are used as required).

Return type: text

Examples:

::

   SELECT quote_nullable('hello');
    quote_nullable
   ----------------
    'hello'
   (1 row)

If command similar to the following exists, text will be escaped.

::

   SELECT quote_nullable(E'O\'hello');
    quote_nullable
   ----------------
    'O''hello'
   (1 row)

If command similar to the following exists, backslash will be properly doubled.

::

   SELECT quote_nullable('O\hello');
    quote_nullable
   ----------------
    E'O\\hello'
   (1 row)

If the parameter is null, return **NULL**.

::

   SELECT quote_nullable(NULL);
    quote_nullable
   ----------------
    NULL
   (1 row)

quote_nullable(value anyelement)
--------------------------------

Description: Converts the given value to text and then quotes it as a literal.

Return type: text

Examples:

::

   SELECT quote_nullable(42.5);
    quote_nullable
   ----------------
    '42.5'
   (1 row)

If command similar to the following exists, the given value will be escaped.

::

   SELECT quote_nullable(E'O\'42.5');
    quote_nullable
   ----------------
    'O''42.5'
   (1 row)

If command similar to the following exists, backslash will be properly doubled.

::

   SELECT quote_nullable('O\42.5');
    quote_nullable
   ----------------
    E'O\\42.5'
   (1 row)

If the parameter is null, return **NULL**.

::

   SELECT quote_nullable(NULL);
    quote_nullable
   ----------------
    NULL
   (1 row)

substring(string [from int] [for int])
--------------------------------------

Description: Extracts a substring. **from int** indicates the start position of the truncation. **for int** indicates the number of characters truncated.

Return type: text

Examples:

::

   SELECT substring('Thomas' from 2 for 3);
    substring
   -----------
    hom
   (1 row)

.. _en-us_topic_0000001811634661__section13931191583319:

substring(string from *pattern*)
--------------------------------

Description: Extracts substring matching POSIX regular expression. It returns the text that matches the pattern. If no match record is found, a null value is returned.

Return type: text

Examples:

::

   SELECT substring('Thomas' from '...$');
    substring
   -----------
    mas
   (1 row)
   SELECT substring('foobar' from 'o(.)b');
    substring
   --------
    o
   (1 row)
   SELECT substring('foobar' from '(o(.)b)');
    substring
   --------
    oob
   (1 row)

.. note::

   If the POSIX pattern contains any parentheses, the portion of the text that matched the first parenthesized sub-expression (the one whose left parenthesis comes first) is returned. You can put parentheses around the whole expression if you want to use parentheses within it without triggering this exception.

.. _en-us_topic_0000001811634661__section12598113333:

substring(string from *pattern* for *escape*)
---------------------------------------------

Description: Extracts substring matching SQL regular expression. The specified pattern must match the entire data string, or else the function fails and returns null. To indicate the part of the pattern that should be returned on success, the pattern must contain two occurrences of the escape character followed by a double quote ("). The text matching the portion of the pattern between these markers is returned.

Return type: text

Examples:

::

   SELECT substring('Thomas' from '%#"o_a#"_' for '#');
    substring
   -----------
    oma
   (1 row)

rawcat(raw,raw)
---------------

Description: Concatenates the given strings.

Return type: raw

Examples:

::

   SELECT rawcat('ab','cd');
    rawcat
   --------
    ABCD
   (1 row)

regexp_like(text,text,text)
---------------------------

Description: Performs a regular expression matching.

Return type: bool

Examples:

::

   SELECT regexp_like('str','[ac]');
    regexp_like
   -------------
    f
   (1 row)

regexp_substr(text,text)
------------------------

Description: Extracts substrings from a regular expression. Its function is similar to **substr**. When a regular expression contains multiple parallel brackets, it also needs to be processed.

Return type: text

Examples:

::

   SELECT regexp_substr('str','[ac]');
    regexp_substr
   ---------------

   (1 row)

.. _en-us_topic_0000001811634661__section1740918406323:

regexp_matches(string text, pattern text [, flags text])
--------------------------------------------------------

Description: Returns all captured substrings resulting from matching a POSIX regular expression against the **string**. If the pattern does not match, the function returns no rows. If the pattern contains no parenthesized sub-expressions, then each row returned is a single-element text array containing the substring matching the whole pattern. If the pattern contains parenthesized sub-expressions, the function returns a text array whose *n*\ th element is the substring matching the *n*\ th parenthesized sub-expression of the pattern.

The optional **flags** argument contains zero or multiple single-letter flags that change function behavior. **i** indicates that the matching is not related to uppercase and lowercase. **g** indicates that each matching substring is replaced, instead of replacing only the first one.

.. important::

   If the last parameter is provided but the parameter value is an empty string ('') and the SQL compatibility mode of the database is set to ORA, the returned result is an empty set. This is because the ORA compatible mode treats the empty string ('') as **NULL**. To resolve this problem, you can:

   -  Change the database SQL compatibility mode to TD.
   -  Do not provide the last parameter or do not set the last parameter to an empty string.

Return type: setof text[]

::

   SELECT regexp_matches('foobarbequebaz', '(bar)(beque)');
    regexp_matches
   ----------------
    {bar,beque}
   (1 row)

   SELECT regexp_matches('foobarbequebaz', 'barbeque');
    regexp_matches
   ----------------
    {barbeque}
   (1 row)

   SELECT regexp_matches('foobarbequebazilbarfbonk', '(b[^b]+)(b[^b]+)', 'g');
       regexp_matches
   --------------
    {bar,beque}
    {bazil,barf}
   (2 rows)

.. caution::

   If there is no subquery, the table data will not be displayed if there is no match for the **regexp_matches** function. This outcome is generally undesirable and should be avoided. It is recommended to use the **regexp_substr** function to achieve the same functionality.

::

   SELECT * FROM tab;
    c1  | c2
   -----+-----
    dws | dws
   (1 row)

   SELECT c1, regexp_matches(c2, '(bar)(beque)') FROM tab;
    c1 | regexp_matches
   ----+----------------
   (0 rows)

   SELECT c1, c2, (SELECT regexp_matches(c2, '(bar)(beque)')) FROM tab;
    c1  | c2  | regexp_matches
   -----+-----+----------------
    dws | dws |
   (1 row)

.. _en-us_topic_0000001811634661__section17325142812322:

regexp_split_to_array(string text, pattern text [, flags text ])
----------------------------------------------------------------

Description: Splits **string** using a POSIX regular expression as the delimiter. The regexp_split_to_array function behaves the same as regexp_split_to_table, except that regexp_split_to_array returns its result as an array of text.

Return type: text[]

Examples:

::

   SELECT regexp_split_to_array('hello world', E'\\s+');
    regexp_split_to_array
   -----------------------
    {hello,world}
   (1 row)

.. _en-us_topic_0000001811634661__section9656102314320:

regexp_split_to_table(string text, pattern text [, flags text])
---------------------------------------------------------------

Description: Splits **string** using a POSIX regular expression as the delimiter. If there is no match to the pattern, the function returns the string. If there is at least one match, for each match it returns the text from the end of the last match (or the beginning of the string) to the beginning of the match. When there are no more matches, it returns the text from the end of the last match to the end of the string.

The **flags** parameter is a text string containing zero or more single-letter flags that change the function's behavior. **i** indicates that the matching is not related to uppercase and lowercase. **g** indicates that each matching substring is replaced, instead of replacing only the first one.

Return type: setof text

Examples:

::

   SELECT regexp_split_to_table('hello world', E'\\s+');
    regexp_split_to_table
   -----------------------
    hello
    world
   (2 rows)

.. caution::

   When a subquery is absent, and the **regexp_split_to_table** function fails to find a match, the table data will not be displayed. This outcome is generally undesirable and should be avoided.

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

repeat(string text, number int)
-------------------------------

Description: Outputs a string repeatedly for a specified number of times.

Return type: text

Examples:

::

   SELECT repeat('Pg', 4);
     repeat
   ----------
    PgPgPgPg
   (1 row)

replace(string text, from text, to text)
----------------------------------------

Description: Replaces all contents of substring **from** in **string** and replaces them with the contents of substring **to**.

Return type: text

Examples:

::

   SELECT replace('abcdefabcdef', 'cd', 'XXX');
       replace
   ----------------
    abXXXefabXXXef
   (1 row)

reverse(str)
------------

Description: Returns reversed string.

Return type: text

Examples:

::

   SELECT reverse('abcde');
    reverse
   ---------
    edcba
   (1 row)

right(str text, n int)
----------------------

Description: Returns the last **n** characters in the string.

-  In the ORA- or TD-compatible mode, all but the last **\|n\|** characters are returned if **n** is negative.
-  In the MySQL-compatible mode, an empty string is returned if **n** is negative.

Return type: text

Examples:

::

   SELECT right('abcde', 2);
    right
   -------
    de
   (1 row)

   SELECT right('abcde', -2);
    right
   -------
    cde
   (1 row)

rpad(string text, length int [, fill text])
-------------------------------------------

Description: Fills up the string to length by appending the characters fill (a space by default). If the string is already longer than length then it is truncated.

Return type: text

Examples:

::

   SELECT rpad('hi', 5, 'xy');
    rpad
   -------
    hixyx
   (1 row)

rtrim(string text [, characters text])
--------------------------------------

Description: Removes the longest string containing only characters from characters (a space by default) from the end of string.

Return type: text

Examples:

::

   SELECT rtrim('trimxxxx', 'x');
    rtrim
   -------
    trim
   (1 row)

sys_context ('namespace' , 'parameter')
---------------------------------------

Description: Returns the parameter values of a specified **namespace**.

Return type: text

Examples:

::

   SELECT SYS_CONTEXT ( 'postgres' , 'archive_mode');
    sys_context
   -------------

   (1 row)

substrb(text,int,int)
---------------------

Description: Extracts a substring. The first **int** indicates the start position of the subtraction. The second **int** indicates the number of bytes subtracted.

Return type: text

Examples:

::

   SELECT substrb('string',2,3);
    substrb
   ---------
    tri
   (1 row)

substrb(text,int)
-----------------

Description: Extracts a substring. **int** indicates the start position of the subtraction.

Return type: text

Examples:

::

   SELECT substrb('string',2);
    substrb
   ---------
    tring
   (1 row)

string \|\| string
------------------

Description: Concatenates strings.

Return type: text

Examples:

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

Examples:

::

   SELECT 'Value: '||42 AS RESULT;
     result
   -----------
    Value: 42
   (1 row)

split_part(string text, delimiter text, field int)
--------------------------------------------------

Description: Splits **string** on **delimiter** and returns the **field**\ th column (counting from text of the first appeared delimiter).

Return type: text

Examples:

::

   SELECT split_part('abc~@~def~@~ghi', '~@~', 2);
    split_part
   ------------
    def
   (1 row)

strpos(string, substring)
-------------------------

Description: Specifies the position of a substring. It is the same as **position(substring in string)**. However, the parameter sequences of them are reversed.

Return type: integer

Examples:

::

   SELECT strpos('source', 'rc');
    strpos
   --------
         4
   (1 row)

to_hex(number int or bigint)
----------------------------

Description: Converts the given number to a hexadecimal expression.

Return type: text

Examples:

::

   SELECT to_hex(2147483647);
     to_hex
   ----------
    7fffffff
   (1 row)

translate(string text, from text, to text)
------------------------------------------

Description: Converts any character in **string** that matches a character in the **from** set into the corresponding character in the **to** set. If **from** is longer than **to**, extra characters occurred in **from** are removed.

Return type: text

Examples:

::

   SELECT translate('12345', '143', 'ax');
    translate
   -----------
    a2x5
   (1 row)

length(string)
--------------

Description: Returns the number of characters in a string.

Return type: integer

Examples:

::

   SELECT length('abcd');
    length
   --------
         4
   (1 row)

lengthb(string)
---------------

Description: Returns the number of characters in a string. The value depends on character sets (GBK and UTF8).

Return type: integer

Examples:

::

   SELECT lengthb('hello');
    lengthb
   ---------
          5
   (1 row)

substr(string,from)
-------------------

Description:

Extracts substrings from a string.

**from** indicates the start position of the extraction.

-  If **from** starts at 0, the value **1** is used.
-  If the value of **from** is positive, all characters from **from** to the end are extracted.
-  If the value of **from** is negative, the last n characters in the string are extracted, in which n indicates the absolute value of **from**.

Return type: varchar

Examples:

If the value of **from** is positive:

::

   SELECT substr('ABCDEF',2);
    substr
   --------
    BCDEF
   (1 row)

If the value of **from** is negative:

::

   SELECT substr('ABCDEF',-2);
    substr
   --------
    EF
   (1 row)

substr(string,from,count)
-------------------------

Description:

Extracts substrings from a string.

**from** indicates the start position of the extraction.

"count" indicates the length of the extracted substring.

-  If **from** starts at 0, the value **1** is used.
-  If the value of **from** is positive, extract **count** characters starting from **from**.
-  If the value of **from** is negative, extract the last **n** **count** characters in the string, in which **n** indicates the absolute value of **from**.
-  If the value of "count" is smaller than 1, null is returned.

Return type: varchar

Examples:

If the value of **from** is positive:

::

   SELECT substr('ABCDEF',2,2);
    substr
   --------
    BC
   (1 row)

If the value of **from** is negative:

::

   SELECT substr('ABCDEF',-3,2);
    substr
   --------
    DE
   (1 row)

substrb(string,from)
--------------------

Description: The functionality of this function is the same as that of **SUBSTR(string,from)**. However, the calculation unit is byte.

Return type: bytea

Examples:

::

   SELECT substrb('ABCDEF',-2);
    substrb
   ---------
    EF
   (1 row)

substrb(string,from,count)
--------------------------

Description: The functionality of this function is the same as that of **SUBSTR(string,from,count)**. However, the calculation unit is byte.

Return type: bytea

Examples:

::

   SELECT substrb('ABCDEF',2,2);
    substrb
   ---------
    BC
   (1 row)

trim([leading \|trailing \|both] [characters] from string)
----------------------------------------------------------

Description: Removes the longest string containing only the characters (a space by default) from the start/end/both ends of the string.

Return type: varchar

Examples:

::

   SELECT trim(BOTH 'x' FROM 'xTomxx');
    btrim
   -------
    Tom
   (1 row)

::

   SELECT trim(LEADING 'x' FROM 'xTomxx');
    ltrim
   -------
    Tomxx
   (1 row)

::

   SELECT trim(TRAILING 'x' FROM 'xTomxx');
    rtrim
   -------
    xTom
   (1 row)

rtrim(string [, characters])
----------------------------

Description: Removes the longest string containing only characters from characters (a space by default) from the end of string.

Return type: varchar

Examples:

::

   SELECT rtrim('TRIMxxxx','x');
    rtrim
   -------
    TRIM
   (1 row)

ltrim(string [, characters])
----------------------------

Description: Removes the longest string containing only characters from characters (a space by default) from the start of string.

Return type: varchar

Examples:

::

   SELECT ltrim('xxxxTRIM','x');
    ltrim
   -------
    TRIM
   (1 row)

upper(string)
-------------

Description: Converts the string into the uppercase.

Return type: varchar

Examples:

::

   SELECT upper('tom');
    upper
   -------
    TOM
   (1 row)

ucase(string)
-------------

Description: Converts the string into the uppercase.

Return type: varchar

Examples:

.. code-block::

   SELECT ucase('sam');
    ucase
   -------
    SAM
   (1 row)

lower(string)
-------------

Description: Converts the string into the lowercase.

Return type: varchar

Examples:

::

   SELECT lower('TOM');
    lower
   -------
    tom
   (1 row)

lcase(string)
-------------

Description: Converts the string into the lowercase.

Return type: varchar

Examples:

.. code-block::

   SELECT lcase('SAM');
    lcase
   -------
    sam
   (1 row)

rpad(string varchar, length int [, fill varchar])
-------------------------------------------------

Description: Fills up the string to length by appending the characters fill (a space by default). If the string is already longer than length then it is truncated.

**length** in GaussDB(DWS) indicates the character length. One Chinese character is counted as one character.

Return type: varchar

Examples:

::

   SELECT rpad('hi',5,'xyza');
    rpad
   -------
    hixyz
   (1 row)

::

   SELECT rpad('hi',5,'abcdefg');
    rpad
   -------
    hiabc
   (1 row)

instr(string,substring[,position,occurrence])
---------------------------------------------

Description: Queries and returns the value of the substring position that occurs the occurrence (first by default) times from the position (1 by default) in the string.

-  If the value of **position** is **0**, **0** is returned.
-  If the value of **position** is negative, searches backwards from the last *n*\ th character in the string, in which *n* indicates the absolute value of **position**.

In this function, the calculation unit is character. One Chinese character is one character.

Return type: integer

Examples:

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

Description: From the specified **position** (**1** by default) in the string on, queries and returns the value of **position** where the substring occurs for the first time. The unit is character. If the string does not contain substrings, 0 is returned.

Return type: integer

Examples:

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

initcap(string)
---------------

Description: Converts the first letter of each word in the string into the uppercase and the other letters into the lowercase.

Return type: text

Examples:

::

   SELECT initcap('hi THOMAS');
     initcap
   -----------
    Hi Thomas
   (1 row)

ascii(string)
-------------

Description: Returns the ASCII code of the first character in the string.

Return type: integer

Examples:

::

   SELECT ascii('xyz');
    ascii
   -------
      120
   (1 row)

replace(string varchar, search_string varchar, replacement_string varchar)
--------------------------------------------------------------------------

Description: Replaces all **search-string** in the string with **replacement_string**.

Return type: varchar

Examples:

::

   SELECT replace('jack and jue','j','bl');
       replace
   ----------------
    black and blue
   (1 row)

lpad(string varchar, length int[, repeat_string varchar])
---------------------------------------------------------

Description: Adds a series of **repeat_string** (a space by default) on the left of the string to generate a new string with the total length of n.

If the length of the string is longer than the specified length, the function truncates the string and returns the substrings with the specified length.

Return type: varchar

Examples:

::

   SELECT lpad('PAGE 1',15,'*.');
         lpad
   -----------------
    *.*.*.*.*PAGE 1
   (1 row)

::

   SELECT lpad('hello world',5,'abcd');
    lpad
   -------
    hello
   (1 row)

concat(str1,str2)
-----------------

Description: Connects str1 and str2 and returns the string.

-  In the ORA- or TD-compatible mode, a combination of all the non-null strings is returned.
-  In the MySQL-compatible mode, **NULL** is returned if an input string is **NULL**.

Return type: varchar

Examples:

::

   SELECT concat('Hello', ' World!');
       concat
   --------------
    Hello World!
   (1 row)

chr(integer)
------------

Description: Returns the character of the ASCII code.

Return type: varchar

Examples:

::

   SELECT chr(65);
    chr
   -----
    A
   (1 row)

regexp_substr(source_char, pattern)
-----------------------------------

Description: Extracts substrings from a regular expression.

Return type: varchar

Examples:

::

   SELECT regexp_substr('500 Hello World, Redwood Shores, CA', ',[^,]+,') "REGEXPR_SUBSTR";
     REGEXPR_SUBSTR
   -------------------
    , Redwood Shores,
   (1 row)

.. _en-us_topic_0000001811634661__section1287153982819:

regexp_replace(string, pattern, replacement [,flags ])
------------------------------------------------------

Description: Replaces substring matching POSIX regular expression. The source string is returned unchanged if there is no match to the pattern. If there is a match, the source string is returned with the replacement string substituted for the matching substring.

The replacement string can contain \\n, where n is 1 through 9, to indicate that the source substring matching the *n*\ th parenthesized sub-expression of the pattern should be inserted, and it can contain \\& to indicate that the substring matching the entire pattern should be inserted.

The optional **flags** argument contains zero or multiple single-letter flags that change function behavior. The following table lists the options of the **flags** argument.

.. table:: **Table 1** Options of the flags argument

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Option                            | Description                                                                                                                                                                                                                                                                                                                                                                                                      |
   +===================================+==================================================================================================================================================================================================================================================================================================================================================================================================================+
   | g                                 | Replace all the matched substrings. (By default, only the first matched substring is replaced.)                                                                                                                                                                                                                                                                                                                  |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | B                                 | Preferentially use the boost regex regular expression library and its regular expression syntax. By default, the Henry Spencer's regular expression library and its regular expression syntax are used.                                                                                                                                                                                                          |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                  |
   |                                   | In the following cases, the Henry Spencer's regular expression library and its regular expression syntax will be used even if this option is specified:                                                                                                                                                                                                                                                          |
   |                                   |                                                                                                                                                                                                                                                                                                                                                                                                                  |
   |                                   | -  One or multiple characters of **p**, **q**, **w**, and **x** are specified for **flags**.                                                                                                                                                                                                                                                                                                                     |
   |                                   | -  The **string** or **pattern** parameter contains multi-byte characters.                                                                                                                                                                                                                                                                                                                                       |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | b                                 | Use POSIX Basic Regular Expressions (BREs) for matching.                                                                                                                                                                                                                                                                                                                                                         |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | c                                 | Case-sensitive matching                                                                                                                                                                                                                                                                                                                                                                                          |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | e                                 | Use POSIX Extended Regular Expressions (EREs) for matching. If neither **b** nor **e** is specified and the Henry Spencer's regular expression library is used, Advanced Regular Expressions (AREs), similar to Perl Compatible Regular Expressions (PCREs), are used for matching; if neither **b** nor **e** is specified and the boost regex regular expression library is used, PCREs are used for matching. |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | i                                 | Case-insensitive matching                                                                                                                                                                                                                                                                                                                                                                                        |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | m                                 | Line feed-sensitive matching, which has the same meaning as option **n**                                                                                                                                                                                                                                                                                                                                         |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n                                 | Line feed-sensitive matching. When this option takes effect, the line separator affects the matching of metacharacters (., ^, $, and [^).                                                                                                                                                                                                                                                                        |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | p                                 | Partial line feed-sensitive matching. When this option takes effect, the line separator affects the matching of metacharacters (. and [^). "Partial" is in comparison with option **n**.                                                                                                                                                                                                                         |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | q                                 | Reset the regular expression to a text string enclosed in double quotation marks ("") and consisting of only common characters.                                                                                                                                                                                                                                                                                  |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | s                                 | Non-line feed-sensitive matching                                                                                                                                                                                                                                                                                                                                                                                 |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | t                                 | Compact syntax (default). When this option takes effect, all characters matter.                                                                                                                                                                                                                                                                                                                                  |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | w                                 | Reverse partial line feed-sensitive matching. When this option takes effect, the line separator affects the matching of metacharacters (^ and $). "Partial" is in comparison with option **n**.                                                                                                                                                                                                                  |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | x                                 | Extended syntax In contrast to the compact syntax, whitespace characters in regular expressions are ignored in the extended syntax. Whitespace characters include spaces, horizontal tabs, new lines, and any other characters in the space character table.                                                                                                                                                     |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return type: varchar

Examples:

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

concat_ws(sep text, str"any" [, str"any" [, ...] ])
---------------------------------------------------

Description: Uses the first parameter as the separator, which is associated with all following parameters.

Return type: text

Examples:

::

   SELECT concat_ws(',', 'ABCDE', 2, NULL, 22);
    concat_ws
   ------------
    ABCDE,2,22
   (1 row)

convert(string bytea, src_encoding name, dest_encoding name)
------------------------------------------------------------

Description: Converts the bytea string to **dest_encoding**. **src_encoding** specifies the source code encoding. The string must be valid in this encoding.

Return type: bytea

Examples:

::

   SELECT convert('text_in_utf8', 'UTF8', 'GBK');
             convert
   ----------------------------
    \x746578745f696e5f75746638
   (1 row)

.. note::

   If the rule for converting between source to target encoding (for example, GBK and LATIN1) does not exist, the string is returned without conversion. See the **pg_conversion** system catalog for details.

   Examples:

   ::

      show server_encoding;
       server_encoding
      -----------------
       LATIN1
      (1 row)

      SELECT convert_from('some text', 'GBK');
       convert_from
      --------------
       some text
      (1 row)

      db_latin1=# SELECT convert_to('some text', 'GBK');
            convert_to
      ----------------------
       \x736f6d652074657874
      (1 row)

      db_latin1=# SELECT convert('some text', 'GBK', 'LATIN1');
             convert
      ----------------------
       \x736f6d652074657874
      (1 row)

convert_from(string bytea, src_encoding name)
---------------------------------------------

Description: Converts the long bytea using the coding mode of the database.

**src_encoding** specifies the source code encoding. The string must be valid in this encoding.

Return type: text

Examples:

::

   SELECT convert_from('text_in_utf8', 'UTF8');
    convert_from
   --------------
    text_in_utf8
   (1 row)
   SELECT convert_from('\x6461746162617365','gbk');
    convert_from
   --------------
    database
   (1 row)

convert_to(string text, dest_encoding name)
-------------------------------------------

Description: Converts the given string into one whose encoding format is **dest_encoding**.

Return type: bytea

Examples:

::

   SELECT convert_to('some text', 'UTF8');
         convert_to
   ----------------------
    \x736f6d652074657874
   (1 row)
   SELECT convert_to('database', 'gbk');
        convert_to
   --------------------
    \x6461746162617365
   (1 row)

string [NOT] LIKE pattern [ESCAPE escape-character]
---------------------------------------------------

Description: Pattern matching function

If the pattern does not include a percentage sign (%) or an underscore (_), this mode represents itself only. In this case, the behavior of LIKE is the same as the equal operator. The underscore (_) in the pattern matches any single character while one percentage sign (%) matches no or multiple characters.

To match with underscores (_) or percent signs (%), corresponding characters in pattern must lead escape characters. The default escape character is a backward slash (\\) and can be specified using the **ESCAPE** clause. To match with escape characters, enter two escape characters.

Return type: boolean

Examples:

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

-  "i": case-insensitive
-  "c": case-sensitive
-  "n": allowing the metacharacter "." in a regular expression to be matched with a linefeed.
-  "m": allows **source_string** to be regarded as multiple rows.

If **match_parameter** is ignored, **case-sensitive** is enabled by default, "." is not matched with a linefeed, and **source_string** is regarded as a single row.

Return type: boolean

Examples:

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

format(formatstr text [, str"any" [, ...] ])
--------------------------------------------

Description: Formats a string.

Return type: text

Examples:

::

   SELECT format('Hello %s, %1$s', 'World');
          format
   --------------------
    Hello World, World
   (1 row)

md5(string)
-----------

Description: Encrypts a string in MD5 mode and returns a value in hexadecimal form.

.. note::

   MD5 is insecure and is not recommended.

Return type: text

Examples:

::

   SELECT md5('ABC');
                  md5
   ----------------------------------
    902fbdd2b1df0c4f70b4a5d23525e932
   (1 row)

decode(string text, format text)
--------------------------------

Description: Decodes binary data from textual representation.

Return type: bytea

Examples:

::

   SELECT decode('ZGF0YWJhc2U=', 'base64');
       decode
   --------------
    \x6461746162617365
   (1 row)

   SELECT convert_from('\x6461746162617365','utf-8');
    convert_from
   --------------
    database
   (1 row)

encode(data bytea, format text)
-------------------------------

Description: Encodes binary data into a textual representation.

Return type: text

Examples:

::

   SELECT encode('database', 'base64');
     encode
   ----------
    ZGF0YWJhc2U=
   (1 row)

CONV(n, fromBase, toBase)
-------------------------

Description: Converts the given value or string into value of a specific number system and outputs the result as a string. If the parameter contains NULL, NULL will be returned. The output value range is [-36, -2] & [2, 36].

Return type: text

Example:

::

   SELECT CONV(-1, 10, 16) as result;
         result
   ------------------
    FFFFFFFFFFFFFFFF
   (1 row)

HEX(n)
------

Description: Returns the hexadecimal string of **n**. **n** can be an integer or a string. If the parameter contains NULL, NULL will be returned.

Return type: text

Examples:

::

   SELECT HEX(255) as result;
    result
   --------
      FF
   (1 row)
   SELECT HEX('abc') as result;
    result
   --------
    616263
   (1 row)

UNHEX(n)
--------

Description: Performs the reverse operation of **HEX(n)**. **n** can be an int or a string. Each pair of hexadecimal digits in the parameter is regarded as a number and converted into the integer representation of the hexadecimal value. If the parameter contains NULL, NULL will be returned. This function is supported by version 8.2.0 or later clusters.

Return type: bytea

Examples:

::

   SELECT UNHEX('abc') as result;
    result
   --------
    \x0abc
   (1 row)

SPACE(n int)
------------

Description: Returns a string consisting of **n** spaces. If the parameter contains NULL, NULL will be returned.

Return type: text

Examples:

::

   SELECT SPACE(2) as result;
    result
   --------

   (1 row)

STRCMP(text, text)
------------------

Description: Compares two strings. If all strings are the same, **0** is returned. If the first string is smaller than the second string, **-1** is returned. In other cases, **1** is returned. If the parameter contains NULL, NULL will be returned.

Return type: text

Examples:

::

   SELECT STRCMP('AA', 'AA'), STRCMP('AA', 'AB'), STRCMP('AA', 'A');
   STRCMP  |  STRCMP  |  STRCMP
   ------------------------------
       0   |    -1    |     1
   (1 row)

BIN(n bigint)
-------------

Description: Converts the bigint type from decimal to binary and returns the result as a string. If the parameter contains NULL, NULL will be returned.

Return type: text

Examples:

::

   SELECT BIN(16) as result;
    result
   --------
    10000
   (1 row)
