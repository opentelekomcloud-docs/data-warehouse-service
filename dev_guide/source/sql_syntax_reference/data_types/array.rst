:original_name: dws_06_0368.html

.. _dws_06_0368:

Array
=====

An array is a set of data. The array type allows a single database field to have multiple values. It is usually used to store and process data with similar attributes.

Syntax
------

::

   ARRAY [ param ]

or

::

   '{ param }'

The **param** parameter is described as follows:

-  param: value contained in the array. It can be zero or many. Multiple values are separated by commas (,). If there is no value, set this parameter to NULL.
-  When you specify an array constant in '{ *param* }' format, the elements of the string type cannot start or end with a single quotation mark. Instead, a double quotation mark is needed. Two consecutive single quotation marks will be converted to one single quotation mark.
-  The data type of the first element is used as the data type of the array. All elements in an array must be of the same type, or their types can be converted to each other.

Definition of the Array Type
----------------------------

An array type is defined by adding values in the square brackets ([]) at the end of a data type.

Create the **books** table. The type of the **price** column (book prices) is a one-dimensional integer array, and the type of the **tags** column (book tags) is a two-dimensional text array.

::

   CREATE TABLE books (id SERIAL PRIMARY KEY, title VARCHAR(100), price_by_quarter int[], tags TEXT[][]);

The **CREATE TABLE** syntax can specify the array size. For example:

::

   CREATE TABLE test ( a int[3]);

In current database implementation, the array size limits of statements is ignored. Statements with array size limits behave the same as those without. At the same time, the number of declared dimensions is not required. Arrays of a particular element type are all treated as the same type. Their size or number of dimensions are ignored.

You can also use the keyword **ARRAY** to define a one-dimensional array. The **price** column in the **books** table is defined using **ARRAY** and the array size is specified as follows:

::

   price_by_quarter int ARRAY[4]

Use **ARRAY** without specifying the array size:

::

   price_by_quarter int ARRAY

Array Value Input
-----------------

When entering an array value, write each array value as a literal constant, surround element values with braces, and separate them with commas. The general format of an array constant is as follows:

::

   '{ val1 delim val2 delim ... }'

**delim** indicates a delimiter, and each **val** may be a constant or a subarray of an array element type.

An example of an array constant is as follows:

::

   '{{1,2,3},{4,5,6},{7,8,9}}'

The constant is a two-dimensional, 3-by-3 array consisting of three integer subarrays.

Insert data into the **books** table and query the **books** table.

::

   INSERT INTO books
     VALUES (1, 'One Hundred years of Solitude','{25,25,25,25}','{{"fiction"}, {"adventure"}}'),
            (2, 'Robinson Crusoe', '{30,32,32,32}', '{{"adventure"}, {"fiction"}}'),
            (3, 'Gone with the Wind', '{27,27,29,28}', '{{"romance"}, {"fantasy"}}');

   SELECT * FROM books;
    id |             title             | price_by_quarter |          tags
   ----+-------------------------------+------------------+-------------------------
     1 | One Hundred years of Solitude | {25,25,25,25}    | {{fiction},{adventure}}
     2 | Robinson Crusoe               | {30,32,32,32}    | {{adventure},{fiction}}
     3 | Gone with the Wind            | {27,27,29,28}    | {{romance},{fantasy}}
   (3 rows)

.. caution::

   When multi-dimensional array data is inserted, each dimension of the array must have a matching length.

Use the **ARRAY** keyword to insert data.

.. code-block::

   INSERT INTO books
     VALUES (1, 'One Hundred years of Solitude',ARRAY[25,25,25,25],ARRAY['fiction', 'adventure']),
            (2, 'Robinson Crusoe', ARRAY[30,32,32,32], ARRAY['adventure', 'fiction']),
            (3, 'Gone with the Wind', ARRAY[27,27,29,28], ARRAY['romance', 'fantasy']);

Accessing Arrays
----------------

**Accessing Array Elements**

Query the names of the books whose prices change in the second quarter.

