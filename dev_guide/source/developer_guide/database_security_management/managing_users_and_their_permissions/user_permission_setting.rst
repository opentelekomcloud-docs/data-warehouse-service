:original_name: dws_04_0060.html

.. _dws_04_0060:

User Permission Setting
=======================

-  To grant the permission for an object directly to a user, use **GRANT**.

   When permissions for a table or view in a schema are granted to a user or role, the **USAGE** permission of the schema must be granted together. Otherwise, the user or role can only see the names of the objects but cannot actually access them.

   In the following example, permissions for the schema **tpcds** are first granted to the user **joe**, and then the **SELECT** permission for the **tpcds.web_returns** table is also granted.

   ::

      GRANT USAGE ON SCHEMA tpcds TO joe;
      GRANT SELECT ON TABLE tpcds.web_returns to joe;

-  Granting a role to a user allows the user to inherit the object permissions of the role.

   #. Create a role.

      Create a role **lily** and grant the system permission **CREATEDB** to the role.

      ::

         CREATE ROLE lily WITH CREATEDB PASSWORD 'password';

   #. To grant object permissions to a role, use **GRANT**.

      For example, first grant permissions for the schema **tpcds** to the role **lily**, and then grant the **SELECT** permission of the **tpcds.web_returns** table to **lily**.

      ::

         GRANT USAGE ON SCHEMA tpcds TO lily;
         GRANT SELECT ON TABLE tpcds.web_returns to lily;

   #. Grant the role permissions to a user.

      ::

         GRANT lily to joe;

      .. note::

         When the permissions of a role are granted to a user, the attributes of the role are not transferred together.

-  To revoke user permissions, use **REVOKE**.
