:original_name: dws_04_0598.html

.. _dws_04_0598:

PG_LARGEOBJECT
==============

**PG_LARGEOBJECT** records the data making up large objects A large object is identified by an OID assigned when it is created. Each large object is broken into segments or "pages" small enough to be conveniently stored as rows in **pg_largeobject**. The amount of data per page is defined to be LOBLKSIZE (which is currently BLCKSZ/4, or typically 2 kB).

It is accessible only to users with system administrator rights.

.. table:: **Table 1** PG_LARGEOBJECT columns

   +--------+---------+--------------------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | Name   | Type    | Reference                                        | Description                                                                                                 |
   +========+=========+==================================================+=============================================================================================================+
   | loid   | oid     | :ref:`PG_LARGEOBJECT_METADATA <dws_04_0599>`.oid | Identifier of the large object that includes this page                                                      |
   +--------+---------+--------------------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | pageno | integer | ``-``                                            | Page number of this page within its large object (counting from zero)                                       |
   +--------+---------+--------------------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | data   | bytea   | ``-``                                            | Actual data stored in the large object. This will never be more than **LOBLKSIZE** bytes and might be less. |
   +--------+---------+--------------------------------------------------+-------------------------------------------------------------------------------------------------------------+

Each row of **pg_largeobject** holds data for one page of a large object, beginning at byte offset (**pageno \* LOBLKSIZE**) within the object. The implementation allows sparse storage: pages might be missing, and might be shorter than **LOBLKSIZE** bytes even if they are not the last page of the object. Missing regions within a large object are read as zeroes.
