:original_name: dws_04_0890.html

.. _dws_04_0890:

Security and Authentication (postgresql.conf)
=============================================

This section describes parameters about how to securely authenticate the client and server.

authentication_timeout
----------------------

**Parameter description**: Specifies the longest duration to wait before the client authentication times out. If a client is not authenticated by the server within the timeout period, the server automatically breaks the connection from the client so that the faulty client does not occupy connection resources.

**Type**: SIGHUP

**Value range**: an integer ranging from 1 to 600. The minimum unit is second (s).

**Default value**: **1min**

session_timeout
---------------

**Parameter description**: Specifies the longest duration with no operations after the connection to the server.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 86400. The minimum unit is second (s). **0** means to disable the timeout.

**Default value**: **10 min**

.. important::

   -  The gsql client of GaussDB(DWS) has an automatic reconnection mechanism. If the initialized local connection of a user to the server times out, gsql disconnects from and reconnects to the server.
   -  Connections from the pooler connection pool to other CNs and DNs are not controlled by the **session_timeout** parameter.

ssl_ciphers
-----------

**Parameter description**: Specifies the encryption algorithm list supported by the SSL.

**Type**: POSTMASTER

**Value range**: a string. Separate multiple encryption algorithms with semicolons (;).

**Default value**: **ALL**

.. note::

   -  The default value of **ssl_ciphers** is **ALL**, indicating that all the following encryption algorithms are supported. Users are advised to retain the default value, unless there are other special requirements on the encryption algorithm.

      -  TLS1_3_RFC_AES_128_GCM_SHA256
      -  TLS1_3_RFC_AES_256_GCM_SHA384
      -  TLS1_3_RFC_CHACHA20_POLY1305_SHA256
      -  TLS1_3_RFC_AES_128_CCM_SHA256
      -  TLS1_3_RFC_AES_128_CCM_8_SHA256

   -  Currently, SSL connection authentication supports only the TLS1.3 encryption algorithm, which has better performance and security. It is also compatible with SSL connection authentication between clients that comply with TLS1.2.

ssl_renegotiation_limit
-----------------------

**Parameter description**: Specifies the traffic volume over the SSL-encrypted channel before the session key is renegotiated. The renegotiation traffic limitation mechanism reduces the probability that attackers use the password analysis method to crack the key based on a huge amount of data but causes big performance losses. The traffic indicates the sum of sent and received traffic.

**Type**: USERSET

.. note::

   You are advised to retain the default value, that is, disable the renegotiation mechanism. You are not advised to use the **gs_guc** tool or other methods to set the **ssl_renegotiation_limit** parameter in the **postgresql.conf** file. The setting does not take effect.

**Value range**: an integer ranging from 0 to **INT_MAX**. The unit is KB. **0** indicates that the renegotiation mechanism is disabled.

**Default value**: **0**

password_policy
---------------

**Parameter description**: Specifies whether to check the password complexity when you run the **CREATE ROLE/USER** or **ALTER ROLE/USER** command to create or modify a GaussDB(DWS) account.

**Type**: SIGHUP

.. important::

   For security purposes, do not disable the password complexity policy.

**Value range**: an integer, **0** or **1**

-  **0** indicates that no password complexity policy is enabled.
-  **1** indicates that the default password complexity policy is disabled.

**Default value**: **1**

.. _en-us_topic_0000001510402265__sbbafd6b400d246ad9b10b95fd632643c:

password_reuse_time
-------------------

**Parameter description:** Specifies whether to check the reuse days of the new password when you run the **ALTER USER** or **ALTER ROLE** command to change a user password.

**Type**: SIGHUP

