:original_name: dws_06_0410.html

.. _dws_06_0410:

Overview of String Processing Functions and Operators
=====================================================

The string processing functions and operators provided by DWS are used to obtain the string length as well as truncate, concatenate, convert, and compare data.

Before learning about the functions, you can learn about the concepts of bit, byte, and character.

-  A bit is the basic unit of data in computers, representing either 0 or 1 in binary. These values correspond to the on/off state of a circuit or the positive/negative polarity of a magnetized surface.
-  1 byte = 8 bits. A byte the basic unit of data storage and transmission in computers.
-  A character is a human-readable text symbol, such as a letter, digit, and punctuation. The database character encoding determines how a character is converted into bytes. For details about the database character encoding, see :ref:`CREATE DATABASE <dws_06_0156>`.

For example, in UTF-8 of DWS, an English character takes one byte, and a Chinese character takes three bytes. If a field is defined as VARCHAR(100), the maximum storage would be 100 characters x 3 bytes/character = 300 bytes. In GBK, a single Chinese character can take two bytes.

Application scenarios of string processing functions are as follows:

-  :ref:`Obtaining the Length of a String <dws_06_0411>`: Query the number of digits and byte length of a string. For example, query the number of digits of the string **World**.
-  :ref:`Truncating a String <dws_06_0412>`: Extract a substring from a long string. For example, extract **Hello** from **HelloWorld**.
-  :ref:`Concatenating Strings <dws_06_0413>`: Concatenate two strings, for example, concatenate **DATA** and **BASE** to obtain **DATABASE**.
-  :ref:`Replacing and Padding Strings <dws_06_0414>`: Replace a character or substring in a string with another character or substring, or delete redundant spaces. For example, delete the space of "**Database** ".
-  :ref:`Deleting a Specified Character <dws_06_0415>`: Delete a specified character from a string, for example, delete **xx** from **DataxBasexx**.
-  :ref:`Splitting a String <dws_06_0416>`: Split a string into several parts as needed and separate the parts with delimiters. For example, split **hello world** into **hello, world**.
-  :ref:`Converting a String <dws_06_0417>`: Convert a string according to certain rules, for example, convert the case of letters.
-  :ref:`Comparing Strings <dws_06_0418>`: Compare two strings.
-  :ref:`Obtaining the Position of a Character or Substring <dws_06_0419>`: For example, obtain the second matching position of the substring **bcd** in the main string **abcdabcdabcd** starting from the second character.
-  :ref:`Checking If a String Matches a Specific Pattern Using String Matching Functions <dws_06_0420>`: For example, check whether **a** is in the regular expression of **[ac]**.
