:original_name: dws_06_0371.html

.. _dws_06_0371:

Composite Types
===============

A composite type represents the structure of a row or record, which is essentially a list of field names and their data types. GaussDB(DWS) allows table columns to be declared as composite types. A composite type is essentially the same as the row type of a table. However, using :ref:`CREATE TYPE <dws_06_0185>` avoids the need to create an actual table when only a type needs to be defined. A stand-alone composite type is useful as the parameter or return type of a function.

Declaration of Composite Types
------------------------------

GaussDB (DWS) allows users to use :ref:`CREATE TYPE <dws_06_0185>` to define composite types.

::

   CREATE TYPE complex AS (
           r       double precision,
           i       double precision
       );

   CREATE TYPE inventory_item AS (
           name            text,
           supplier_id     integer,
           price           numeric
       );

After defining a composite type, you can create a table or function.

::

   CREATE TABLE on_hand (
           item      inventory_item,
           count     integer
       );

   INSERT INTO on_hand VALUES (ROW('fuzzy dice', 42, 1.99), 1000);

::

   CREATE FUNCTION price_extension(inventory_item, integer) RETURNS numeric
       AS 'SELECT $1.price * $2' LANGUAGE SQL;

   SELECT price_extension(item, 10) FROM on_hand;

Constructing a Compound Value
-----------------------------

To write a composite value as a literal constant, enclose the field value in parentheses and separate them with commas. You can add double quotation marks to any field value. This is mandatory if the field value contains commas or parentheses. The general format of a compound constant is as follows:

::

   '( val1 , val2 , ... )'

**'("fuzzy dice",42,1.99)'** is a valid value of the **inventory_item** type.

To set a field to NULL, left the position empty. If you want a field to be an empty string, use quotation marks. In the following example, the first column is a non-null empty string, and the third column is NULL.

::

   '("",42,)'

ROW expressions can also be used to build composite values. Example:

::

   ROW('fuzzy dice', 42, 1.99)
   ROW('', 42, NULL)

Access Composite Types
----------------------

To access a field in a composite column, you can write a dot and the name of the field, just like selecting a field from a table. To avoid confusion, you must use parentheses to distinguish it. For example, select some fields from table **on_hand**:

::

   SELECT item.name FROM on_hand WHERE item.price > 9.99;
   ERROR:  missing FROM-clause entry for table "item"

This writing is confused with selecting fields from a table because **item** is regarded as a table name instead of a column name of **on_hand**. It must be written as follows:

::

   SELECT (item).name FROM on_hand WHERE (item).price > 9.99;

If you need to use the table name (for example, in a multi-table query):

::

   SELECT (on_hand.item).name FROM on_hand WHERE (on_hand.item).price > 9.99;

Objects with parentheses are now correctly interpreted as references to column **item**, from which fields can be selected.

Similar syntax applies when a field is selected from a compound value. For example, to select a field from the result of a function that returns a compound value, write as follows:

::

   SELECT (my_func(...)).field FROM ...

If there are no additional parentheses, this generates a syntax error.
