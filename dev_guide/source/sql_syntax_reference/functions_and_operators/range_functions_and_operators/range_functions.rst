:original_name: dws_06_0335.html

.. _dws_06_0335:

Range Functions
===============

lower(anyrange)
---------------

Description: Lower bound of range

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

Description: Upper bound of range

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

Description: Is the range empty?

Return type: boolean

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

Description: Is the lower bound inclusive?

Return type: boolean

Example:

::

   SELECT lower_inc(numrange(1.1,2.2)) AS RESULT;
    result
   --------
    t
   (1 row)

upper_inc(anyrange)
-------------------

Description: Is the upper bound inclusive?

Return type: boolean

Example:

::

   SELECT upper_inc(numrange(1.1,2.2)) AS RESULT;
    result
   --------
    f
   (1 row)

.. _en-us_topic_0000001811515649__en-us_topic_0000001495702129_section3663181517345:

lower_inf(anyrange)
-------------------

Description: Is the lower bound infinite?

Return type: boolean

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

Description: Is the upper bound infinite?

Return type: boolean

Example:

::

   SELECT upper_inf('(,)'::daterange) AS RESULT;
    result
   --------
    t
   (1 row)

.. note::

   The **lower** and **upper** functions return null if the range is empty or the requested bound is infinite. The **lower_inc**, **upper_inc**, **lower_inf**, and **upper_inf** functions all return false for an empty range.
