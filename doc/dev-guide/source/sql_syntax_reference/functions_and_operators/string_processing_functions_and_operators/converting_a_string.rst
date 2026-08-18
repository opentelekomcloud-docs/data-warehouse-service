:original_name: dws_06_0417.html

.. _dws_06_0417:

Converting a String
===================

upper(string)
-------------

Description: Converts the string into the uppercase.

Return type: varchar

Example:

::

   SELECT upper('database');
     upper
   ----------
    DATABASE
   (1 row)

ucase(string)
-------------

Description: Converts the string into the uppercase.

Return type: varchar

Example:

.. code-block::

   SELECT ucase('database');
     ucase
   ----------
    DATABASE
   (1 row)

lower(string)
-------------

Description: Converts the string into the lowercase.

Return type: varchar

Example:

::

   SELECT lower('DATABASE');
     lower
   ----------
    database
   (1 row)

lcase(string)
-------------

Description: Converts the string into the lowercase.

Return type: varchar

Example:

.. code-block::

   SELECT lcase('DATABASE');
     lcase
   ----------
    database
   (1 row)

initcap(string)
---------------

Description: Converts the first letter of each word in the string into the uppercase and the other letters into the lowercase.

Return type: text

Example:

::

   SELECT initcap('hi THOMAS');
     initcap
   -----------
    Hi Thomas
   (1 row)

quote_ident(string text)
------------------------

Description: Returns the given string suitably quoted to be used as an identifier in an SQL statement string (quotation marks are used as required). Quotation marks are added only when necessary (the string contains non-identifier characters or characters that can be converted to uppercase or lowercase). Embedded quotation marks are doubled.

Return type: text

Example:

::

   SELECT quote_ident('hello world');
    quote_ident
   --------------
    "hello world"
   (1 row)

quote_literal(string text)
--------------------------

Description: Returns the given string suitably quoted to be used as a text in an SQL statement string (quotation marks are used as required).

Return type: text

Example:

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

If the parameter is NULL, NULL is returned. If the parameter may be NULL, you are advised to use **quote_nullable**.

::

   SELECT quote_literal(NULL);
    quote_literal
   ---------------

   (1 row)

quote_literal(value anyelement)
-------------------------------

Description: Coerces the given value to text and then quotes it as a literal.

Return type: text

Example:

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

Example:

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

If the parameter is **NULL**, **NULL** is returned.

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

Example:

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

If the parameter is NULL, NULL is returned.

::

   SELECT quote_nullable(NULL);
    quote_nullable
   ----------------
    NULL
   (1 row)

BIN(n bigint)
-------------

Description: Converts the bigint type from decimal to binary and returns the result as a string. If the parameter contains NULL, NULL will be returned.

Return type: text

Example:

::

   SELECT BIN(16) as result;
    result
   --------
    10000
   (1 row)

HEX(n)
------

Description: Returns the hexadecimal string of **n**. **n** can be an integer or string. If the parameter contains NULL, NULL will be returned.

Return type: text

Example:

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

Description: Performs the reverse operation of **HEX(n)**. *n* can be of the int type or a string. Each pair of hexadecimal digits in a parameter is considered as a number and converted into the character represented by the number. If the parameter contains NULL, NULL will be returned. This parameter is supported only by clusters of version 8.2.0 or later.

Return type: bytea

Example:

::

   SELECT UNHEX('abc') as result;
    result
   --------
    \x0abc
   (1 row)

reverse(str)
------------

Description: Returns a string in reverse order.

Return type: text

Example:

::

   SELECT reverse('abcde');
    reverse
   ---------
    edcba
   (1 row)

to_hex(number int or bigint)
----------------------------

Description: Converts a number to a hexadecimal number.

Return type: text

Example:

::

   SELECT to_hex(2147483647);
     to_hex
   ----------
    7fffffff
   (1 row)

translate(string text, from text, to text)
------------------------------------------

Description: Replaces any character in the original string that matches a character in the **from** set by the corresponding character in the **to** set. If **from** is longer than **to**, extra characters in **from** are removed.

Parameters:

-  **string**: original string to be processed.
-  **from**: specifies the characters to be replaced.
-  **to**: specifies the characters after replacement.

Return type: text

Example:

Set the **from** parameter to **143** and the **to** parameter to **ax**. Replace **1** and **4** in the string **12345** with **a** and **x** respectively. Delete **3** because there is no corresponding replacement character. **a2x5** is returned.

::

   SELECT translate('12345', '143', 'ax');
    translate
   -----------
    a2x5
   (1 row)

ascii(string)
-------------

Description: Returns the ASCII code of the first character in a string.

Return type: integer

Example:

Return the ASCII code of the first character **x** in the string **xyz**.

::

   SELECT ascii('xyz');
    ascii
   -------
      120
   (1 row)

convert(string bytea, src_encoding name, dest_encoding name)
------------------------------------------------------------

Description: Converts the string **bytea** to **dest_encoding**. **src_encoding** specifies the source code encoding. **string** must be valid in this encoding.

Return type: bytea

Example:

::

   SELECT convert('text_in_utf8', 'UTF8', 'GBK');
             convert
   ----------------------------
    \x746578745f696e5f75746638
   (1 row)

.. note::

   If the rule for converting the source encoding to the target encoding (for example, GBK and LATIN1) does not exist, the string is returned without conversion. See the **pg_conversion** system catalog for details.

   Example:

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

Description: Converts the string **bytea** using the coding mode of the database.

**src_encoding** specifies the source code encoding. **string** must be valid in this encoding.

Return type: text

Example:

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

Example:

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

chr(integer)
------------

Description: Converts an integer to the corresponding ASCII character.

Return type: varchar

Example:

::

   SELECT chr(65);
    chr
   -----
    A
   (1 row)

format(formatstr text [, str"any" [, ...] ])
--------------------------------------------

Description: Formats a string.

Return type: text

Example:

::

   SELECT format('Hello %s, %1$s', 'World');
          format
   --------------------
    Hello World, World
   (1 row)

md5(string)
-----------

Description: Encrypts a string using MD5 and returns a hexadecimal number.

.. note::

   MD5 is not recommended for security purposes.

Return type: text

Example:

::

   SELECT md5('data');
                  md5
   ----------------------------------
    8d777f385d3dfec8815d20f7496026dc
   (1 row)

decode(string text, format text)
--------------------------------

Description: Decodes binary data from textual representation.

Return type: bytea

Example:

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

Example:

::

   SELECT encode('database', 'base64');
     encode
   ----------
    ZGF0YWJhc2U=
   (1 row)

CONV(n, fromBase, toBase)
-------------------------

Description: Converts the given value or string into value of a specific number system and outputs the result as a string. If the parameter contains NULL, NULL will be returned. The value range is [-36, -2] and [2, 36].

Return type: text

Example:

::

   SELECT CONV(-1, 10, 16) as result;
         result
   ------------------
    FFFFFFFFFFFFFFFF
   (1 row)
