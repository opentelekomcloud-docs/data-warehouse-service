:original_name: dws_06_0217.html

.. _dws_06_0217:

MOVE
====

Function
--------

Repositions a cursor without retrieving any data. **MOVE** works exactly like the :ref:`FETCH <dws_06_0216>` command, except it only repositions the cursor and does not return rows.

Precautions
-----------

None

Syntax
------

::

   MOVE [ direction [ FROM | IN ] ] cursor_name;

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

Parameter Description
---------------------

**MOVE** command parameters are the same as **FETCH** command parameters. For details, see :ref:`Parameter Description <en-us_topic_0000001460880956__s680662240a104ac7a51873c7c888bdd1>` in **FETCH**.

.. note::

   On successful completion, a **MOVE** command returns a command tag of the form **MOVE count**. The **count** is the number of rows that a **FETCH** command with the same parameters would have returned (possibly zero).

Examples
--------

Start a transaction:

::

   START TRANSACTION;

Define the **cursor1** cursor:

::

   CURSOR cursor1 FOR SELECT * FROM tpcds.reason;

Skip the first three rows of **cursor1**:

::

   MOVE FORWARD 3 FROM cursor1;

Fetch the first four rows from **cursor1**:

::

   FETCH 4 FROM cursor1;
    r_reason_sk |   r_reason_id    |                                            r_reason_desc
   -------------+------------------+------------------------------------------------------------------------------------------------------
              4 | AAAAAAAAEAAAAAAA | Not the product that was ordred
              5 | AAAAAAAAFAAAAAAA | Parts missing
              6 | AAAAAAAAGAAAAAAA | Does not work with a product that I have
              7 | AAAAAAAAHAAAAAAA | Gift exchange
   (4 rows)

Close a cursor:

::

   CLOSE cursor1;

End the transaction:

::

   END;

Helpful Links
-------------

:ref:`CLOSE <dws_06_0152>`, :ref:`FETCH <dws_06_0216>`
