:original_name: dws_04_0622.html

.. _dws_04_0622:

PG_TABLESPACE
=============

**PG_TABLESPACE** records tablespace information.

.. table:: **Table 1** PG_TABLESPACE columns

   +------------+-----------+----------------------------------------------------------------+
   | Column     | Type      | Description                                                    |
   +============+===========+================================================================+
   | spcname    | Name      | Name of the tablespace.                                        |
   +------------+-----------+----------------------------------------------------------------+
   | spcowner   | OID       | Owner of the tablespace, usually the user who created it.      |
   +------------+-----------+----------------------------------------------------------------+
   | spcacl     | aclitem[] | Access permissions. For details, see **GRANT** and **REVOKE**. |
   +------------+-----------+----------------------------------------------------------------+
   | spcoptions | Text[]    | Options of the tablespace.                                     |
   +------------+-----------+----------------------------------------------------------------+
   | spcmaxsize | Text      | Maximum size of the available disk space, in bytes.            |
   +------------+-----------+----------------------------------------------------------------+
