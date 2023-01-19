:original_name: dws_06_0036.html

.. _dws_06_0036:

Type Conversion Functions
=========================


Type Conversion Functions
-------------------------

-  cast(x as y)

   Description: Converts x into the type specified by y.

   For example:

   ::

      SELECT cast('22-oct-1997' as timestamp);
            timestamp
      ---------------------
       1997-10-22 00:00:00
      (1 row)

-  hextoraw(string)

   Description: Converts a string in hexadecimal format into binary format.

   Return type: raw

   For example:

   ::

      SELECT hextoraw('7D');
       hextoraw
      ----------
       7D
      (1 row)

-  numtoday(numeric)

   Description: Converts values of the number type into the timestamp of the specified type.

   Return type: timestamp

   For example:

   ::

      SELECT numtoday(2);
       numtoday
      ----------
       2 days
      (1 row)

-  pg_systimestamp()

   Description: Obtains the system timestamp.

   Return type: timestamp with time zone

   For example:

   ::

      SELECT pg_systimestamp();
              pg_systimestamp
      -------------------------------
       2015-10-14 11:21:28.317367+08
      (1 row)

-  rawtohex(string)

   Description: Converts a string in binary format into hexadecimal format.

   The result is the ACSII code of the input characters in hexadecimal format.

   Return type: varchar

   For example:

   ::

      SELECT rawtohex('1234567');
          rawtohex
      ----------------
       31323334353637
      (1 row)

-  to_char (datetime/interval [, fmt])

   Description: Converts a DATETIME or INTERVAL value of the DATE/TIMESTAMP/TIMESTAMP WITH TIME ZONE/TIMESTAMP WITH LOCAL TIME ZONE type into the VARCHAR type according to the format specified by **fmt**.

   -  The optional parameter **fmt** includes the following types: date, time, week, quarter, and century. Each type has a unique template. The templates can be combined together. Common templates include: HH, MM, SS, YYYY, MM, and DD. For details, see :ref:`Table 2 <en-us_topic_0000001098831022__tecb001c170ee45b38a3522119b2c5aae>`.
   -  A template may have a modification word. FM is a common modification word and is used to suppress the preceding zero or the following blank spaces.

   Return type: varchar

   For example:

   ::

      SELECT to_char(current_timestamp,'HH12:MI:SS');
       to_char
      ----------
       10:19:26
      (1 row)

   ::

      SELECT to_char(current_timestamp,'FMHH12:FMMI:FMSS');
       to_char
      ----------
       10:19:46
      (1 row)

-  to_char(double precision, text)

   Description: Converts the values of the double-precision type into the strings in the specified format.

   Return type: text

   For example:

   ::

      SELECT to_char(125.8::real, '999D99');
       to_char
      ---------
        125.80
      (1 row)

-  to_char (integer/number[, fmt])

   Descriptions: Converts an integer or a value in floating point format into a string in specified format.

   -  The optional parameter **fmt** includes the following types: decimal characters, grouping characters, positive/negative sign and currency sign. Each type has a unique template. The templates can be combined together. Common templates include: 9, 0, millesimal sign (,), and decimal point (.). For details, see :ref:`Table 1 <en-us_topic_0000001098831022__t351061e37e45427ead6ddec4cd1ad376>`.
   -  A template can have a modification word, similar to FM. However, FM does not suppress 0 which is output according to the template.
   -  Use the template X or x to convert an integer value into a string in hexadecimal format.

   Return type: varchar

   For example:

   ::

      SELECT to_char(1485,'9,999');
       to_char
      ---------
        1,485
      (1 row)

   ::

      SELECT to_char( 1148.5,'9,999.999');
        to_char
      ------------
        1,148.500
      (1 row)

   ::

      SELECT to_char(148.5,'990999.909');
         to_char
      -------------
          0148.500
      (1 row)

   ::

      SELECT to_char(123,'XXX');
       to_char
      ---------
         7B
      (1 row)

-  to_char(interval, text)

   Description: Converts the values of the time interval type into the strings in the specified format.

   Return type: text

   For example:

   ::

      SELECT to_char(interval '15h 2m 12s', 'HH24:MI:SS');
       to_char
      ----------
       15:02:12
      (1 row)

-  to_char(int, text)

   Description: Converts the values of the integer type into the strings in the specified format.

   Return type: text

   For example:

   ::

      SELECT to_char(125, '999');
       to_char
      ---------
        125
      (1 row)

