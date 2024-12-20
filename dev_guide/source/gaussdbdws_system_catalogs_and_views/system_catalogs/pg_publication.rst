:original_name: dws_04_1040.html

.. _dws_04_1040:

PG_PUBLICATION
==============

**PG_PUBLICATION** records all the publications created in the current database. This system catalog is supported only by clusters of version 8.2.0.100 or later.

.. table:: **Table 1** PG_PUBLICATION columns

   +--------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | Name         | Type    | Reference                          | Description                                                                                                                                 |
   +==============+=========+====================================+=============================================================================================================================================+
   | oid          | oid     | ``-``                              | Row identifier (hidden attribute; displayed only when explicitly selected)                                                                  |
   +--------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | pubname      | name    | ``-``                              | Publication name                                                                                                                            |
   +--------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | pubowner     | oid     | :ref:`PG_AUTHID <dws_04_0574>`.oid | Publication owner                                                                                                                           |
   +--------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | puballtables | boolean | ``-``                              | If its value is **true**, the publication includes all the tables in the database, including any tables that will be created in the future. |
   +--------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | pubinsert    | boolean | ``-``                              | If its value is **true**, the INSERT operation is copied for the tables in the publication.                                                 |
   +--------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | pubupdate    | boolean | ``-``                              | If its value is **true**, the UPDATE operation is copied for the tables in the publication.                                                 |
   +--------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | pubdelete    | boolean | ``-``                              | If its value is **true**, the DELETE operation is copied for the tables in the publication.                                                 |
   +--------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
   | pubtruncate  | boolean | ``-``                              | If its value is **true**, the TRUNCATE operation is copied for the tables in the publication.                                               |
   +--------------+---------+------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

View all releases.

::

   SELECT * FROM pg_publication;
    pubname | pubowner | puballtables | pubinsert | pubupdate | pubdelete | pubtruncate
   ---------+----------+--------------+-----------+-----------+-----------+-------------
    mypub   |       10 | t            | t         | t         | t         | t
   (1 row)
