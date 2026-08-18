:original_name: dws_06_0333.html

.. _dws_06_0333:

Array Functions
===============

Array functions perform basic operations on input array data, such as adding elements, searching for elements, converting arrays, and returning the processed array data.

:ref:`Table 1 <en-us_topic_0000001811634621__table9815145610547>` lists the array functions supported by DWS.

.. _en-us_topic_0000001811634621__table9815145610547:

.. table:: **Table 1** Array Functions

   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | Function                                                                                              | Description                                                                                                            | Example                                                                  |
   +=======================================================================================================+========================================================================================================================+==========================================================================+
   | :ref:`array_append(anyarray, anyelement) <en-us_topic_0000001811634621__section1828214072919>`        | Append an element to the end of an array.                                                                              | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT array_append(ARRAY[1,2], 3) AS RESULT;                         |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`array_prepend(anyelement, anyarray) <en-us_topic_0000001811634621__section546414434297>`        | Append an element to the beginning of an array.                                                                        | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT array_append(ARRAY[1,2], 3) AS RESULT;                         |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`array_cat(anyarray, anyarray) <en-us_topic_0000001811634621__section5545124717296>`             | Concatenate two arrays.                                                                                                | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT array_cat(ARRAY[1,2,3], ARRAY[4,5]) AS RESULT;                 |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`array_ndims(anyarray) <en-us_topic_0000001811634621__section918165132917>`                      | Return the number of dimensions of the array.                                                                          | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT array_dims(ARRAY[[1,2,3], [4,5,6]]) AS RESULT;                 |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`array_dims(anyarray) <en-us_topic_0000001811634621__section21931754122915>`                     | Return a text representation of array's dimensions.                                                                    | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT array_dims(ARRAY[[1,2,3], [4,5,6]]) AS RESULT;                 |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`array_length(anyarray, int) <en-us_topic_0000001811634621__section7232105742910>`               | Return the length of the requested array dimension.                                                                    | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT array_length(array[1,2,3], 1) AS RESULT;                       |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`array_lower(anyarray, int) <en-us_topic_0000001811634621__section8761306303>`                   | Return lower bound of the requested array dimension.                                                                   | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT array_lower('[0:2]={1,2,3}'::int[], 1) AS RESULT;              |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`array_upper(anyarray, int) <en-us_topic_0000001811634621__section107618283018>`                 | Return upper bound of the requested array dimension.                                                                   | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT array_upper(ARRAY[1,8,3,7], 1) AS RESULT;                      |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`array_to_string(anyarray, text [, text]) <en-us_topic_0000001811634621__section20497185133015>` | Convert an array into a string.                                                                                        | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT array_to_string(ARRAY[1, 2, 3, NULL, 5], ',', '*') AS RESULT;  |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`string_to_array(text, text [, text]) <en-us_topic_0000001811634621__section1191158173016>`      | Convert a string into an array.                                                                                        | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT string_to_array('xx~^~yy~^~zz', '~^~', 'yy') AS RESULT;        |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`unnest(anyarray) <en-us_topic_0000001811634621__section16222141123011>`                         | Expand an array to a set of rows.                                                                                      | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT unnest(ARRAY[1,2]) AS RESULT;                                  |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`interval(N, N1, N2, N3 ... ) <en-us_topic_0000001811634621__section197871051103016>`            | Search for the last array index that is less than or equal to the target parameter **n** from the input integer array. | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT INTERVAL(10, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 11) AS RESULT; |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+
   | :ref:`split(text, text) <en-us_topic_0000001811634621__section1866518595300>`                         | Separate strings by a delimiter and return an array.                                                                   | ::                                                                       |
   |                                                                                                       |                                                                                                                        |                                                                          |
   |                                                                                                       |                                                                                                                        |    SELECT SPLIT('a-b-c-d-e', '-') AS RESULT;                             |
   +-------------------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------+

.. _en-us_topic_0000001811634621__section1828214072919:

array_append(anyarray, anyelement)
----------------------------------

Description: Appends an element to the end of an array, and only supports dimension-1 arrays.

Return type: anyarray

Example: Append element **3** to the end of the array [1,2].

::

   SELECT array_append(ARRAY[1,2], 3) AS RESULT;
    result
   ---------
    {1,2,3}
   (1 row)

.. _en-us_topic_0000001811634621__section546414434297:

