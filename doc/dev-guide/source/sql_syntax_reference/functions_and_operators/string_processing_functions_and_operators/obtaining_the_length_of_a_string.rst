:original_name: dws_06_0411.html

.. _dws_06_0411:

Obtaining the Length of a String
================================

In DWS, you can use the following functions to obtain the length of a string, including the number of bits, bytes, and characters. For details about the differences among bits, bytes, and characters, see :ref:`Overview of String Processing Functions and Operators <dws_06_0410>`.

.. table:: **Table 1** Common functions

   +-------------+--------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Type        | Description                                                                          | Function                                                                                                    | Example                            | Usage Difference                                                                                                                                                                                                                                                                                      |
   +=============+======================================================================================+=============================================================================================================+====================================+=======================================================================================================================================================================================================================================================================================================+
   | Bits        | Obtain the number of digits in a string.                                             | :ref:`bit_length(string) <en-us_topic_0000002483630660__section8601617163518>`                              | ::                                 | ``-``                                                                                                                                                                                                                                                                                                 |
   |             |                                                                                      |                                                                                                             |                                    |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    SELECT bit_length('world');     |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |     bit_length                     |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    ------------                    |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |             40                     |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    (1 row)                         |                                                                                                                                                                                                                                                                                                       |
   +-------------+--------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Bytes       | Obtain the number of bytes in all string types.                                      | :ref:`octet_length(string) <en-us_topic_0000002483630660__section14230173743119>`                           | ::                                 | The functions of **octet_length(string)** and **lengthb(string)** are the same. When developing cross-platform databases such as MySQL and DWS, **octet_length(string)** is preferred. **lengthb(string)** is a mapping function for compatibility with Oracle and is used only for Oracle migration. |
   |             |                                                                                      |                                                                                                             |                                    |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    SELECT octet_length('world');   | **lengthb(text/bpchar)** is the same as **lengthb(string)**, but its input parameter is of the text or bpchar type.                                                                                                                                                                                   |
   |             |                                                                                      |                                                                                                             |     octet_length                   |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    ------------                    |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |             5                      |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    (1 row)                         |                                                                                                                                                                                                                                                                                                       |
   +-------------+--------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |             | Obtain the number of bytes in all string types (only for compatibility with Oracle). | :ref:`lengthb(string) <en-us_topic_0000002483630660__section1567184515163>`                                 | ::                                 |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |                                    |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    SELECT lengthb('abc'::text);    |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    lengthb                         |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    ------------                    |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |            3                       |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    (1 row)                         |                                                                                                                                                                                                                                                                                                       |
   +-------------+--------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |             | Obtain the number of bytes in text and bpchar (only for compatibility with Oracle).  | :ref:`lengthb(text/bpchar) <en-us_topic_0000002483630660__section157296615456>`                             | ::                                 |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |                                    |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    SELECT lengthb('abc'::CHAR(5)); |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    lengthb                         |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    ------------                    |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |            5                       |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    (1 row)                         |                                                                                                                                                                                                                                                                                                       |
   +-------------+--------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Characters  | Obtain the number of characters in a string.                                         | :ref:`char_length(string) or character_length(string) <en-us_topic_0000002483630660__section1124561251118>` | ::                                 | **length(string**) is equivalent to **char_length(string)**, but its format is simpler.                                                                                                                                                                                                               |
   |             |                                                                                      |                                                                                                             |                                    |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      | :ref:`length(string) <en-us_topic_0000002483630660__section4570183884812>`                                  |    SELECT length('database');      |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |     length                         |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    --------                        |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |          8                         |                                                                                                                                                                                                                                                                                                       |
   |             |                                                                                      |                                                                                                             |    (1 row)                         |                                                                                                                                                                                                                                                                                                       |
   +-------------+--------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |             | Obtain the number of characters in a string in the specified encoding format.        | :ref:`length(string bytea, encoding name) <en-us_topic_0000002483630660__section54401568146>`               | ``-``                              | ``-``                                                                                                                                                                                                                                                                                                 |
   +-------------+--------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------+------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000002483630660__section8601617163518:

