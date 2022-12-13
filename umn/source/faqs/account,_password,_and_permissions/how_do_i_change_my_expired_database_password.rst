:original_name: dws_03_0092.html

.. _dws_03_0092:

How Do I Change My Expired Database Password?
=============================================

To change the password of the database administrator **dbadmin**, log in to the console and choose **More** > **Reset Password** in cluster row.

For security, the following two parameters manage account passwords. Log in to the console, click the cluster name and switch to the parameter modification page to modify the parameters.

-  **failed_login_attempts**: maximum number of consecutive incorrect password attempts before the account is locked. Run the following statement as user **dbadmin** to unlock the account:

   .. code-block::

      ALTER USER user_name ACCOUNT UNLOCK;

-  **password_effect_time**: validity period of the account password, in days. The default value is **90**.
