:original_name: dws_04_0054.html

.. _dws_04_0054:

Default Permission Mechanism
============================

A user who creates an object is the owner of this object. By default, :ref:`Separation of Permissions <dws_04_0056>` is disabled after cluster installation. A database system administrator has the same permissions as object owners. After an object is created, only the object owner or system administrator can query, modify, and delete the object, and grant permissions for the object to other users through **GRANT** by default.

To enable another user to use the object, grant required permissions to the user or the role that contains the user.

GaussDB(DWS) supports the following permissions: **SELECT**, **INSERT**, **UPDATE**, **DELETE**, **TRUNCATE**, **REFERENCES**, **CREATE**, **CONNECT**, **EXECUTE**, **USAGE** and **ANALYZE**\ \|\ **ANALYSE**. Permission types are associated with object types. For permission details, see GRANT.

To remove permissions, use **REVOKE**. Object owner permissions such as **ALTER**, **DROP**, **GRANT**, and **REVOKE** are implicit and cannot be granted or revoked. That is, you have the implicit permissions for an object if you are the owner of the object. Object owners can remove their own common permissions, for example, making tables read-only to themselves or others.

System catalogs and views are visible to either system administrators or all users. System catalogs and views that require system administrator permissions can be queried only by system administrators. For details, see :ref:`System Catalogs and System Views <dws_04_0559>`.

The database provides the object isolation feature. If this feature is enabled, users can view only the objects (tables, views, columns, and functions) that they have the permission to access. System administrators are not affected by this feature. For details, see ALTER DATABASE.
