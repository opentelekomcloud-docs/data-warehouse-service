:original_name: dws_04_0586.html

.. _dws_04_0586:

PG_DESCRIPTION
==============

**PG_DESCRIPTION** records optional descriptions (comments) for each database object. Descriptions of many built-in system objects are provided in the initial contents of **PG_DESCRIPTION**.

See also :ref:`PG_SHDESCRIPTION <dws_04_0617>`, which performs a similar function for descriptions involving objects that are shared across a database cluster.

.. table:: **Table 1** PG_DESCRIPTION columns

   +-------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Name        | Type    | Reference                          | Description                                                                                                                                                               |
   +=============+=========+====================================+===========================================================================================================================================================================+
   | objoid      | oid     | Any OID column                     | OID of the object this description pertains to                                                                                                                            |
   +-------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | classoid    | oid     | :ref:`PG_CLASS <dws_04_0578>`\ oid | OID of the system catalog this object appears in                                                                                                                          |
   +-------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | objsubid    | integer | ``-``                              | For a comment on a table column, this is the column number (the **objoid** and **classoid** refer to the table itself). For all other object types, this column is **0**. |
   +-------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | description | text    | ``-``                              | Arbitrary text that serves as the description of this object                                                                                                              |
   +-------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
