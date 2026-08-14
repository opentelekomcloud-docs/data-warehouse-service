:original_name: dws_06_0222.html

.. _dws_06_0222:

SET ROLE
========

Function
--------

**SET ROLE** sets the current user identifier for the current session, that is, switches to or enables a specified role in the current database session.

Precautions
-----------

-  Users of the current session must be the role members specified by **role_name**, but system administrators can switch to any roles.
-  You can use SET ROLE to add permissions or restrict the permissions of a user. If a role of a session user has the **INHERITS** attribute, it automatically has all permissions of roles that **SET ROLE** enables the role to be. In this case, **SET ROLE** physically deletes all permissions directly granted to session users and permissions of its belonging roles and only leaves permissions of the specified roles. If a role of a session user has the **NOINHERITS** attribute, **SET ROLE** deletes permissions directly granted to the session user and obtains permissions of the specified role.

Syntax
------

-  Switch or enable a specified role in the current database session.

   ::

      SET [ SESSION | LOCAL ] ROLE role_name PASSWORD '{password}';

-  Reset the current session role.

   ::

      RESET ROLE;

Parameter Description
---------------------

.. table:: **Table 1** SET ROLE parameters

   +------------+-------------------------------------------------------------------------------------+---------------------------+
   | Parameter  | Description                                                                         | Value Range               |
   +============+=====================================================================================+===========================+
   | SESSION    | Specifies that the command takes effect only for the current session (default).     | ``-``                     |
   +------------+-------------------------------------------------------------------------------------+---------------------------+
   | LOCALE     | Specifies that the specified command takes effect only for the current transaction. | ``-``                     |
   +------------+-------------------------------------------------------------------------------------+---------------------------+
   | role_name  | Specifies the role name.                                                            | Name of an existing role. |
   +------------+-------------------------------------------------------------------------------------+---------------------------+
   | password   | Specifies the password of a role.                                                   | ``-``                     |
   +------------+-------------------------------------------------------------------------------------+---------------------------+
   | RESET ROLE | Resets the current user identifier.                                                 | ``-``                     |
   +------------+-------------------------------------------------------------------------------------+---------------------------+

Examples
--------

View the current session user.

::

   SELECT CURRENT_USER;

.. code-block::

    current_user
   --------------
    dbadmin
   (1 row)

Create a role named **role_new** and switch the current session user to **role_new**.

::

   CREATE ROLE role_new IDENTIFIED BY '{Password}';
   SET ROLE role_new PASSWORD '{password}';

Check the current session user. The user is displayed as **role_new**.

::

   SELECT CURRENT_USER;

::

    current_user
   --------------
    role_new
   (1 row)

Reset the current user to the original session user.

::

   RESET role;
