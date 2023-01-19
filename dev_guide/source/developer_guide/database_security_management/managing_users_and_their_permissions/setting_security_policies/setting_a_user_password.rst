:original_name: dws_04_0067.html

.. _dws_04_0067:

Setting a User Password
=======================

User passwords are stored in the system catalog **pg_authid**. To prevent password leakage, GaussDB(DWS) encrypts and stores the user passwords.

-  Password complexity

   The password complexity requirements are as follows:

   -  The number of uppercase letters (A-Z) ranges from 0 to 999, the number of lowercase letters (a-z) ranges from 0 to 999, the number of digits ranges from 0 to 999, and the number of special characters ranges from 0 to 999. For details about the special characters, see :ref:`Table 1 <en-us_topic_0000001101463816__en-us_topic_0000001082926733_tbef4ccb5367248c1a36d06d61ee7059c>`.

      .. note::

         A password must contain at least three types of the preceding characters (uppercase letters, lowercase letters, digits, and special characters).

   -  A password must contain at least 6 characters, and the default length is 8 characters.
   -  A password can contain a maximum of 999 characters, and the default length is 32 characters.
   -  A password must differ from the username or the username in reverse order.
   -  A new password must be different from the current password or the current password in reverse order.

-  Password reuse

   When a user changes the password, the user can reuse a password only if it has not been used for over 60 days.

-  Password validity period

   A validity period (90 days by default) is set for each database user password. If the password is about to expire (in seven days), the system displays a message reminding the user to change it upon login.

   .. note::

      Considering the usage and service continuity of a database, the database still allows a user to log in after the password expires. A password change notification is displayed every time the user logs in to the database until the password is changed.

-  Password change

   -  During database installation, an OS user named the same as the initial user is created. The password of the OS user needs to be changed periodically to ensure account security.

      If the password of the user **user1** needs to be changed, run the following command:

      .. code-block::

         passwd user1

      Change the password as prompted.

   -  Both system administrators and common users need to periodically change their passwords to prevent the accounts from being stolen.

      For example, to change the password of the user **user1**, connect to the database as the administrator and run the following command:

      ::

         ALTER USER user1 IDENTIFIED BY "1234@abc" REPLACE "5678@def";

      .. note::

         **1234@abc** and **5678@def** represent the new password and the original password of the user **user1**, respectively. The new password must conform to the complexity rules. Otherwise, the new password is invalid.

   -  An administrator can change its own password and other accounts' passwords. With the permission for changing other accounts' passwords, the administrator can resolve a login failure when a user forgets its password.

      To change the password of the user **joe**, run the following command:

      ::

         ALTER USER joe IDENTIFIED BY 'password';

   .. note::

      -  System administrators are not allowed to change passwords for each other.
      -  When a system administrator changes the password of a common user, the original password is not required.
      -  However, when a system administrator changes its own password, the original password is required.

-  Password verification

   Password verification is required when you set the user or role in the current session. If the entered password is inconsistent with the stored password of the user, an error is reported.

   To set the password of the user **joe**, run the following command:

   ::

      SET ROLE joe PASSWORD 'password';

.. _en-us_topic_0000001101463816__en-us_topic_0000001082926733_tbef4ccb5367248c1a36d06d61ee7059c:

.. table:: **Table 1** Special characters

   === ========= === ========= === ========= ===== =========
   No. Character No. Character No. Character No.   Character
   === ========= === ========= === ========= ===== =========
   1   ~         9   \*        17  \|        25    <
   2   !         10  (         18  [         26    .
   3   @         11  )         19  {         27    >
   4   #         12  ``-``     20  }         28    /
   5   $         13  \_        21  ]         29    ?
   6   %         14  =         22  ;         ``-`` ``-``
   7   ^         15  +         23  :         ``-`` ``-``
   8   &         16  \\        24  ,         ``-`` ``-``
   === ========= === ========= === ========= ===== =========
