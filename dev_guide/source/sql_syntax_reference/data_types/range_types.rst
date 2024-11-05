:original_name: dws_06_0370.html

.. _dws_06_0370:

Range Types
===========

A range type is a data type that represents a range of values for certain element types (subtypes of ranges). For example, the range of a timestamp may be used to express a time range in which a conference room is reserved. In this case, the data type is **tsrange** (short for timestamp range), and **timestamp** is the subtype. The subtype must have an overall order so that the element value can be clearly specified as within, before, or after a range.

Range types can express multiple element values in a single range value and can clearly express concepts such as range overlapping. It helps to schedule the timing and duration of an event, and can also be utilized to set price or measurement parameters for equipment.

Built-in Range Types
--------------------

GaussDB(DWS) has the following built-in range types:

-  int4range: integer range.
-  int8range: bigint range.
-  numrange: numeric range.
-  tsrange: range of timestamp without the time zone.
-  tstzrange: range of timestamp with the time zone
-  daterange: date range.

.. note::

   GaussDB(DWS) does not support user-defined range types.

Create the reservation table and insert data into the table. The **during** column is of the **tsrange** type.

.. code-block::

   CREATE TABLE reservation (room int, during tsrange);
   INSERT INTO reservation VALUES (1108, '[2010-01-01 14:30, 2010-01-01 15:30)');

Query whether the range contains specific values:

.. code-block::

   SELECT int4range(10, 20) @> 3;
    ?column?
   ----------
    f
   (1 row)

Query whether the range overlaps with another value:

.. code-block::

   SELECT numrange(11.1, 22.2) && numrange(20.0, 30.0);
    ?column?
   ----------
    t
   (1 row)

Obtain the upper bound of the range:

.. code-block::

   SELECT upper(int8range(15, 25));
    upper
   -------
       25
   (1 row)

Calculate the intersection with another range:

.. code-block::

   SELECT int4range(10, 20) * int4range(15, 25);
    ?column?
   ----------
    [15,20)
   (1 row)

Query whether the range is empty:

.. code-block::

   SELECT isempty(numrange(1, 5));
    isempty
   ---------
    f
   (1 row)

Inclusion and Exclusion of Bounds
---------------------------------

Each non-empty range has two bounds: a lower bound and an upper bound. All values between the two bounds are included in the range. Inclusion of bounds means that boundary values are also included in the range, while exclusion of bounds means that boundary values are not included in the range.

Inclusion of lower bound is represented by "[", and exclusion of lower bound is represented by "(". Similarly, inclusion of upper bound is represented by "]", and exclusion of upper bound is represented by ")".

The :ref:`lower_inc(anyrange) <en-us_topic_0000001510281961__en-us_topic_0000001495702129_section19905992346>` and :ref:`lower_inc(anyrange) <en-us_topic_0000001510281961__en-us_topic_0000001495702129_section19905992346>` functions are used to test the upper and lower bounds of a range, respectively.

Infinite (Unbounded) Range
--------------------------

When the lower bound of a range is unbounded, it means that all values less than the upper bound are included in the range, for example, (,3]. Similarly, when the upper bound of a range is unbounded, all values greater than the upper bound are included in the range. When both the upper and lower bounds are unbounded, all values of the element type are considered within the range. The missing bounds are automatically converted to exclusions, for example, [,] is converted to (,). You can think of these missing values as positive infinity or negative infinity, but they are special range type values and are considered to be positive and negative infinity values that go beyond any range element type.

Element types with the infinity values can be used as explicit bound values. For example, in the timestamp range, [today, infinity) does not include a special timestamp value **infinity**.

The functions :ref:`lower_inf(anyrange) <en-us_topic_0000001510281961__en-us_topic_0000001495702129_section3663181517345>` and :ref:`upper_inf(anyrange) <en-us_topic_0000001510281961__en-us_topic_0000001495702129_section1578181883411>` are used to test whether a range has infinite upper bound or lower bound, respectively.

Input/Output Values of a Range
------------------------------

Range values must follow one of the following patterns:

.. code-block::

        (lower-bound,upper-bound)
        (lower-bound,upper-bound]
        [lower-bound,upper-bound)
        [lower-bound,upper-bound]
        empty

Parentheses () or square brackets [] indicate whether the upper and lower bounds are excluded or included. The last format is **empty**, which represents an empty range (a range that does not contain values).

The value of *lower-bound* can be a valid string of the subtype, or left empty, which indicates that there is no lower bound. The value of *upper-bound* can be a valid string of the subtype, or null, which indicates that there is no upper bound.

