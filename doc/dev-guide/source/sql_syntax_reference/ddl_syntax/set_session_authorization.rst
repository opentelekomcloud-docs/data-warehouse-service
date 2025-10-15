:original_name: dws_06_0223.html

.. _dws_06_0223:

SET SESSION AUTHORIZATION
=========================

Function
--------

**SET SESSION AUTHORIZATION** sets the session user identifier and the current user identifier of the current SQL session to a specified user.

Precautions
-----------

The session identifier can be changed only when the initial session user has the system administrator rights. Otherwise, the system supports the command only when the authenticated user name is specified.

Syntax
------

-  **SET SESSION AUTHORIZATION** sets the session user identifier and the current user identifier of the current session.

   ::

      SET [ SESSION | LOCAL ] SESSION AUTHORIZATION role_name PASSWORD '{password}';

-  Reset the identifiers of the session and current users to the initially authenticated user names.

   ::

      {SET [ SESSION | LOCAL ] SESSION AUTHORIZATION DEFAULT
          | RESET SESSION AUTHORIZATION};

Parameter Description
---------------------

-  **SESSION**

   Indicates that the specified parameters take effect for the current session.

   Value range: A string. It must comply with the naming convention.

-  **LOCALE**

   Indicates that the specified command takes effect only for the current transaction.

-  **role_name**

   User name.

   Value range: A string. It must comply with the naming convention.

-  **password**

   Specifies the password of a role. It must comply with the password convention.

-  **DEFAULT**

   Reset the identifiers of the session and current users to the initially authenticated user names.

Examples
--------

Set the current user to **paul**:

::

   SET SESSION AUTHORIZATION paul password '{password}';

View the current session user and the current user:

::

   SELECT SESSION_USER, CURRENT_USER;

Reset the current user:

::

   RESET SESSION AUTHORIZATION;

Helpful Links
-------------

:ref:`SET ROLE <dws_06_0222>`
