:original_name: dws_06_0333.html

.. _dws_06_0333:

Array Functions
===============

array_append(anyarray, anyelement)
----------------------------------

Description: Appends an element to the end of an array, and only supports dimension-1 arrays.

Return type: anyarray

Example:

::

   SELECT array_append(ARRAY[1,2], 3) AS RESULT;
    result
   ---------
    {1,2,3}
   (1 row)

array_prepend(anyelement, anyarray)
-----------------------------------

Description: Appends an element to the beginning of an array, and only supports dimension-1 arrays.

Return type: anyarray

Example:

::

   SELECT array_prepend(1, ARRAY[2,3]) AS RESULT;
    result
   ---------
    {1,2,3}
   (1 row)

array_cat(anyarray, anyarray)
-----------------------------

Description: Concatenates two arrays, and supports multi-dimensional arrays.

Return type: anyarray

Example:

::

   SELECT array_cat(ARRAY[1,2,3], ARRAY[4,5]) AS RESULT;
      result
   -------------
    {1,2,3,4,5}
   (1 row)

   SELECT array_cat(ARRAY[[1,2],[4,5]], ARRAY[6,7]) AS RESULT;
          result
   ---------------------
    {{1,2},{4,5},{6,7}}
   (1 row)

array_ndims(anyarray)
---------------------

Description: Returns the number of dimensions of the array.

Return type: int

Example:

::

   SELECT array_ndims(ARRAY[[1,2,3], [4,5,6]]) AS RESULT;
    result
   --------
         2
   (1 row)

array_dims(anyarray)
--------------------

Description: Returns a text representation of array's dimensions.

Return type: text

Example:

::

   SELECT array_dims(ARRAY[[1,2,3], [4,5,6]]) AS RESULT;
      result
   ------------
    [1:2][1:3]
   (1 row)

array_length(anyarray, int)
---------------------------

Description: Returns the length of the requested array dimension.

Return type: int

Example:

::

   SELECT array_length(array[1,2,3], 1) AS RESULT;
    result
   --------
         3
   (1 row)

array_lower(anyarray, int)
--------------------------

Description: Returns lower bound of the requested array dimension.

Return type: int

Example:

::

   SELECT array_lower('[0:2]={1,2,3}'::int[], 1) AS RESULT;
    result
   --------
         0
   (1 row)

array_upper(anyarray, int)
--------------------------

Description: Returns upper bound of the requested array dimension.

Return type: int

Example:

::

   SELECT array_upper(ARRAY[1,8,3,7], 1) AS RESULT;
    result
   --------
         4
   (1 row)

array_to_string(anyarray, text [, text])
----------------------------------------

Description: Uses the first **text** as the new delimiter and the second **text** to replace **NULL** values.

Return type: text

Example:

::

   SELECT array_to_string(ARRAY[1, 2, 3, NULL, 5], ',', '*') AS RESULT;
     result
   -----------
    1,2,3,*,5
   (1 row)

.. note::

   In **array_to_string**, if the null-string parameter is omitted or NULL, any null elements in the array are simply skipped and not represented in the output string.

string_to_array(text, text [, text])
------------------------------------

Description: Uses the second **text** as the new delimiter and the third **text** as the substring to be replaced by **NULL** values. A substring can be replaced by **NULL** values only when it is the same as the third **text**.

Return type: text[]

Example:

::

   SELECT string_to_array('xx~^~yy~^~zz', '~^~', 'yy') AS RESULT;
       result
   --------------
    {xx,NULL,zz}
   (1 row)
   SELECT string_to_array('xx~^~yy~^~zz', '~^~', 'y') AS RESULT;
      result
   ------------
    {xx,yy,zz}
   (1 row)

.. note::

   -  In **string_to_array**, if the delimiter parameter is NULL, each character in the input string will become a separate element in the resulting array. If the delimiter is an empty string, then the entire input string is returned as a one-element array. Otherwise the input string is split at each occurrence of the delimiter string.
   -  In **string_to_array**, if the null-string parameter is omitted or NULL, none of the substrings of the input will be replaced by NULL.

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

The **unnest** function is used together with the **string_to_array** array. To convert an array to columns, the statement first splits a string into arrays by comma, and then converts the arrays into columns.

::

   SELECT unnest(string_to_array('a,b,c,d',',')) AS RESULT;
    result
   --------
    a
    b
    c
    d
   (4 rows)
