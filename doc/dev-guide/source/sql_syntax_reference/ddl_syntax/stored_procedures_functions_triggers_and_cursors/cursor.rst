:original_name: dws_06_0188.html

.. _dws_06_0188:

CURSOR
======

Function
--------

This statement is used to create a cursor and retrieve specified rows of data from a query.

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
       FOR query ;

Parameter Description
---------------------

.. table:: **Table 1** CURSOR parameters

   +---------------------------+-------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | Parameter                 | Description                                                                                                                         | Value Range                                                                                                         |
   +===========================+=====================================================================================================================================+=====================================================================================================================+
   | cursor_name               | Specifies the name of the cursor to be created.                                                                                     | A string, which must comply with the :ref:`naming convention <en-us_topic_0000001811634529__section1475018612353>`. |
   +---------------------------+-------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | BINARY                    | Specifies that data retrieved by the cursor will be returned in binary format, not in text format.                                  | ``-``                                                                                                               |
   +---------------------------+-------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | NO SCROLL                 | Specifies the mode of data retrieval by the cursor.                                                                                 | ``-``                                                                                                               |
   |                           |                                                                                                                                     |                                                                                                                     |
   |                           | -  NO SCROLL: If **NO SCROLL** is specified, backward fetches will be rejected.                                                     |                                                                                                                     |
   |                           | -  Not stated: The system automatically determines whether the cursor can be used for backward fetches based on the execution plan. |                                                                                                                     |
   +---------------------------+-------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | WITH HOLD \| WITHOUT HOLD | Specifies whether the cursor can still be used after the cursor creation event.                                                     | ``-``                                                                                                               |
   |                           |                                                                                                                                     |                                                                                                                     |
   |                           | -  **WITH HOLD** indicates that the cursor can still be used.                                                                       |                                                                                                                     |
   |                           | -  **WITHOUT HOLD** indicates that the cursor cannot be used. (default setting)                                                     |                                                                                                                     |
   +---------------------------+-------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | query                     | The **SELECT** or **VALUES** clause specifies the row to return the cursor value.                                                   | Use the **SELECT** or **VALUES** clause.                                                                            |
   +---------------------------+-------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+

Examples
--------

Prepare table data.

::

   DROP TABLE IF EXISTS customer_address;
   CREATE TABLE customer_address
   (ca_address_sk INT,
   ca_address_id VARCHAR(16),
   ca_street_number VARCHAR(10),
   ca_street_name VARCHAR(60),
   ca_street_type VARCHAR(15),
   ca_suite_number VARCHAR(10));

   INSERT INTO customer_address VALUES
   (1, 'ID1', '100', 'Main', 'St', 'A1'),
   (2, 'ID2', '200', 'Oak', 'Ave', 'B2'),
   (3, 'ID3', '300', 'Pine', 'Blvd', 'C3');

Start a transaction.

::

   START TRANSACTION;

Create a cursor named **cursor1**:

::

   CURSOR cursor1 FOR SELECT * FROM customer_address ORDER BY 1;

create a cursor named **cursor2**:

::

   CURSOR cursor2 FOR VALUES(1,2),(0,3) ORDER BY 1;

Usage of the **WITH HOLD** cursor:

#. Set up the WITH HOLD cursor.

   ::

      DECLARE cursor3 CURSOR WITH HOLD FOR SELECT * FROM customer_address ORDER BY 1;

#. Fetch the first two rows from cursor3.

   ::

      FETCH FORWARD 2 FROM cursor3;

   |image1|

#. End the transaction.

   ::

      END;

#. Fetch the next row from cursor3.

   ::

      FETCH FORWARD 1 FROM cursor3;

   |image2|

#. Close the cursor.

   ::

      CLOSE cursor3;

Helpful Links
-------------

:ref:`FETCH <dws_06_0216>`

.. |image1| image:: /_static/images/en-us_image_0000002660734375.png
.. |image2| image:: /_static/images/en-us_image_0000002630495202.png
