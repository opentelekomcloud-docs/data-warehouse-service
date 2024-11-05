:original_name: dws_06_0216.html

.. _dws_06_0216:

FETCH
=====

Function
--------

**FETCH** retrieves data using a previously-created cursor.

A cursor has an associated position, which is used by **FETCH**. The cursor position can be before the first row of the query result, on any particular row of the result, or after the last row of the result.

-  When created, a cursor is positioned before the first row.
-  After fetching some rows, the cursor is positioned on the row most recently retrieved.
-  If **FETCH** runs off the end of the available rows then the cursor is left positioned after the last row, or before the first row if fetching backward.
-  **FETCH ALL** or **FETCH BACKWARD ALL** will always leave the cursor positioned after the last row or before the first row.

Precautions
-----------

-  If **NO SCROLL** is defined for the cursor, a backward fetch like **FETCH BACKWARD** is not allowed.
-  The forms **NEXT**, **PRIOR**, **FIRST**, **LAST**, **ABSOLUTE**, and **RELATIVE** appropriately fetch a record after moving the cursor. If the cursor is already after the last row before being moved, an empty result is returned, and the cursor is left positioned before the first row (backward fetch) or after the last row (forward fetch) as appropriate.
-  The forms using **FORWARD** and **BACKWARD** retrieve the indicated number of rows moving in the forward or backward direction, leaving the cursor positioned on the last-returned row (or after (backward fetch)/before (forward fetch) all rows, if the count exceeds the number of rows available).
-  **RELATIVE 0**, **FORWARD 0**, and **BACKWARD 0** all request fetching the current row without moving the cursor, that is, re-fetching the most recently fetched row. This will succeed unless the cursor is positioned before the first row or after the last row, in which case, no row is returned.
-  If the cursor of **FETCH** involves a column-store table, backward fetches like **BACKWARD**, **PRIOR**, and **FIRST** are not supported.

Syntax
------

::

   FETCH [ direction { FROM | IN } ] cursor_name;

The **direction** clause specifies optional parameters.

::

   NEXT
      | PRIOR
      | FIRST
      | LAST
      | ABSOLUTE count
      | RELATIVE count
      | count
      | ALL
      | FORWARD
      | FORWARD count
      | FORWARD ALL
      | BACKWARD
      | BACKWARD count
      | BACKWARD ALL

.. _en-us_topic_0000001460880956__s680662240a104ac7a51873c7c888bdd1:

Parameter Description
---------------------

-  **direction_clause**

   Defines the fetch direction.

   Valid value:

   -  **NEXT** (default value)

      Fetches the next row.

   -  PRIOR

      Fetches the prior row.

   -  FIRST

      Fetches the first row of the query (same as **ABSOLUTE 1**).

   -  LAST

      Fetches the last row of the query (same as **ABSOLUTE -1**).

   -  ABSOLUTE count

      Fetches the (**count**)'th row of the query.

      **ABSOLUTE** fetches are not any faster than navigating to the desired row with a relative move: the underlying implementation must traverse all the intermediate rows anyway.

      **count** is a possibly-signed integer constant:

      -  If **count** is a positive integer, fetches the (count)'th row of the query, starting from the first row. If **count** is less than the current cursor position, a **rewind** operation is required, which is currently not supported.
      -  If **count** is a negative value or 0, a backward scanning is required, which is currently not supported.

   -  RELATIVE count

      Fetches the (count)'th succeeding row, or the abs(count)'th prior row if count is negative.

      **count** is a possibly-signed integer constant:

      -  If **count** is a positive integer, fetches the (count)'th succeeding row.
      -  If **count** is a negative value, a backward scanning is required, which is currently not supported.
      -  **RELATIVE 0** fetches the current row.

   -  count

      Fetches the next **count** rows (same as **FORWARD count**).

   -  ALL

      Fetches all remaining rows (same as **FORWARD ALL**).

   -  FORWARD

      Fetches the next row (same as **NEXT**).

   -  FORWARD count

      Fetches the next **count** rows (same as **RELATIVE count**). **FORWARD 0** re-fetches the current row.

   -  FORWARD ALL

      Fetches all remaining rows.

   -  BACKWARD

      Fetches the prior row (same as **PRIOR**).

   -  BACKWARD count

      Fetches the prior **count** rows (scanning backwards).

      **count** is a possibly-signed integer constant:

      -  If **count** is a positive integer, fetches the (count)'th prior row.
      -  If **count** is a negative integer, fetches the abs(count)'th succeeding row.
      -  **BACKWARD 0** re-fetches the current row.

   -  BACKWARD ALL

      Fetches all prior rows (scanning backwards).

-  **{ FROM \| IN } cursor_name**

   Specifies the cursor name using the keyword **FROM** or **IN**.

   Value range: an existing cursor name.

Examples
--------

Example 1: Run the **SELECT** statement to read a table using a cursor.

Set up the **cursor1** cursor:

::

   CURSOR cursor1 FOR SELECT * FROM tpcds.customer_address ORDER BY 1;

Fetch the first three rows from **cursor1**:

::

   FETCH FORWARD 3 FROM cursor1;
    ca_address_sk |  ca_address_id   | ca_street_number |   ca_street_name   | ca_street_type  | ca_suite_number |     ca_city     |    ca_county    | ca_state |   ca_zip   |  ca_country   | ca_gmt_offset |   ca_location_type
   ---------------+------------------+------------------+--------------------+-----------------+-----------------+-----------------+-----------------+----------+------------+---------------+---------------+----------------------
                1 | AAAAAAAABAAAAAAA | 18               | Jackson            | Parkway         | Suite 280       | Fairfield       | Maricopa County | AZ       | 86192      | United States |         -7.00 | condo
                2 | AAAAAAAACAAAAAAA | 362              | Washington 6th     | RD              | Suite 80        | Fairview        | Taos County     | NM       | 85709      | United States |         -7.00 | condo
                3 | AAAAAAAADAAAAAAA | 585              | Dogwood Washington | Circle          | Suite Q         | Pleasant Valley | York County     | PA       | 12477      | United States |         -5.00 | single family
   (3 rows)

Example 2: Use a cursor to read the content in the **VALUES** clause.

Set up the cursor **cursor2**:

::

   CURSOR cursor2 FOR VALUES(1,2),(0,3) ORDER BY 1;

Fetch the first two rows from **cursor2**:

::

   FETCH FORWARD 2 FROM cursor2;
   column1 | column2
   ---------+---------
   0 |       3
   1 |       2
   (2 rows)

Helpful Links
-------------

:ref:`CLOSE <dws_06_0152>`, :ref:`MOVE <dws_06_0217>`
