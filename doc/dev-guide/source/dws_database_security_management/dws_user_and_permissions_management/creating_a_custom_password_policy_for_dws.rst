:original_name: dws_04_0063.html

.. _dws_04_0063:

Creating a Custom Password Policy for DWS
=========================================

When creating or modifying a user, you need to specify a password. DWS has default password complexity requirements. You can also define database account password policies.

.. _en-us_topic_0000001811491169__en-us_topic_0000001188482292_section6276155616306:

Default DWS Password Policy
---------------------------

By default, DWS verifies the password complexity. The default password policy requires that the password:

-  Contain 8 to 32 characters.

-  Contain at least three types of the following characters: uppercase letters, lowercase letters, digits, and special characters. For details about the supported special characters, see :ref:`Table 1 <en-us_topic_0000001811491169__en-us_topic_0000001188482292_t4898d0ad2fc048f18baef73bbeacb95b>`.
-  Cannot be the same as the user name or the user name in reverse order, case insensitive.
-  Cannot be the current password or the current password spelled backwards.

.. _en-us_topic_0000001811491169__en-us_topic_0000001188482292_t4898d0ad2fc048f18baef73bbeacb95b:

.. table:: **Table 1** Supported special characters

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

.. _en-us_topic_0000001811491169__en-us_topic_0000001188482292_section1897910435417:

Setting and Changing a Password
-------------------------------

-  Both system administrators and common users need to periodically change their passwords to prevent the accounts from being stolen.

   For example, to change the password of the user **u1**, connect to the database as the administrator and run the following command:

   ::

      ALTER USER u1 IDENTIFIED BY 'newpassword' REPLACE 'oldpassword';

   .. note::

      The password must meet requirements in :ref:`Default DWS Password Policy <en-us_topic_0000001811491169__en-us_topic_0000001188482292_section6276155616306>`, or the execution will fail.

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

   If the following information is displayed, the role setting has been modified:

   .. code-block::

      SET ROLE