-  to_char(numeric, text)

   Description: Converts the values of the numeric type into the strings in the specified format.

   Return type: text

   For example:

   ::

      SELECT to_char(-125.8, '999D99S');
       to_char
      ---------
       125.80-
      (1 row)

-  to_char (string)

   Description: Converts the CHAR/VARCHAR/VARCHAR2/CLOB type into the VARCHAR type.

   If this function is used to convert data of the CLOB type, and the value to be converted exceeds the value range of the target type, an error is returned.

   Return type: varchar

   For example:

   ::

      SELECT to_char('01110');
       to_char
      ---------
       01110
      (1 row)

-  to_char(timestamp, text)

   Description: Converts the values of the timestamp type into the strings in the specified format.

   Return type: text

   For example:

   ::

      SELECT to_char(current_timestamp, 'HH12:MI:SS');
       to_char
      ----------
       10:55:59
      (1 row)

-  to_clob(char/nchar/varchar/nvarchar/varchar2/nvarchar2/text/raw)

   Description: Convert the RAW type or text character set type CHAR/NCHAR/VARCHAR/VARCHAR2/NVARCHAR2/TEXT into the CLOB type.

   Return type: clob

   For example:

   ::

      SELECT to_clob('ABCDEF'::RAW(10));
       to_clob
      ---------
       ABCDEF
      (1 row)

   ::

      SELECT to_clob('hello111'::CHAR(15));
       to_clob
      ----------
       hello111
      (1 row)

   ::

      SELECT to_clob('gauss123'::NCHAR(10));
       to_clob
      ----------
       gauss123
      (1 row)

   ::

      SELECT to_clob('gauss234'::VARCHAR(10));
       to_clob
      ----------
       gauss234
      (1 row)

   ::

      SELECT to_clob('gauss345'::VARCHAR2(10));
       to_clob
      ----------
       gauss345
      (1 row)

   ::

      SELECT to_clob('gauss456'::NVARCHAR2(10));
       to_clob
      ----------
       gauss456
      (1 row)

   ::

      SELECT to_clob('World222!'::TEXT);
        to_clob
      -----------
       World222!
      (1 row)

-  to_date(text)

   Description: Converts values of the text type into the timestamp in the specified format.

   Return type: timestamp

   For example:

   ::

      SELECT to_date('2015-08-14');
             to_date
      ---------------------
       2015-08-14 00:00:00
      (1 row)

-  to_date(text, text)

   Description: Converts the values of the string type into the dates in the specified format.

   Return type: timestamp

   For example:

   ::

      SELECT to_date('05 Dec 2000', 'DD Mon YYYY');
             to_date
      ---------------------
       2000-12-05 00:00:00
      (1 row)

-  to_date(string, fmt)

   Description: Converts a string into a value of the DATE type according to the format specified by **fmt**. For details about the fmt format, see :ref:`Table 2 <en-us_topic_0000001098831022__tecb001c170ee45b38a3522119b2c5aae>`.

   This function cannot support the CLOB type directly. However, a parameter of the CLOB type can be converted using implicit conversion.

   Return type: date

   For example:

   ::

      SELECT TO_DATE('05 Dec 2010','DD Mon YYYY');
             to_date
      ---------------------
       2010-12-05 00:00:00
      (1 row)

-  to_number ( expr [, fmt])

   Description: Converts **expr** into a value of the NUMBER type according to the specified format.

   For details about the type conversion formats, see :ref:`Table 1 <en-us_topic_0000001098831022__t351061e37e45427ead6ddec4cd1ad376>`.

   If a hexadecimal string is converted into a decimal number, the hexadecimal string can include a maximum of 16 bytes if it is to be converted into a sign-free number.

   During the conversion from a hexadecimal string to a decimal digit, the format string cannot have a character other than x or X. Otherwise, an error is reported.

   Return type: number

   For example:

   ::

      SELECT to_number('12,454.8-', '99G999D9S');
       to_number
      -----------
        -12454.8
      (1 row)

-  to_number(text, text)

   Description: Converts the values of the string type into the numbers in the specified format.

   Return type: numeric

   For example:

   ::

      SELECT to_number('12,454.8-', '99G999D9S');
       to_number
      -----------
        -12454.8
      (1 row)

-  to_timestamp(double precision)

   Description: Converts a UNIX century into a timestamp.

   Return type: timestamp with time zone

   For example:

   ::

      SELECT to_timestamp(1284352323);
            to_timestamp
      ------------------------
       2010-09-13 12:32:03+08
      (1 row)