.. important::

   When you change the password, the system checks the values of :ref:`password_reuse_time <en-us_topic_0000001510402265__sbbafd6b400d246ad9b10b95fd632643c>` and :ref:`password_reuse_max <en-us_topic_0000001510402265__scadaeaf8f1ee4427b11857bcd78cb191>`.

   -  If the values of **password_reuse_time** and **password_reuse_max** are both positive numbers, the password can be reused if either of the following conditions is met:
   -  If the value of **password_reuse_time** is **0**, the days of password reuse are not limited and only the times of password reuse are limited.
   -  If the value of **password_reuse_max** is **0**, the times of password reuse are not limited and only the days of password reuse are limited.
   -  If the values of both parameters are **0**, password reuse is not restricted.

**Value range**: a floating number ranging from 0 to 3650. The unit is day.

-  **0** indicates that the password reuse days are not checked.
-  A positive number indicates that the new password cannot be the one that is used within the specified days.

**Default value**: **60**

.. _en-us_topic_0000001510402265__scadaeaf8f1ee4427b11857bcd78cb191:

password_reuse_max
------------------

**Parameter description:** Specifies whether to check the reuse times of the new password when you run the **ALTER USER** or **ALTER ROLE** command to change a user password.

**Type**: SIGHUP

.. important::

   When you change the password, the system checks the values of :ref:`password_reuse_time <en-us_topic_0000001510402265__sbbafd6b400d246ad9b10b95fd632643c>` and :ref:`password_reuse_max <en-us_topic_0000001510402265__scadaeaf8f1ee4427b11857bcd78cb191>`.

   -  If the values of **password_reuse_time** and **password_reuse_max** are both positive numbers, the password can be reused if either of the following conditions is met:
   -  If the value of **password_reuse_time** is **0**, the days of password reuse are not limited and only the times of password reuse are limited.
   -  If the value of **password_reuse_max** is **0**, the times of password reuse are not limited and only the days of password reuse are limited.
   -  If the values of both parameters are **0**, password reuse is not restricted.

**Value range**: an integer ranging from 0 to 1000

-  **0** indicates that the password reuse times are not checked.
-  A positive number indicates that the new password cannot be the one whose reuse times exceed the specified number.

**Default value**: **0**

.. _en-us_topic_0000001510402265__s943fe3c453f648fb958919ab0aa2b08b:

password_lock_time
------------------

**Parameter description**: Specifies the duration before an account is automatically unlocked.

**Type**: SIGHUP

.. important::

   -  The locking and unlocking functions take effect only when the values of **password_lock_time** and :ref:`failed_login_attempts <en-us_topic_0000001510402265__s98a9fdb6b85f4f6ab813a269524dc136>` are positive numbers.
   -  The integral part of the value of the **password_lock_time** parameter indicates the number of days and its decimal part can be converted into hours, minutes, and seconds.

**Value range**: a floating number ranging from 0 to 365. The unit is day.

-  **0** indicates that the automatic locking function does not take effect if the password verification fails.
-  A positive number indicates the duration after which an account is automatically unlocked.

**Default value**: **1**

.. _en-us_topic_0000001510402265__s98a9fdb6b85f4f6ab813a269524dc136:

failed_login_attempts
---------------------

**Parameter description**: Specifies the maximum number of incorrect password attempts before an account is locked. The account will be automatically unlocked after the time specified in **password_lock_time**. For example, incorrect password attempts during login and password input failures when using the **ALTER USER** command

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 1000

-  **0** indicates that the automatic locking function does not take effect.
-  A positive number indicates that an account is locked when the number of incorrect password attempts reaches the value of **failed_login_attempts**.

**Default value**: **10**

.. important::

   -  The locking and unlocking functions take effect only when the values of **failed_login_attempts** and :ref:`password_lock_time <en-us_topic_0000001510402265__s943fe3c453f648fb958919ab0aa2b08b>` are positive numbers.
   -  **failed_login_attempts** works with the SSL connection mode of the client to identify the number of incorrect password attempts. If PGSSLMODE is set to **allow** or **prefer**, two connection requests are generated for a password connection request. One request attempts an SSL connection, and the other request attempts a non-SSL connection. In this case, the number of incorrect password attempts perceived by the user is the value of **failed_login_attempts** divided by 2.

