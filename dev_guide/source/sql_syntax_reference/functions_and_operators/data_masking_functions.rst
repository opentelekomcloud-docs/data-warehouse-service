:original_name: dws_06_0064.html

.. _dws_06_0064:

Data Masking Functions
======================

Data masking functions are used to mask and protect sensitive data. Generally, you are advised to bind these functions to the columns to be redacted based on the data masking syntax, rather than use them directly on query statements.

mask_none(column_name)
----------------------

Description: Masks no data (for internal tests only).

Return type: same as **column_name**

mask_full(column_name)
----------------------

Description: Replaces all data with a fixed value. The fixed value varies depending on the data type of the redacted column.

Return type: same as **column_name**

mask_partial(column_name, mask_digital, mask_from[, mask_to])
-------------------------------------------------------------

Description: Replaces the digits from the **mask_from** to **mask_to** position in a number with the digit specified by **mask_digital**. The default value of **mask_to** can be used, which indicates that the digits from the **mask_from** position to the end of the number are replaced. **mask_digital** can only be a digit from 0 to 9.

Return type: same as **column_name**

mask_partial(column_name [, input_format, output_format], mask_char, mask_from[, mask_to])
------------------------------------------------------------------------------------------

Description: Replaces the digits from the **mask_from** to **mask_to** position in a string with the character specified by **mask_char** based on the given input and output formats.

Parameter description:

-  input_format

   The input format is a character string of V and F, whose length is the same as that of the data in the redacted column. Characters in positions corresponding to V may be masked, and characters in positions corresponding to F are skipped. The V character string specifies which characters are to be masked. The input and output formats apply to data with a fixed length, such as bank card numbers, ID card numbers, and phone numbers.

-  output_format

   The output format is a character string of V and any other character, whose length is the same as that of the data in the redacted column. V characters correspond to those in the **input_format**, and other characters correspond to the F characters in the **input_format**.

   For parameters **input_format** and **output_format**, you can use their default values or set them to **""**. In this case, there is no requirement for the input or output format, and the whole string will be masked.

-  mask_char

   Masking character, which can be any one character, for example, an asterisk (``*``) or a number sign (#).

-  mask_from

   First character in the string that will be masked. The value must be greater than **0**.

-  mask_to

   Last character in the string that will be masked. The default value can be used, which indicates that the character from the **mask_from** position to the last character of the string will be masked.

Return type: same as **column_name**

mask_partial(column_name, mask_field1, mask_value1, mask_field2, mask_value2, mask_field3, mask_value3)
-------------------------------------------------------------------------------------------------------

Description: Masks a date or time based on three specified fields. If **mask_value** is **-1**, the corresponding **mask_field** is not masked. **mask_field** can be **month**, **day**, **year**, **hour**, **minute**, or **second**. The value range of each field must be within that of the actual time unit.

Return type: same as **column_name**

.. note::

   Masking functions are recommended if you want to create masking policies.

   For details about how to use data masking functions, see the examples in in "Database Security Management > Sensitive Data Management > Data Masking" in the *Developer Guide*.

User-Defined Masking Functions
------------------------------

You can use the PL/pgSQL language to customize masking functions.

User-defined masking functions must meet the following requirements:

-  The return type must be the same as the data type of the redacted column.
-  The functions can be pushed down.
-  In addition to the masking format, only one column can be specified in the argument list for data masking.
-  The functions only implement the formatting for specific data types and do not involve complex association operations with other table objects.

If either of the first two requirements is not met, an error will be reported when you create a masking policy. If either of the last two requirements is not met, unexpected problems may occur in query execution results.
