:original_name: dws_04_0785.html

.. _dws_04_0785:

PG_TDE_INFO
===========

**PG_TDE_INFO** displays the encryption information about the current cluster.

.. table:: **Table 1** PG_TDE_INFO columns

   +-----------------------+-----------------------+----------------------------------------------+
   | Column                | Type                  | Description                                  |
   +=======================+=======================+==============================================+
   | is_encrypt            | Text                  | Whether the cluster is an encryption cluster |
   |                       |                       |                                              |
   |                       |                       | -  **f**: Non-encryption cluster             |
   |                       |                       | -  **t**: Encryption cluster                 |
   +-----------------------+-----------------------+----------------------------------------------+
   | g_tde_algo            | Text                  | Encryption algorithm                         |
   |                       |                       |                                              |
   |                       |                       | -  AES-CTR-128                               |
   +-----------------------+-----------------------+----------------------------------------------+
   | remain                | Text                  | Reserved columns                             |
   +-----------------------+-----------------------+----------------------------------------------+

Examples
--------

Check whether the current cluster is encrypted, and check the encryption algorithm (if any) used by the current cluster.

::

   SELECT * FROM PG_TDE_INFO;
    is_encrypt | g_tde_algo  | remain
   ------------+-------------+--------
    f          | AES-CTR-128 | remain
   (1 row)
