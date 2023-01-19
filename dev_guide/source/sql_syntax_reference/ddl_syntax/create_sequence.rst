:original_name: dws_06_0174.html

.. _dws_06_0174:

CREATE SEQUENCE
===============

Function
--------

**CREATE SEQUENCE** adds a sequence to the current database. The owner of a sequence is the user who creates the sequence.

Precautions
-----------

-  A sequence is a special table that stores arithmetic sequence. Such a table is controlled by DBMS. It has no actual meaning and is usually used to generate unique identifiers for rows or tables.
-  If a schema name is given, the sequence is created in the specified schema; otherwise, it is created in the current schema. The sequence name must be different from the names of other sequences, tables, indexes, views in the same schema.
-  After the sequence is created, functions **NEXTVAL()** and **generate_series(1,N)** insert data to the table. Make sure that the number of times for invoking **NEXTVAL** is greater than or equal to N+1. Otherwise, errors will be reported because the number of times for invoking the function **generate_series()** is N+1.
-  A sequence cannot be created in the **template1** database.

Syntax
------

::

   CREATE SEQUENCE name [ INCREMENT [ BY ] increment ]
       [ MINVALUE minvalue | NO MINVALUE | NOMINVALUE ] [ MAXVALUE maxvalue | NO MAXVALUE | NOMAXVALUE]
       [ START [ WITH ] start ] [ CACHE cache ] [ [ NO ] CYCLE | NOCYCLE ]
       [ OWNED BY { table_name.column_name | NONE } ];

Parameter Description
---------------------

-  **name**

   Specifies the name of a sequence to be created.

   Value range: The value can contain only lowercase letters, uppercase letters, special characters #_$, and digits.

-  **increment**

   Specifies the step for a sequence. A positive generates an ascending sequence, and a negative generates a decreasing sequence.

   The default value is **1**.

-  **MINVALUE minvalue \| NO MINVALUE\| NOMINVALUE**

   Specifies the minimum value of a sequence. If **MINVALUE** is not declared, or **NO MINVALUE** is declared, the default value of the ascending sequence is **1**, and that of the descending sequence is **-2\ 63-1**. **NOMINVALUE** is equivalent to **NO MINVALUE**.

-  **MAXVALUE maxvalue \| NO MAXVALUE\| NOMAXVALUE**

   Specifies the maximum value of a sequence. If **MAXVALUE** is not declared or **NO MAXVALUE** is declared, the default value of the ascending sequence is **2\ 63-1**, and that of the descending sequence is **-1**. **NOMAXVALUE** is equivalent to **NO MAXVALUE**.

-  **start**

   Specifies the start value of the sequence. The default value for ascending sequences is **minvalue** and for descending sequences **maxvalue**.

-  **cache**

   Specifies the number of sequence numbers stored in the memory for quick access. Within a cache period, the CN does not request a sequence number from the GTM. Instead, the CN uses the sequence number that is locally applied for in advance.

   Default value **1** indicates that one value can be generated each time.

   .. note::

      -  It is not recommended that **cache** and **maxvalue** or **minvalue** be defined at the same time. The continuity of sequences cannot be ensured after **cache** is defined because unacknowledged sequences may be generated, causing waste of sequence number segments.
      -  You are advised not to set a large value for **cache** (less than 100000000). Otherwise, it takes a long time to cache the sequence number (the first **NEXTVAL** in each cache period). Set a proper value for **cache** based on services to ensure quick access without wasting sequence numbers.

-  **CYCLE**

   Used to ensure that sequences can recycle after the number of sequences reaches **maxvalue** or **minvalue**.

   If you declare **NO CYCLE**, any invocation of **nextval** would return an error after the sequence reaches its maximum value.

   **NOCYCLE** is equivalent to **NO CYCLE**.

   The default value is **NO CYCLE**.

   If the sequence is defined as **CYCLE**, the sequence uniqueness cannot be ensured.

-  **OWNED BY**\ ``-``

   Associates a sequence with a specified column included in a table. In this way, the sequence will be deleted when you delete its associated field or the table where the field belongs. The associated table and sequence must be owned by the same user and in the same schema. **OWNED BY** only establishes the association between a table column and the sequence. The sequence is not created for this column.

   If the default value is **OWNED BY NONE**, indicating that such association does not exist.

   .. important::

      You are not advised to use the sequence created using **OWNED BY** in other tables. If multiple tables need to share a sequence, the sequence must not belong to a specific table.

Examples
--------

Create an ascending sequence named **serial**, which starts from 101:

::

   CREATE SEQUENCE serial
    START 101
    CACHE 20;

Select the next number from the sequence:

::

   SELECT nextval('serial');
    nextval
    ---------
         101

Select the next number from the sequence:

::

   SELECT nextval('serial');
    nextval
    ---------
         102

Create a sequence associated with the table:

::

   CREATE TABLE customer_address
   (
       ca_address_sk             integer               not null,
       ca_address_id             char(16)              not null,
       ca_street_number          char(10)                      ,
       ca_street_name            varchar(60)                   ,
       ca_street_type            char(15)                      ,
       ca_suite_number           char(10)                      ,
       ca_city                   varchar(60)                   ,
       ca_county                 varchar(30)                   ,
       ca_state                  char(2)                       ,
       ca_zip                    char(10)                      ,
       ca_country                varchar(20)                   ,
       ca_gmt_offset             decimal(5,2)                  ,
       ca_location_type          char(20)
   ) ;

   CREATE SEQUENCE serial1
    START 101
    CACHE 20
   OWNED BY customer_address.ca_address_sk;

Use SERIAL to create a serial table **serial_table** for primary key auto-increment.

::

   CREATE TABLE serial_table(a int, b serial);
   INSERT INTO serial_table (a) VALUES (1),(2),(3);
   SELECT * FROM serial_table ORDER BY b;
    a | b
   ---+---
    1 | 1
    2 | 2
    3 | 3
   (3 rows)

Helpful Links
-------------

:ref:`DROP SEQUENCE <dws_06_0205>` :ref:`ALTER SEQUENCE <dws_06_0137>`