password_encryption_type
------------------------

**Parameter description**: Specifies the encryption type of user passwords.

**Type**: SIGHUP

**Value range**: an integer, **0**, **1**, or **2**

.. table:: **Table 1** Value description

   +-----------------------+---------------------------------------------------------------------------------------------------------------+------------------------------------------------+
   | Value                 | Password Storage Format                                                                                       | Supported Driver                               |
   +=======================+===============================================================================================================+================================================+
   | 0                     | Passwords are encrypted in by MD5 and stored in ciphertext.                                                   | GaussDB and open-source drivers are supported. |
   +-----------------------+---------------------------------------------------------------------------------------------------------------+------------------------------------------------+
   | 1                     | Passwords are encrypted by SHA256 and are compatible with the MD5 user authentication of the postgres client. | GaussDB and open-source drivers are supported. |
   |                       |                                                                                                               |                                                |
   |                       | Passwords are encrypted using MD5 and SHA256.                                                                 |                                                |
   +-----------------------+---------------------------------------------------------------------------------------------------------------+------------------------------------------------+
   | 2                     | Passwords are encrypted by SHA256 and stored in ciphertext.                                                   | GaussDB drivers are supported.                 |
   +-----------------------+---------------------------------------------------------------------------------------------------------------+------------------------------------------------+

.. important::

   -  MD5 is not recommended because it is not a secure encryption algorithm.
   -  For a user created when **password_encryption_type** is set to **2**, the password has been saved using the SHA256 algorithm. In this case, changing the parameter value does not change the password storage method in the database. So, open source clients using MD5 may still fail to connect to the database.
   -  When **password_encryption_type** is set to **1** and **pg_hba** is set to **MD5** or **SHA256**, the two encryption modes are checked to ensure compatibility.

**Default value**: 1

password_min_length
-------------------

**Parameter description**: Specifies the minimum account password length.

**Type**: SIGHUP

**Value range**: an integer. A password can contain 6 to 999 characters.

**Default value**: **8**

password_max_length
-------------------

**Parameter description**: Specifies the maximum account password length.

**Type**: SIGHUP

**Value range**: an integer. A password can contain 6 to 999 characters.

**Default value**: **32**

password_min_uppercase
----------------------

**Parameter description**: Specifies the minimum number of uppercase letters that an account password must contain.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 999.

-  **0** means no limit.
-  A positive integer indicates the minimum number of uppercase letters in the password specified for creating an account.

**Default value**: **0**

password_min_lowercase
----------------------

**Parameter description**: Specifies the minimum number of lowercase letters that an account password must contain.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 999.

-  **0** means no limit.
-  A positive integer indicates the minimum number of lowercase letters in the password specified for creating an account.

**Default value**: **0**

password_min_digital
--------------------

**Parameter description**: Specifies the minimum number of digits that an account password must contain.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 999.

-  **0** means no limit.
-  A positive integer indicates the minimum number of digits in the password specified for creating an account.

**Default value**: **0**

password_min_special
--------------------

**Parameter description**: minimum number of special characters that a password must contain.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 999.

-  **0** means no limit.
-  A positive integer indicates the minimum number of special characters in the password specified for creating an account.

**Default value**: **0**

.. table:: **Table 2** Special characters

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

password_effect_time
--------------------

**Parameter description**: Specifies the validity period of an account password.

**Type**: SIGHUP

**Value range**: a floating number ranging from 0 to 999. The unit is day.

-  **0** indicates the function of validity period restriction is disabled.
-  A floating point number from 1 to 999 indicates the validity period of the password specified for creating an account. When the password is about to expire or has expired, the system prompts the user to change the password.

**Default value**: **90**

password_notify_time
--------------------

**Parameter description**: Specifies how many days in advance users are notified before the account password expires.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 999. The unit is day.

-  **0** indicates the reminder is disabled.
-  A positive integer indicates how long before expiry the reminder will appear.

**Default value**: **7**