bit_length(string)
------------------

Description: Obtains the number of bits in a string. That is, the number of bytes multiplied by 8. The number of bits in a string depends on the database encoding mode. For example, in GBK, a Chinese character is 2 bytes. In UTF-8 and SQL_ASCII, a Chinese character is 3 bytes.

Return type: integer

Example:

::

   SELECT bit_length('world');
    bit_length
   ------------
            40
   (1 row)

.. _en-us_topic_0000002483630660__section14230173743119:

octet_length(string)
--------------------

Description: Obtains the number of bytes in a string. It is important for processing multi-byte characters (such as UTF-8 characters and special symbols) and can accurately obtain the storage size of a string.

Return type: integer

Example:

::

   SELECT octet_length('data');
    octet_length
   --------------
               4
   (1 row)

.. _en-us_topic_0000002483630660__section1567184515163:

lengthb(string)
---------------

Description: Obtains the length of a string in bytes. It shares the same functions as :ref:`octet_length(string) <en-us_topic_0000002483630660__section14230173743119>` and is only used for compatibility with Oracle. If data is migrated from Oracle, use **lengthb(string)**. However, if workloads need to run across databases (for example, DWS and MySQL), use **octet_length(string)** because MySQL does not have the **lengthb** function.

The return result depends on character sets (GBK and UTF-8).

Return type: integer

Example:

::

   SELECT lengthb('hello');
    lengthb
   ---------
          5
   (1 row)

.. _en-us_topic_0000002483630660__section157296615456:

lengthb(text/bpchar)
--------------------

Description: Obtains the number of bytes of a specified string. The input parameter can be text or bpchar (equivalent to **CHAR(n)**).

-  **lengthb(text)** indicates the byte length of actual characters (without padding spaces), which is suitable for most scenarios.
-  **lengthb(bpchar)** indicates the byte length of a fixed-length string (including padding spaces), which is suitable for strict fixed-length scenarios (such as fixed-length codes and certificate numbers). It is not recommended for daily use (implicit problems may be caused by space padding).

Return type: integer

.. note::

   -  For a string containing newline characters, for example, a string consisting of a newline character and a space, the value of **length** and **lengthb** in DWS is 2.
   -  This function returns the number of bytes in a specified string. For multi-byte character encoding, such as UTF-8, one character may occupy multiple bytes.

Example:

::

   SELECT lengthb('hello');
    lengthb
   ---------
          5
   (1 row)

Examples of input parameters of the text and bpchar types:

::

   -- Case 1: Specify the input parameters as text.
   SELECT lengthb('abc'::text); -- 'abc' occupies 3 bytes (UTF-8), and the returned value is 3.
   -- Case 2: Specify the input parameters as bpchar or CHAR(5).
   SELECT lengthb('abc'::CHAR(5)); -- The value is stored as 'abc..', and the returned value is 5 (each space occupies 1 byte).

|image1|

.. _en-us_topic_0000002483630660__section1124561251118:

char_length(string) or character_length(string)
-----------------------------------------------

Description: Obtains the number of characters in a string.

Return type: integer

.. note::

   For multi-byte character encoding (such as UTF-8), one character may occupy multiple bytes.

Example:

::

   SELECT char_length('hello');
    char_length
   -------------
              5
   (1 row)

.. _en-us_topic_0000002483630660__section4570183884812:

length(string)
--------------

Description: Obtains the character length (number of characters) of a string. It is equivalent to :ref:`char_length(string) <en-us_topic_0000002483630660__section1124561251118>` and is simpler.

Return type: integer

Example:

::

   SELECT length('database');
    length
   --------
         8
   (1 row)

.. _en-us_topic_0000002483630660__section54401568146:

length(string bytea, encoding name)
-----------------------------------

Description: Obtains the number of characters in a string in the specified encoding format. In the specified encoding format, **string** must be valid.

Return type: integer

Example:

::

   SELECT length('database', 'UTF8');
    length
   --------
         8
   (1 row)

.. |image1| image:: /_static/images/en-us_image_0000002483953324.png
