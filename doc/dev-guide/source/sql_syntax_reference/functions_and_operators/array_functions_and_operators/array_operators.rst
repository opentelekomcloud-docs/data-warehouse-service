:original_name: dws_06_0332.html

.. _dws_06_0332:

Array Operators
===============

Array comparisons compare the array contents element-by-element, using the default B-tree comparison function for the element data type. In multidimensional arrays, the elements are accessed in row-major order. If the contents of two arrays are equal but the dimensionality is different, the first difference in the dimensionality information determines the sort order. :ref:`Table 1 <en-us_topic_0000001811515637__table1559125515252>` lists the array operators supported by DWS.

.. _en-us_topic_0000001811515637__table1559125515252:

.. table:: **Table 1** Array operators

   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   | Type               | Description                                                                  | Operator                                                        | Example                                                       |
   +====================+==============================================================================+=================================================================+===============================================================+
   | Compare            | Check whether two arrays are equal.                                          | :ref:`= <en-us_topic_0000001811515637__section1160592019252>`   | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT ARRAY[1.1,2.1,3.1]::int[] = ARRAY[1,2,3] AS RESULT; |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   |                    | Check whether two arrays are not equal.                                      | :ref:`<> <en-us_topic_0000001811515637__section5912919102718>`  | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT ARRAY[1,2,3] <> ARRAY[1,2,4] AS RESULT;             |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   |                    | Check whether an array is smaller than another array.                        | :ref:`< <en-us_topic_0000001811515637__section11210323112710>`  | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT ARRAY[1,2,3] < ARRAY[1,2,4] AS RESULT;              |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   |                    | Check whether an array is greater than another array.                        | :ref:`> <en-us_topic_0000001811515637__section1457872642712>`   | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT ARRAY[1,4,3] > ARRAY[1,2,4] AS RESULT;              |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   |                    | Check whether an array is less than or equal to another array.               | :ref:`<= <en-us_topic_0000001811515637__section1805202962715>`  | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT ARRAY[1,2,3] <= ARRAY[1,2,3] AS RESULT;             |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   |                    | Check whether an array is greater than or equal to another array.            | :ref:`>= <en-us_topic_0000001811515637__section1185013262719>`  | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT ARRAY[1,4,3] >= ARRAY[1,4,3] AS RESULT;             |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   | Include or overlap | Check whether an array contains another array.                               | :ref:`@> <en-us_topic_0000001811515637__section4942113518278>`  | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT ARRAY[1,4,3] @> ARRAY[3,1] AS RESULT;               |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   |                    | Check whether an array is included in another array.                         | :ref:`<@ <en-us_topic_0000001811515637__section1880012385272>`  | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT ARRAY[2,7] <@ ARRAY[1,7,4,2,6] AS RESULT;           |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   |                    | Check whether an array overlaps with another array (shares common elements). | :ref:`&& <en-us_topic_0000001811515637__section2795134113275>`  | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT ARRAY[1,4,3] && ARRAY[2,1] AS RESULT;               |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   | Connect            | Perform array-to-array concatenation.                                        | :ref:`|| <en-us_topic_0000001811515637__section29565442278>`    | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT ARRAY[1,2,3] || ARRAY[4,5,6] AS RESULT;             |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   |                    | Perform element-to-array concatenation.                                      | :ref:`|| <en-us_topic_0000001811515637__section12911522276>`    | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT 3 || ARRAY[4,5,6] AS RESULT;                        |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+
   |                    | Perform array-to-element concatenation.                                      | :ref:`|| <en-us_topic_0000001811515637__section10481314202920>` | ::                                                            |
   |                    |                                                                              |                                                                 |                                                               |
   |                    |                                                                              |                                                                 |    SELECT ARRAY[4,5,6] || 7 AS RESULT;                        |
   +--------------------+------------------------------------------------------------------------------+-----------------------------------------------------------------+---------------------------------------------------------------+

.. _en-us_topic_0000001811515637__section1160592019252:

``=``
-----

Description: Specifies whether two arrays are equal.

Example:

::

   SELECT ARRAY[1.1,2.1,3.1]::int[] = ARRAY[1,2,3] AS RESULT;
    result
   --------
    t
   (1 row)

.. _en-us_topic_0000001811515637__section5912919102718:


<>
--

Description: Specifies whether two arrays are not equal.

Example:

::

   SELECT ARRAY[1,2,3] <> ARRAY[1,2,4] AS RESULT;
    result
   --------
    t
   (1 row)

.. _en-us_topic_0000001811515637__section11210323112710:


``<``
-----

Description: Specifies whether an array is less than another.

Example:

::

   SELECT ARRAY[1,2,3] < ARRAY[1,2,4] AS RESULT;
    result
   --------
    t
   (1 row)

.. _en-us_topic_0000001811515637__section1457872642712:


``>``
-----

Description: Specifies whether an array is greater than another.

Example:

::

   SELECT ARRAY[1,4,3] > ARRAY[1,2,4] AS RESULT;
    result
   --------
    t
   (1 row)

.. _en-us_topic_0000001811515637__section1805202962715:


<=
--

Description: Specifies whether an array is less than another.

Example:

::

   SELECT ARRAY[1,2,3] <= ARRAY[1,2,3] AS RESULT;
    result
   --------
    t
   (1 row)

.. _en-us_topic_0000001811515637__section1185013262719:


>=
--

Description: Specifies whether an array is greater than or equal to another.

Example:

::

   SELECT ARRAY[1,4,3] >= ARRAY[1,4,3] AS RESULT;
    result
   --------
    t
   (1 row)

.. _en-us_topic_0000001811515637__section4942113518278:


@>
--

Description: Specifies whether an array contains another.

Example:

::

   SELECT ARRAY[1,4,3] @> ARRAY[3,1] AS RESULT;
    result
   --------
    t
   (1 row)

.. _en-us_topic_0000001811515637__section1880012385272:


<@
--

Description: Specifies whether an array is contained in another.

Example:

::

   SELECT ARRAY[2,7] <@ ARRAY[1,7,4,2,6] AS RESULT;
    result
   --------
    t
   (1 row)

.. _en-us_topic_0000001811515637__section2795134113275:


&&
--

Description: Specifies whether an array overlaps another (have common elements).

Example:

::

   SELECT ARRAY[1,4,3] && ARRAY[2,1] AS RESULT;
    result
   --------
    t
   (1 row)

.. _en-us_topic_0000001811515637__section29565442278:


\|\|
----

Description: Specifies array-to-array concatenation.

Example:

::

   SELECT ARRAY[1,2,3] || ARRAY[4,5,6] AS RESULT;
       result
   ---------------
    {1,2,3,4,5,6}
   (1 row)

::

   SELECT ARRAY[1,2,3] || ARRAY[[4,5,6],[7,8,9]] AS RESULT;
             result
   ---------------------------
    {{1,2,3},{4,5,6},{7,8,9}}
   (1 row)

.. _en-us_topic_0000001811515637__section12911522276:


\|\|
----

Description: Specifies element-to-array concatenation.

Example:

::

   SELECT 3 || ARRAY[4,5,6] AS RESULT;
     result
   -----------
    {3,4,5,6}
   (1 row)

.. _en-us_topic_0000001811515637__section10481314202920:


\|\|
----

Description: Specifies array-to-element concatenation.

Example:

::

   SELECT ARRAY[4,5,6] || 7 AS RESULT;
     result
   -----------
    {4,5,6,7}
   (1 row)