-  to_timestamp(string [,fmt])

   Description: Converts a string into a value of the timestamp type according to the format specified by **fmt**. When **fmt** is not specified, perform the conversion according to the format specified by **nls_timestamp_format**. For details about the fmt format, see :ref:`Table 2 <en-us_topic_0000001098831022__tecb001c170ee45b38a3522119b2c5aae>`.

   In **to_timestamp** in GaussDB(DWS):

   -  If the input year *YYYY* is 0, an error will be reported.
   -  If the input year YYYY<0 to specify SYYYY in fmt, the year with the value of n (an absolute value) BC is output correctly.

   Characters in the fmt must match the schema for formatting the data and time. Otherwise, an error is reported.

   Return type: timestamp without time zone

   For example:

   ::

      SHOW nls_timestamp_format;
          nls_timestamp_format
      ----------------------------
       DD-Mon-YYYY HH:MI:SS.FF AM
      (1 row)

      SELECT to_timestamp('12-sep-2014');
          to_timestamp
      ---------------------
       2014-09-12 00:00:00
      (1 row)

   ::

      SELECT to_timestamp('12-Sep-10 14:10:10.123000','DD-Mon-YY HH24:MI:SS.FF');
            to_timestamp
      -------------------------
       2010-09-12 14:10:10.123
      (1 row)

   ::

      SELECT to_timestamp('-1','SYYYY');
            to_timestamp
      ------------------------
       0001-01-01 00:00:00 BC
      (1 row)

   ::

      SELECT to_timestamp('98','RR');
          to_timestamp
      ---------------------
       1998-01-01 00:00:00
      (1 row)

   ::

      SELECT to_timestamp('01','RR');
          to_timestamp
      ---------------------
       2001-01-01 00:00:00
      (1 row)

-  to_timestamp(text, text)

   Description: Converts values of the string type into the timestamp of the specified type.

   Return type: timestamp

   For example:

   ::

      SELECT to_timestamp('05 Dec 2000', 'DD Mon YYYY');
          to_timestamp
      ---------------------
       2000-12-05 00:00:00
      (1 row)

The following table describes the value formats of the **to_number** function.

.. _en-us_topic_0000001098831022__t351061e37e45427ead6ddec4cd1ad376:

.. table:: **Table 1** Template patterns for numeric formatting

   +------------+-----------------------------------------------------------------------+
   | Schema     | Description                                                           |
   +============+=======================================================================+
   | 9          | Value with specified digits                                           |
   +------------+-----------------------------------------------------------------------+
   | 0          | Values with leading zeros                                             |
   +------------+-----------------------------------------------------------------------+
   | Period (.) | Decimal point                                                         |
   +------------+-----------------------------------------------------------------------+
   | Comma (,)  | Group (thousand) separator                                            |
   +------------+-----------------------------------------------------------------------+
   | PR         | Negative values in angle brackets                                     |
   +------------+-----------------------------------------------------------------------+
   | S          | Sign anchored to number (uses locale)                                 |
   +------------+-----------------------------------------------------------------------+
   | L          | Currency symbol (uses locale)                                         |
   +------------+-----------------------------------------------------------------------+
   | D          | Decimal point (uses locale)                                           |
   +------------+-----------------------------------------------------------------------+
   | G          | Group separator (uses locale)                                         |
   +------------+-----------------------------------------------------------------------+
   | MI         | Minus sign in the specified position (if the number is less than 0)   |
   +------------+-----------------------------------------------------------------------+
   | PL         | Plus sign in the specified position (if the number is greater than 0) |
   +------------+-----------------------------------------------------------------------+
   | SG         | Plus or minus sign in the specified position                          |
   +------------+-----------------------------------------------------------------------+
   | RN         | Roman numerals (the input values are between 1 and 3999)              |
   +------------+-----------------------------------------------------------------------+
   | TH or th   | Ordinal number suffix                                                 |
   +------------+-----------------------------------------------------------------------+
   | V          | Shifts specified number of digits (decimal)                           |
   +------------+-----------------------------------------------------------------------+

The following table describes the patterns of date and time values. They can be used for the **to_date**, **to_timestamp**, and **to_char** functions, and the **nls_timestamp_format** parameter.

.. _en-us_topic_0000001098831022__tecb001c170ee45b38a3522119b2c5aae:

