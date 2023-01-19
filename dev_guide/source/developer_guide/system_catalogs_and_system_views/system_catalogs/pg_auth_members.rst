:original_name: dws_04_0576.html

.. _dws_04_0576:

PG_AUTH_MEMBERS
===============

**PG_AUTH_MEMBERS** records the membership relations between roles.

.. table:: **Table 1** PG_AUTH_MEMBERS columns

   +--------------+---------+-----------------------------------------------------------+
   | Name         | Type    | Description                                               |
   +==============+=========+===========================================================+
   | roleid       | oid     | ID of a role that has a member                            |
   +--------------+---------+-----------------------------------------------------------+
   | member       | oid     | ID of a role that is a member of ROLEID                   |
   +--------------+---------+-----------------------------------------------------------+
   | grantor      | oid     | ID of a role that grants this membership                  |
   +--------------+---------+-----------------------------------------------------------+
   | admin_option | boolean | Whether a member can grant membership in ROLEID to others |
   +--------------+---------+-----------------------------------------------------------+
