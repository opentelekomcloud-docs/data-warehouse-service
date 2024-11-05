:original_name: dws_04_0622.html

.. _dws_04_0622:

PG_TABLESPACE
=============

**PG_TABLESPACE** records tablespace information.

.. table:: **Table 1** PG_TABLESPACE columns

   +------------+-----------+----------------------------------------------------------+
   | Name       | Type      | Description                                              |
   +============+===========+==========================================================+
   | spcname    | name      | Name of the tablespace                                   |
   +------------+-----------+----------------------------------------------------------+
   | spcowner   | oid       | Owner of the tablespace, usually the user who created it |
   +------------+-----------+----------------------------------------------------------+
   | spcacl     | aclitem[] | Access permissions For details, see GRANT and REVOKE.    |
   +------------+-----------+----------------------------------------------------------+
   | spcoptions | text[]    | Specifies options of the tablespace.                     |
   +------------+-----------+----------------------------------------------------------+
   | spcmaxsize | text      | Maximum size of the available disk space, in bytes       |
   +------------+-----------+----------------------------------------------------------+