Each bound value can be referenced using the quotation marks (""). This is necessary if the bound value contains parentheses (), square brackets [], commas (,), quotation marks (""), or backslashes (\\), because otherwise those characters will be considered part of the range syntax. To put a quotation mark or backslash in a bound value, put a backslash in front of it (and a pair of double quotation marks in a referenced bound value represents one quotation mark character, which is similar to the single quotation mark rule in SQL strings). In addition, you can avoid referencing and use backslash escapes to preventing data characters from being used as part of the return syntax. If you want to input a bound value that is an empty string, write "", indicating infinite bounds.

Spaces are allowed before and after a range value, but any space between parentheses() or square brackets[] is used as part of the upper or lower bound value (depending on the element type, the space may or may not represent a value).

**Examples**

Query all values between 3 and 7, including 3 and excluding 7:

.. code-block::

   SELECT '[3,7)'::int4range;
    int4range
   -----------
    [3,7)
   (1 row)

Query all values between 3 and 7, excluding 3 and 7:

.. code-block::

   SELECT '(3,7)'::int4range;
    int4range
   -----------
    [4,7)
   (1 row)

Query the value 4:

.. code-block::

   SELECT '[4,4]'::int4range;
    int4range
   -----------
    [4,5)
   (1 row)

Query a range containing no values (normalized to **empty**):

.. code-block::

   SELECT '[4,4)'::int4range;
    int4range
   -----------
    empty
   (1 row)

Constructing Range
------------------

Each range type has a constructor function with the same name. Using constructor functions is more convenient than writing a range literal constant because it avoids extra references to bound values. Constructor functions accept two or three parameters. Two parameters form a range in the standard form, where the lower bound is included and the upper bound is excluded. If a range contains three parameters, the third parameter specifies the range exclusion/inclusion type. The third parameter must be one of the following character strings: (), (], [], or []. The following is an example:

The complete form is: lower bound, upper bound, and textual parameters indicating the inclusion/exclusion of bounds.

.. code-block::

   SELECT numrange(1.0, 14.0, '(]');
     numrange
   ------------
    (1.0,14.0]
   (1 row)

If the third parameter is ignored, the range will be deemed as '[)'.

.. code-block::

   SELECT numrange(1.0, 14.0);
     numrange
   ------------
    [1.0,14.0)
   (1 row)

Although '(]' is specified here, it is converted to the standard form in the return result because int8range is a type of :ref:`Discrete Ranges <en-us_topic_0000001665098248__en-us_topic_0000001645397490_section19719344172411>`:

.. code-block::

   SELECT int8range(1, 14, '(]');
    int8range
   -----------
    [2,15)
   (1 row)

**NULL** indicates that the range is unbounded.

.. code-block::

   SELECT numrange(NULL, 2.2);
    numrange
   ----------
    (,2.2)
   (1 row)

.. _en-us_topic_0000001665098248__en-us_topic_0000001645397490_section19719344172411:

Discrete Ranges
---------------

A discrete range is a range in which its elements have a clearly defined "step", such as integer or date. When there is no valid value between two elements, they are adjacent.

Each element value in a discrete range has a next value and a previous value. You can use the next or previous value to convert between inclusion and exclusion. For example, in an integer range, [4,8] and (3,9) represent the same set of values, but this is not the case for ranges other than numeric ranges.

A discrete range should have a canonical function for identifying the step in the range. The normalization function can convert the equivalents of the range type to expressions of the same meanings, which are consistent with the inclusion or exclusion bounds. If no normalization function is specified, ranges with different formats are always considered unequal, even if they actually express the same set of values.

The built-in range types int4range, int8range, and daterange use the normalized form, which includes the lower bound and excludes the upper bound, that is, [). However, user-defined ranges types can use other conventions.

User-defined Range Types
------------------------

Users can define range types. To create a range type subtype float8, execute the following commands:

.. code-block::

   CREATE TYPE floatrange AS RANGE (
           subtype = float8,
           subtype_diff = float8mi
       );

   SELECT '[1.234, 5.678]'::floatrange;

Indexes
-------

You can create GiST indexes for table columns of the range type. Example:

.. code-block::

   CREATE TABLE reservation (room int, during tsrange);
   CREATE INDEX reservation_idx ON reservation USING GIST (during);

GiST indexes can accelerate queries involving the following range operators: =, &&, <@, @>, <>, ``-|-``, &<, and &. >. For details, see :ref:`Range Operators <en-us_topic_0000001510520885>`.

In addition, you can also create B-tree indexes on table columns of the range type. For these index types, basically the only useful range operation is equivalence. B-tree indexes for range types are primarily used to enable sorting within a query. They are not actually created.
