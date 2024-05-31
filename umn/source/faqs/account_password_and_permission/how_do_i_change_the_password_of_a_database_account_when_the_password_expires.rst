:original_name: dws_03_0092.html

.. _dws_03_0092:

How Do I Change the Password of a Database Account When the Password Expires?
=============================================================================

-  To change the password of the database administrator **dbadmin**, log in to the console and choose **More** > **Reset Password** in cluster row.


   .. figure:: /_static/images/en-us_image_0000001687122453.png
      :alt: **Figure 1** Resetting the password of user dbadmin

      **Figure 1** Resetting the password of user dbadmin

   For security, the following two parameters manage account passwords. Log in to the console, click the cluster name and switch to the parameter modification page to modify the parameters.

   -  **failed_login_attempts**: maximum number of consecutive incorrect password attempts before the account is locked. Run the following statement as user **dbadmin** to unlock the account:

      .. code-block::

         ALTER USER user_name ACCOUNT UNLOCK;

   -  **password_effect_time**: validity period of the account password, in days. The default value is **90**.

-  You can also connect to the database and run the **ALTER USER** command to change the password validity period of a database account (common user and administrator dbadmin).

   ::

      ALTER USER username PASSWORD EXPIRATION 90;
