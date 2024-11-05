:original_name: dws_04_0078.html

.. _dws_04_0078:

GaussDB(DWS) Database and Schema Design Rules
=============================================

In GaussDB(DWS), services can be isolated by databases and schemas. Databases share little resources and cannot directly access each other. Connections to and permissions on them are also isolated. Schemas share more resources than databases do. User permissions on schemas and subordinate objects can be controlled using the **GRANT** and **REVOKE** syntax.

-  You are advised to use schemas to isolate services for convenience and resource sharing.
-  It is recommended that system administrators create schemas and databases and then assign required permissions to users.

Database Design Suggestions
---------------------------

-  Create databases as required. Do not use the default **gaussdb** database of a cluster.
-  Create a maximum of three user-defined databases in a cluster.
-  To make your database encoding compatible with most characters, you are advised to use the UTF-8 encoding when creating a database.
-  Exercise caution when you set **ENCODING** and **DBCOMPATIBILITY** configuration items during database creation. In GaussDB(DWS), **DBCOMPATIBILITY** can be set to **TD**, **Oracle**, or **MySQL** to be compatible with Teradata, Oracle, or MySQL syntax, respectively. Syntax behavior may vary with the three modes. For details, see :ref:`Syntax Compatibility Differences Among Oracle, Teradata, and MySQL <dws_04_0042>`.
-  By default, a database owner has all permissions for all objects in the database, including the deletion permission. Exercise caution when using the deletion permission.

Schema Design Suggestions
-------------------------

-  To let a user access an object in a schema, grant the **usage** permission and the permissions for the object to the user, unless the user has the **sysadmin** permission or is the schema owner.
-  To let a user create an object in the schema, grant the **CREATE** permission for the schema to the user.
-  By default, a schema owner has all permissions for all objects in the schema, including the deletion permission. Exercise caution when using the deletion permission.