::

   SELECT title FROM books WHERE price_by_quarter[1] <> price_by_quarter[2];
         title
   -----------------
    Robinson Crusoe
   (1 row)

Search for the prices of all books in the third quarter:

::

   SELECT price_by_quarter[3] FROM books;
    price_by_quarter
   ------------------
                  29
                  25
                  32
   (3 rows)

**Accessing Array Slices**

Any rectangular slice or subarray of an array can be accessed. An array slice can be defined by specifying [lower bound: upper bound] on one or more array dimensions.

Query the second label of **Gone with the Wind**.

::

   SELECT tags[2:2] FROM books WHERE title = 'Gone with the Wind';
      tags
   -----------
    {fantasy}
   (1 row)

**Using Functions to Access Arrays**

Use the **array_dims** function to obtain the dimension of an array value.

::

   SELECT array_dims(tags) FROM books WHERE title = 'Robinson Crusoe';
    array_dims
   ------------
    [1:2]
   (1 row)

You can also use **array_upper** and **array_lower** to obtain the array dimension. They return the upper and lower bounds of a specified array, respectively.

::

   SELECT array_upper(tags, 1) FROM books WHERE title = 'Robinson Crusoe';
    array_upper
   -------------
              2
   (1 row)

The **array_length** function returns the length of a specified array dimension.

::

   SELECT array_length(tags, 1) FROM books WHERE title = 'Robinson Crusoe';
    array_length
   --------------
               2
   (1 row)

Modifying an Array
------------------

**Updating an Array**

Update the entire array data:

.. code-block::

   UPDATE books SET price_by_quarter = '{30,30,30,30}'
        WHERE title = 'Robinson Crusoe';

Use the ARRAY expression to update the entire array data:

.. code-block::

   UPDATE books SET price_by_quarter = ARRAY[30,30,30,30]
        WHERE title = 'Robinson Crusoe';

Update an element in the array:

.. code-block::

   UPDATE books SET price_by_quarter[4] = 35
        WHERE title = 'Robinson Crusoe';

Update a slice element in the array:

.. code-block::

   UPDATE books SET price_by_quarter[1:2] = '{27,27}'
        WHERE title = 'Robinson Crusoe';

A stored array value can be expanded by assigning a value to a new element. Positions between an existing element and the new element is filled with null values. For example, if the array **myarray** currently has four elements, it will have six elements after a value is assigned to **myarray[6]** using **UPDATE**. **myarray[5]** is filled with null. Currently, this method can be used only on one-dimensional arrays.

**Building a New Array**

New arrays can also be constructed using the concatenation operator \||. The concatenation operator allows a single element to be added to the beginning or end of a one-dimensional array. It can also accept two *N* dimensional arrays, or an N dimensional array and an N+1 dimensional array.

.. code-block::

   SELECT ARRAY[1,2] || ARRAY[3,4];
    ?column?
   -----------
    {1,2,3,4}
   (1 row)

   SELECT ARRAY[5,6] || ARRAY[[1,2],[3,4]];
         ?column?
   ---------------------
    {{5,6},{1,2},{3,4}}
   (1 row)

Use the **array_prepend**, **array_append**, or **array_cat function** to build an array.

.. code-block::

   SELECT array_prepend(1, ARRAY[2,3]);
    array_prepend
   ---------------
    {1,2,3}
   (1 row)

   SELECT array_append(ARRAY[1,2], 3);
    array_append
   --------------
    {1,2,3}
   (1 row)

   SELECT array_cat(ARRAY[1,2], ARRAY[3,4]);
    array_cat
   -----------
    {1,2,3,4}
   (1 row)

   SELECT array_cat(ARRAY[[1,2],[3,4]], ARRAY[5,6]);

         array_cat
   ---------------------
    {{1,2},{3,4},{5,6}}
   (1 row)

   SELECT array_cat(ARRAY[5,6], ARRAY[[1,2],[3,4]]);
         array_cat
   ---------------------
    {{5,6},{1,2},{3,4}}
   (1 row)
