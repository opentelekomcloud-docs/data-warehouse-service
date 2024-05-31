:original_name: dws_06_0137.html

.. _dws_06_0137:

ALTER SEQUENCE
==============

Function
--------

Modifies the sequence definition.

Precautions
-----------

-  You must be the owner of the sequence to use **ALTER SEQUENCE**.
-  In the current version, you can modify only the owner, home column, and the maximum value. To modify other parameters, delete the sequence and create it again. Then, use the Setval function to restore original parameter values.
-  **ALTER SEQUENCE MAXVALUE** cannot be used in transactions, functions, and stored procedures.
-  After the maximum value of a sequence is changed, the cache of the sequence in all sessions is cleared.
-  **ALTER SEQUENCE** blocks the invocation of **nextval**, **setval**, **currval**, and **lastval**.

Syntax
------

Change the maximum value or home column of the sequence.

::

   ALTER SEQUENCE [ IF EXISTS ] name
       [ MAXVALUE maxvalue | NO MAXVALUE | NOMAXVALUE ]
       [ OWNED BY { table_name.column_name | NONE } ] ;

Change the owner of a sequence.

::

   ALTER SEQUENCE [ IF EXISTS ] name OWNER TO new_owner;

Parameter Description
---------------------

-  **name**

   Specifies the sequence name to be changed.

-  **IF EXISTS**

   Sends a notification instead of an error when you are modifying a non-existing sequence.

-  **MAXVALUE maxvalue \| NO MAXVALUE**

   Maximum value of a sequence. If **NO MAXVALUE** is declared, the default value of the ascending sequence is **2\ 63-1**, and that of the descending sequence is **-1**. **NOMAXVALUE** is equivalent to **NO MAXVALUE**.

-  **OWNED BY**

   Associates a sequence with a specified column included in a table. In this way, the sequence will be deleted when you delete its associated field or the table where the field belongs.

   If the sequence has been associated with another table before you use this parameter, the new association will overwrite the old one.

   The associated table and sequence must be owned by the same user and in the same schema.

   If **OWNED BY NONE** is used, existing associations will be deleted.

-  **new_owner**

   Specifies the user name of the new owner. To change the owner, you must also be a direct or indirect member of the new role, and this role must have **CREATE** permission on the sequence's schema.

Examples
--------

Create the example sequence **seq_test** and example table **t1**:

::

   CREATE SEQUENCE seq_test START 101;

::

   DROP TABLE IF EXISTS t1;
   CREATE TABLE t1(c1 bigint default nextval('seq_test'));

Modify the maximum value of **serial** to **200**.

::

   ALTER SEQUENCE seq_test MAXVALUE 200;

Change the column of the **seq_test** sequence to **t1.c1**.

::

   ALTER SEQUENCE seq_test OWNED BY t1.c1;

Helpful Links
-------------

:ref:`CREATE SEQUENCE <dws_06_0174>`, :ref:`DROP SEQUENCE <dws_06_0205>`