array_prepend(anyelement, anyarray)
-----------------------------------

Description: Appends an element to the beginning of an array, and only supports dimension-1 arrays.

Return type: anyarray

Example: Append element **1** to the beginning of the array [2,3].

::

   SELECT array_prepend(1, ARRAY[2,3]) AS RESULT;
    result
   ---------
    {1,2,3}
   (1 row)

.. _en-us_topic_0000001811634621__section5545124717296:

array_cat(anyarray, anyarray)
-----------------------------

Description: Concatenates two arrays, and supports multi-dimensional arrays.

Return type: anyarray

Example: Concatenate the array [1,2,3] with the array [4,5].

::

   SELECT array_cat(ARRAY[1,2,3], ARRAY[4,5]) AS RESULT;
      result
   -------------
    {1,2,3,4,5}
   (1 row)

Concatenate the two-dimensional array [[1,2],[4,5]] with the one-dimensional array [6,7].

::

   SELECT array_cat(ARRAY[[1,2],[4,5]], ARRAY[6,7]) AS RESULT;
          result
   ---------------------
    {{1,2},{4,5},{6,7}}
   (1 row)

.. _en-us_topic_0000001811634621__section918165132917:

array_ndims(anyarray)
---------------------

Description: Returns the number of dimensions of the array.

Return type: int

Example: View the number of dimensions of the array [[1,2,3], [4,5,6]].

::

   SELECT array_ndims(ARRAY[[1,2,3], [4,5,6]]) AS RESULT;
    result
   --------
         2
   (1 row)

.. _en-us_topic_0000001811634621__section21931754122915:

array_dims(anyarray)
--------------------

Description: Returns a text representation of array's dimensions.

Return type: text

Example: Obtain the dimensions of an array. The returned result indicates that the array is a two-dimensional array with 2 rows and 3 columns.

::

   SELECT array_dims(ARRAY[[1,2,3], [4,5,6]]) AS RESULT;
      result
   ------------
    [1:2][1:3]
   (1 row)

.. _en-us_topic_0000001811634621__section7232105742910:

array_length(anyarray, int)
---------------------------

Description: Returns the length of the requested array dimension.

Return type: int

Example: Calculate the length of the array [1,2,3] in the first dimension.

::

   SELECT array_length(array[1,2,3], 1) AS RESULT;
    result
   --------
         3
   (1 row)

.. _en-us_topic_0000001811634621__section8761306303:

array_lower(anyarray, int)
--------------------------

Description: Returns lower bound of the requested array dimension.

Return type: int

Example: Query the lower bound of the array [0:2]={1,2,3} in the first dimension.

::

   SELECT array_lower('[0:2]={1,2,3}'::int[], 1) AS RESULT;
    result
   --------
         0
   (1 row)

.. _en-us_topic_0000001811634621__section107618283018:

array_upper(anyarray, int)
--------------------------

Description: Returns upper bound of the requested array dimension.

Return type: int

Example: Query the upper bound of the array [1, 8, 3, 7] in the first dimension.

::

   SELECT array_upper(ARRAY[1,8,3,7], 1) AS RESULT;
    result
   --------
         4
   (1 row)

.. _en-us_topic_0000001811634621__section20497185133015:

array_to_string(anyarray, text [, text])
----------------------------------------

Description: Converts an array to a string, concatenates the elements using a specified delimiter, and defines how to handle NULL in the array.

The first parameter **text** is used as the new delimiter for the array, and the second parameter **text** is used to replace NULL in the array.

Return type: text

Example: Convert the array [1, 2, 3, NULL, 5] to a string, separate the elements with **,**, and replace NULL with **\***.

::

   SELECT array_to_string(ARRAY[1, 2, 3, NULL, 5], ',', '*') AS RESULT;
     result
   -----------
    1,2,3,*,5
   (1 row)

.. note::

   The second parameter **text** of this function controls how NULL elements in the array are handled. If this parameter is omitted or explicitly set to **NULL**, the function ignores all NULL elements in the array and they will not appear in the final output string.

   Example:

   Omit the second parameter **text**.

   ::

      SELECT array_to_string(ARRAY[1, NULL, 3, NULL, 5], ',') AS result;
       result
      --------
       1,3,5
      (1 row)

   Set the second parameter **text** to **NULL**.

   ::

      SELECT array_to_string(ARRAY[1, NULL, 3, NULL, 5], ',',NULL) AS RESULT;
       result
      --------
       1,3,5
      (1 row)

