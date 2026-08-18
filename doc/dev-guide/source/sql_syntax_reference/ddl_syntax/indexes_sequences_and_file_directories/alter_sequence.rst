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
-  To change the owner, you must also be a direct or indirect member of the new role, and this role must have the **CREATE** permission on the sequence's schema.
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
       [ CACHE cache ]
       [ OWNED BY { table_name.column_name | NONE } ] ;

Change the owner of the sequence.

::

   ALTER SEQUENCE [ IF EXISTS ] name OWNER TO new_owner;

Parameter Description
---------------------

.. table:: **Table 1** ALTER SEQUENCE parameters

   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | Parameter                        | Description                                                                                                                                                                                                                                                                                         | Value Range                                                                                                   |
   +==================================+=====================================================================================================================================================================================================================================================================================================+===============================================================================================================+
   | IF EXISTS                        | Sends a message instead of an error if the sequence does not exist.                                                                                                                                                                                                                                 | ``-``                                                                                                         |
   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | name                             | Specifies the name of the sequence to be modified.                                                                                                                                                                                                                                                  | Name of an existing sequence.                                                                                 |
   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | MAXVALUE maxvalue \| NO MAXVALUE | Specifies the maximum value of a sequence. If **NO MAXVALUE** is specified, the default value of the ascending sequence is **2\ 63-1**, and that of the descending sequence is **-1**. **NOMAXVALUE** is equivalent to **NO MAXVALUE**.                                                             | ``-``                                                                                                         |
   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | CACHE cache                      | Allocates sequence numbers in advance and stores them in memory to speed up access. This parameter is supported only by clusters of version 8.2.1.100 or later.                                                                                                                                     | -  The minimum value of **cachevalue** is **1**. (One value can be generated at a time, that is **NOCACHE**.) |
   |                                  |                                                                                                                                                                                                                                                                                                     | -  If this parameter is not specified, the old cache value is retained.                                       |
   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | OWNED BY                         | Associates a sequence with a specified field in a table. In this way, the sequence will be deleted when you delete its associated field or the table where the field belongs.                                                                                                                       | ``-``                                                                                                         |
   |                                  |                                                                                                                                                                                                                                                                                                     |                                                                                                               |
   |                                  | If the sequence has been associated with another table before you use this parameter, the new association will overwrite the old one. The associated table and sequence must be owned by the same user and in the same schema. If **OWNED BY NONE** is used, existing associations will be deleted. |                                                                                                               |
   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------+
   | new_owner                        | Specifies the username of the new owner.                                                                                                                                                                                                                                                            | Name of an existing user.                                                                                     |
   +----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------+

Examples
--------

Create the sample sequence **seq_test** and sample table **t1**.

::

   CREATE SEQUENCE seq_test START 101;

::

   DROP TABLE IF EXISTS t1;
   CREATE TABLE t1(c1 bigint default nextval('seq_test'));

Modify the maximum value of **serial** to **200**.

::

   ALTER SEQUENCE seq_test MAXVALUE 200;

Change the home column of **seq_test** to **t1.c1**.

::

   ALTER SEQUENCE seq_test OWNED BY t1.c1;

Helpful Links
-------------

:ref:`CREATE SEQUENCE <dws_06_0174>` and :ref:`DROP SEQUENCE <dws_06_0205>`