.. table:: **Table 2** Schemas for formatting date and time

   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Type                  | Schema                | Description                                                                                                                                                                                 |
   +=======================+=======================+=============================================================================================================================================================================================+
   | Hour                  | HH                    | Number of hours in one day (01-12)                                                                                                                                                          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | HH12                  | Number of hours in one day (01-12)                                                                                                                                                          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | HH24                  | Number of hours in one day (00-23)                                                                                                                                                          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Minute                | MI                    | Minute (00-59)                                                                                                                                                                              |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Second                | SS                    | Second (00-59)                                                                                                                                                                              |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | FF                    | Microsecond (000000-999999)                                                                                                                                                                 |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | SSSSS                 | Second after midnight (0-86399)                                                                                                                                                             |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Morning and afternoon | AM or A.M.            | Morning identifier                                                                                                                                                                          |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | PM or P.M.            | Afternoon identifier                                                                                                                                                                        |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Year                  | Y,YYY                 | Year with comma (with four digits or more)                                                                                                                                                  |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | SYYYY                 | Year with four digits BC                                                                                                                                                                    |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | YYYY                  | Year (with four digits or more)                                                                                                                                                             |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | YYY                   | Last three digits of a year                                                                                                                                                                 |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | YY                    | Last two digits of a year                                                                                                                                                                   |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | Y                     | Last one digit of a year                                                                                                                                                                    |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | IYYY                  | ISO year (with four digits or more)                                                                                                                                                         |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | IYY                   | Last three digits of an ISO year                                                                                                                                                            |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | IY                    | Last two digits of an ISO year                                                                                                                                                              |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | I                     | Last one digit of an ISO year                                                                                                                                                               |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | RR                    | Last two digits of a year (A year of the 20th century can be stored in the 21st century.)                                                                                                   |
   |                       |                       |                                                                                                                                                                                             |
   |                       |                       | The password must comply with the following rules:                                                                                                                                          |
   |                       |                       |                                                                                                                                                                                             |
   |                       |                       | -  If the range of the input two-digit year is between 00 and 49:                                                                                                                           |
   |                       |                       |                                                                                                                                                                                             |
   |                       |                       |    If the last two digits of the current year are between 00 and 49, the first two digits of the returned year are the same as the first two digits of the current year.                    |
   |                       |                       |                                                                                                                                                                                             |
   |                       |                       |    If the last two digits of the current year are between 50 and 99, the first two digits of the returned year equal to the first two digits of the current year plus 1.                    |
   |                       |                       |                                                                                                                                                                                             |
   |                       |                       | -  If the range of the input two-digit year is between 50 and 99:                                                                                                                           |
   |                       |                       |                                                                                                                                                                                             |
   |                       |                       |    If the last two digits of the current year are between 00 and 49, the first two digits of the returned year equal to the first two digits of the current year minus 1.                   |
   |                       |                       |                                                                                                                                                                                             |
   |                       |                       |    If the last two digits of the current year are between 50 and 99, the first two digits of the returned year are the same as the first two digits of the current year.                    |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | RRRR                  | Capable of receiving a year with four digits or two digits. If there are 2 digits, the value is the same as the returned value of RR. If there are 4 digits, the value is the same as YYYY. |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | -  BC or B.C.         | Era indicator Before Christ (BC) and After Christ (AD)                                                                                                                                      |
   |                       | -  AD or A.D.         |                                                                                                                                                                                             |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Month                 | MONTH                 | Full spelling of a month in uppercase (9 characters are filled in if the value is empty.)                                                                                                   |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | MON                   | Month in abbreviated format in uppercase (with three characters)                                                                                                                            |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | MM                    | Month (01-12)                                                                                                                                                                               |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | RM                    | Month in Roman numerals (I-XII; I=JAN) and uppercase                                                                                                                                        |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Day                   | DAY                   | Full spelling of a date in uppercase (9 characters are filled in if the value is empty.)                                                                                                    |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | DY                    | Day in abbreviated format in uppercase (with three characters)                                                                                                                              |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | DDD                   | Day in a year (001-366)                                                                                                                                                                     |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | DD                    | Day in a month (01-31)                                                                                                                                                                      |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | D                     | Day in a week (1-7).                                                                                                                                                                        |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Week                  | W                     | Week in a month (1-5) (The first week starts from the first day of the month.)                                                                                                              |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | WW                    | Week in a year (1-53) (The first week starts from the first day of the year.)                                                                                                               |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   |                       | IW                    | Week in an ISO year (The first Thursday is in the first week.)                                                                                                                              |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Century               | CC                    | Century (with two digits) (The 21st century starts from 2001-01-01.)                                                                                                                        |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Julian date           | J                     | Julian date (starting from January 1 of 4712 BC)                                                                                                                                            |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Quarter               | Q                     | Quarter                                                                                                                                                                                     |
   +-----------------------+-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
