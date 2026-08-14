:original_name: dws_06_0335.html

.. _dws_06_0335:

Range Functions
===============

Range functions are used to define the upper and lower bounds of a range and determine whether a value is within the range.

lower(anyrange)
---------------

Description: Obtains the lower bound of a range. The function returns **null** if the range is empty or the lower bound is infinite.

Return type: Range's element type

Example:

::

   SELECT lower(numrange(1.1,2.2)) AS RESULT;
    result
   --------
       1.1
   (1 row)

upper(anyrange)
---------------

Description: Obtains the upper bound of a range. The function returns **null** if the range is empty or the upper bound is infinite.

Return type: Range's element type

Example:

::

   SELECT upper(numrange(1.1,2.2)) AS RESULT;
    result
   --------
       2.2
   (1 row)

isempty(anyrange)
-----------------

Description: Determines whether a range is empty.

Return type: Boolean

Example:

::

   SELECT isempty(numrange(1.1,2.2)) AS RESULT;
    result
   --------
    f
   (1 row)

.. _en-us_topic_0000001811515649__en-us_topic_0000001495702129_section19905992346:

lower_inc(anyrange)
-------------------

Description: Determines whether the lower bound of a range is included.

Return type: Boolean

Example:

::

   SELECT lower_inc(numrange(1.1,2.2)) AS RESULT;
    result
   --------
    t
   (1 row)

.. _en-us_topic_0000001811515649__en-us_topic_0000001495702129_section1084113120348:

upper_inc(anyrange)
-------------------

Description: Determines whether the upper bound of a range is included.

Return type: Boolean

Example:

**numrange(1.1, 2.2)** uses the [1.1, 2.2) format by default. The upper bound (2.2) is not included by default. Therefore, **false** is returned.

::

   SELECT upper_inc(numrange(1.1,2.2)) AS RESULT;
    result
   --------
    f
   (1 row)

.. _en-us_topic_0000001811515649__en-us_topic_0000001495702129_section3663181517345:

lower_inf(anyrange)
-------------------

Description: Determines whether the lower bound of a range is infinite.

Return type: Boolean

Example:

::

   SELECT lower_inf('(,)'::daterange) AS RESULT;
    result
   --------
    t
   (1 row)

.. _en-us_topic_0000001811515649__en-us_topic_0000001495702129_section1578181883411:

upper_inf(anyrange)
-------------------

Description: Determines whether the upper bound of a range is infinite.

Return type: Boolean

Example:

::

   SELECT upper_inf('(,)'::daterange) AS RESULT;
    result
   --------
    t
   (1 row)

.. note::

   The **lower_inc**, **upper_inc**, **lower_inf**, and **upper_inf** functions all return false for an empty range.
