:original_name: dws_04_0731.html

.. _dws_04_0731:

PG_GROUP
========

**PG_GROUP** displays the database role authentication and the relationship between roles.

.. table:: **Table 1** PG_GROUP columns

   ======== ===== ==================================================
   Column   Type  Description
   ======== ===== ==================================================
   groname  Name  Group name
   grosysid OID   Group ID
   grolist  oid[] An array, including all the role IDs in this group
   ======== ===== ==================================================