.. _en-us_topic_0000001811634621__section1191158173016:

string_to_array(text, text [, text])
------------------------------------

Description: Splits a string into an array based on a specified delimiter and replaces the specific value with NULL.

The second **text** indicates the specified delimiter. The third optional **text** replaces the specific value with NULL. If the separated substring exactly matches the third optional **text**, the substring is replaced with NULL.

Return type: text[]

Example: Split the string "xx~^~yy~^~zz" into arrays based on the delimiter "~^~" and replace the value "yy" with NULL.

::

   SELECT string_to_array('xx~^~yy~^~zz', '~^~', 'yy') AS RESULT;
       result
   --------------
    {xx,NULL,zz}
   (1 row)

The substring "yy" obtained after the string "xx~^~yy~^~zz" is split does not completely match the parameter "y" and will not be replaced with NULL.

::

   SELECT string_to_array('xx~^~yy~^~zz', '~^~', 'y') AS RESULT;
      result
   ------------
    {xx,yy,zz}
   (1 row)

.. note::

   -  The third parameter **text** of the function controls whether to replace the specific value with NULL. If this parameter is omitted or explicitly set to **NULL**, the function replaces the empty substring between two consecutive delimiters in the original string with NULL.

      Example:

      Omit the third parameter **text**.

      ::

         SELECT string_to_array('a,,b', ',') AS result;
           result
         ----------
          {a,"",b}
         (1 row)

      Set the third parameter **text** to **NULL**.

      ::

         SELECT string_to_array('a,,b', ',', NULL) AS result;
           result
         ----------
          {a,"",b}
         (1 row)

   -  In **string_to_array**, if the delimiter parameter is NULL, each character in the input string will become a separate element in the resulting array. If the delimiter is an empty string, the entire input string becomes a single-element array. Otherwise the input string is split at each occurrence of the delimiter string.

      Example:

      The delimiter parameter is NULL.

      ::

         SELECT string_to_array('abc', NULL) AS result;
          result
         ---------
          {a,b,c}
         (1 row)

      The delimiter is an empty string.

      ::

         SELECT string_to_array('abcde', ' ')AS result;
          result
         ---------
          {abcde}
         (1 row)

.. _en-us_topic_0000001811634621__section16222141123011:

unnest(anyarray)
----------------

Description: Expands an array to a set of rows.

Return type: setof anyelement

Example:

::

   SELECT unnest(ARRAY[1,2]) AS RESULT;
    result
   --------
         1
         2
   (2 rows)

The **unnest** function is used together with the :ref:`string_to_array(text, text [, text]) <en-us_topic_0000001811634621__section1191158173016>` array. To convert an array to columns, the statement first splits a string into arrays by comma, and then converts the arrays into columns.

::

   SELECT unnest(string_to_array('a,b,c,d',',')) AS RESULT;
    result
   --------
    a
    b
    c
    d
   (4 rows)

.. _en-us_topic_0000001811634621__section197871051103016:

interval(N, N1, N2, N3 ... )
----------------------------

Description: Searches for the last array index that is less than or equal to the target parameter **n** from the input integer array.

If n is NULL, **-1** is returned. The **interval()** function does not support the interval(N, N1) scenario.

Return type: int

Example: Find the interval to which the number **10** belongs in a sequence of numbers and return the corresponding interval index.

::

   SELECT INTERVAL(10, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 11) AS RESULT;
    result
   --------
        11
   (1 row)

.. _en-us_topic_0000001811634621__section1866518595300:

split(text, text)
-----------------

Description: Separates strings by a delimiter and returns an array.

The first parameter **text** is a string, and the second parameter **text** is a delimiter.

Return type: text[]

Example: Split the string "a-b-c-d-e" into multiple parts based on the delimiter "-" and return an array.

::

   SELECT SPLIT('a-b-c-d-e', '-') AS RESULT;
      result
   -------------
    {a,b,c,d,e}
   (1 row)

Split the string "a-b-c-d-e" based on the delimiter "-" and extract the fourth element.

::

   SELECT SPLIT('a-b-c-d-e', '-')[4] AS RESULT;
    result
   --------
    d
   (1 row)
