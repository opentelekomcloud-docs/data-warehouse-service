:original_name: dws_06_0222.html

.. _dws_06_0222:

SET ROLE
========

Function
--------

**SET ROLE** sets the current user identifier of the current session.

Precautions
-----------

-  Users of the current session must be members of specified **rolename**, but the system administrator can choose any roles.
-  Executing this command may add rights of a user or restrict rights of a user. If the role of a session user has the **INHERITS** attribute, it automatically has all rights of roles that **SET ROLE** enables the role to be. In this case, **SET ROLE** physically deletes all rights directly granted to session users and rights of its belonging roles and only leaves rights of the specified roles. If the role of the session user has the **NOINHERITS** attribute, **SET ROLE** deletes rights directly granted to the session user and obtains rights of the specified role.

Syntax
------

-  **SET ROLE** sets the current user identifier of the current session.

   ::

      SET [ SESSION | LOCAL ] ROLE role_name PASSWORD 'password';

-  Reset the current user identifier to that of the current session.

   ::

      RESET ROLE;

Parameter Description
---------------------

-  **SESSION**

   Specifies that the command takes effect only for the current session. This parameter is used by default.

   Value range: A string. It must comply with the naming convention rule.

-  **LOCALE**

   Indicates that the specified command takes effect only for the current transaction.

-  **role_name**

   Specifies the role name.

   Value range: A string. It must comply with the naming convention rule.

-  **password**

   Specifies the password of a role. It must comply with the password convention.

-  **RESET ROLE**

   Resets the current user identifier.

Examples
--------

Set the current user to **paul**.

::

   SET ROLE paul PASSWORD '{password}';

View the current session user and the current user.

::

   SELECT SESSION_USER, CURRENT_USER;

Reset the current user.

::

   RESET role;
