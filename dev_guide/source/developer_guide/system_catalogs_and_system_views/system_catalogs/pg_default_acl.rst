:original_name: dws_04_0584.html

.. _dws_04_0584:

PG_DEFAULT_ACL
==============

**PG_DEFAULT_ACL** records the initial privileges assigned to the newly created objects.

.. table:: **Table 1** PG_DEFAULT_ACL columns

   +-----------------------+-----------------------+-----------------------------------------------------------------------+
   | Name                  | Type                  | Description                                                           |
   +=======================+=======================+=======================================================================+
   | defaclrole            | oid                   | ID of the role associated with the permission                         |
   +-----------------------+-----------------------+-----------------------------------------------------------------------+
   | defaclnamespace       | oid                   | Namespace associated with the permission; the value is **0** if no ID |
   +-----------------------+-----------------------+-----------------------------------------------------------------------+
   | defaclobjtype         | "char"                | Object type of the permission:                                        |
   |                       |                       |                                                                       |
   |                       |                       | -  **r** indicates a table or view.                                   |
   |                       |                       | -  **S** indicates a sequence.                                        |
   |                       |                       | -  **f** indicates a function.                                        |
   |                       |                       | -  **T** indicates a type.                                            |
   +-----------------------+-----------------------+-----------------------------------------------------------------------+
   | defaclacl             | aclitem[]             | Access permissions that this type of object should have on creation   |
   +-----------------------+-----------------------+-----------------------------------------------------------------------+

Example
-------

Run the following command to view the initial permissions of the new user **role1**:

::

   select * from PG_DEFAULT_ACL;
    defaclrole | defaclnamespace | defaclobjtype |    defaclacl
   ------------+-----------------+---------------+-----------------
         16820 |           16822 | r             | {role1=r/user1}

You can also run the following statement to convert the format:

::

   SELECT pg_catalog.pg_get_userbyid(d.defaclrole) AS "Granter",  n.nspname AS "Schema",  CASE d.defaclobjtype WHEN 'r' THEN 'table' WHEN 'S' THEN 'sequence' WHEN 'f' THEN 'function' WHEN 'T' THEN 'type' END AS "Type",  pg_catalog.array_to_string(d.defaclacl, E', ') AS "Access privileges" FROM pg_catalog.pg_default_acl d LEFT JOIN pg_catalog.pg_namespace n ON n.oid = d.defaclnamespace ORDER BY 1, 2, 3;

If the following information is displayed, **user1** grants **role1** the read permission on schema **user1**.

::

    Granter | Schema | Type  | Access privileges
   ---------+--------+-------+-------------------
    user1   | user1  | table | role1=r/user1
   (1 row)
