:original_name: dws_06_0079.html

.. _dws_06_0079:

Value Storage
=============

**Value Storage Type Resolution**
---------------------------------

#. Search for an exact match with the target column.
#. Try to convert the expression to the target type. This will succeed if there is a registered cast between the two types. If the expression is an unknown-type literal, the content of the literal string will be fed to the input conversion routine for the target type.
#. Check whether there is a sizing cast for the target type. A sizing cast is a cast from that type to itself. If one is found in the **pg_cast** catalog, apply it to the expression before storing into the destination column. The implementation function for such a cast always takes an extra parameter of type **integer**. The parameter receives the destination column's **atttypmod** value (typically its declared length, although the interpretation of **atttypmod** varies for different data types), and may take a third boolean parameter that says whether the cast is explicit or implicit. The cast function is responsible for applying any length-dependent semantics such as size checking or truncation.

Examples
--------

Use the **character** storage type conversion as an example. For a target column declared as **character(20)** the following statement shows that the stored value is sized correctly:

::

   CREATE TABLE x1
   (
       customer_sk             integer,
       customer_id             char(20),
       first_name              char(6),
       last_name               char(8)
   )
   with (orientation = column,compression=middle)
   distribute by hash (last_name);

   INSERT INTO x1(customer_sk, customer_id, first_name) VALUES (3769, 'abcdef', 'Grace');

   SELECT customer_id, octet_length(customer_id) FROM x1;
        customer_id      | octet_length
   ----------------------+--------------
    abcdef               |           20
   (1 row)

.. note::

   What has really happened here is that the two unknown literals are resolved to **text** by default, allowing the **\|\|** operator to be resolved as **text** concatenation. Then the **text** result of the operator is converted to **bpchar** ("blank-padded char", the internal name of the **character** data type) to match the target column type. Since the conversion from **text** to **bpchar** is binary-coercible, this conversion does not insert any real function call. Finally, the sizing function **bpchar(bpchar, integer, boolean)** is found in the system catalog and used for the operator's result and the stored column length. This type-specific function performs the required length check and addition of padding spaces.
