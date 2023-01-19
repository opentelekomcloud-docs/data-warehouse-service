:original_name: dws_06_0188.html

.. _dws_06_0188:

CURSOR
======

Function
--------

CURSOR defines a cursor. This command retrieves few rows of data in a query.

To process SQL statements, the stored procedure process assigns a memory segment to store context association. Cursors are handles or pointers to context regions. With cursors, stored procedures can control alterations in context regions.

Precautions
-----------

-  **CURSOR** is used only in transaction blocks.
-  Generally, **CURSOR** and **SELECT** both have text returns. Since data is stored in binary format in the system, the system needs to convert the data from the binary format to the text format. If data is returned in text format, the client-end application needs to convert the data back to a binary format for processing. **FETCH** implements conversion between binary data and text data.
-  Use a binary cursor unless necessary, since a text cursor occupies larger storage space than a binary cursor. A binary cursor returns internal binary data, which is easier to operate. To return data in text format, it is advisable to retrieve data in text format, therefore reducing workload at the client end. For example, the value 1 in an integer column of a query is returned as a character string 1 if a default cursor is used, but is returned as a 4-byte binary value (big-endian) if a binary cursor is used.

Syntax
------

::

   CURSOR cursor_name
       [ BINARY ]  [ NO SCROLL ]  [ { WITH | WITHOUT } HOLD ]
       FOR query;

Parameter Description
---------------------

-  **cursor_name**

   Specifies the name of a cursor to be created.

   Value range: Its value must comply with the database naming convention.

-  **BINARY**

   Specifies that data retrieved by the cursor will be returned in binary format, not in text format.

-  **NO SCROLL**

   Specifies the mode of data retrieval by the cursor.

   -  NO SCROLL: If **NO SCROLL** is specified, backward fetches will be rejected.
   -  Not stated: The system automatically determines whether the cursor can be used for backward fetches based on the execution plan.

-  **WITH HOLD \| WITHOUT HOLD**

   Specifies whether the cursor can still be used after the cursor creation event.

   -  **WITH HOLD** indicates that the cursor can still be used.
   -  **WITHOUT HOLD** indicates that the cursor cannot be used.
   -  If neither **WITH HOLD** nor **WITHOUT HOLD** is specified, the default value is **WITHOUT HOLD**.

-  **query**

   The **SELECT** or **VALUES** clause specifies the row to return the cursor value.

   Value range: **SELECT** or **VALUES** clause

Examples
--------

Set up the cursor1 cursor.

::

   CURSOR cursor1 FOR SELECT * FROM tpcds.customer_address ORDER BY 1;

Set up the cursor cursor2.

::

   CURSOR cursor2 FOR VALUES(1,2),(0,3) ORDER BY 1;

An example of using the WITH HOLD cursor is as follows:

Start a transaction.

::

   START TRANSACTION;

Set up a WITH HOLD cursor.

::

   DECLARE cursor3 CURSOR WITH HOLD FOR SELECT * FROM tpcds.customer_address ORDER BY 1;

Fetch the first two rows from cursor3.

::

   FETCH FORWARD 2 FROM cursor3;
    ca_address_sk |  ca_address_id   | ca_street_number |   ca_street_name   | ca_street_type  | ca_suite_number |     ca_city     |    ca_county    | ca_state |   ca_zip   |  ca_country   | ca_gmt_offset |   ca_location_type
   ---------------+------------------+------------------+--------------------+-----------------+-----------------+-----------------+-----------------+----------+------------+---------------+---------------+----------------------
                1 | AAAAAAAABAAAAAAA | 18               | Jackson            | Parkway         | Suite 280       | Fairfield       | Maricopa County | AZ       | 86192      | United States |         -7.00 | condo
                2 | AAAAAAAACAAAAAAA | 362              | Washington 6th     | RD              | Suite 80        | Fairview        | Taos County     | NM       | 85709      | United States |         -7.00 | condo
   (2 rows)

End the transaction.

::

   END;

Fetch the next row from cursor3.

::

   FETCH FORWARD 1 FROM cursor3;
    ca_address_sk |  ca_address_id   | ca_street_number |   ca_street_name   | ca_street_type  | ca_suite_number |     ca_city     |    ca_county    | ca_state |   ca_zip   |  ca_country   | ca_gmt_offset |   ca_location_type
   ---------------+------------------+------------------+--------------------+-----------------+-----------------+-----------------+-----------------+----------+------------+---------------+---------------+----------------------
                3 | AAAAAAAADAAAAAAA | 585              | Dogwood Washington | Circle          | Suite Q         | Pleasant Valley | York County     | PA       | 12477      | United States |         -5.00 | single family
   (1 row)

Close a cursor.

::

   CLOSE cursor3;

Helpful Links
-------------

:ref:`FETCH <dws_06_0216>`
