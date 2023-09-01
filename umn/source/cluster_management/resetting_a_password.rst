:original_name: dws_01_0026.html

.. _dws_01_0026:

Resetting a Password
====================

GaussDB(DWS) allows you to reset the password of the database administrator. If a database administrator forgets their password or the account is locked because the number of consecutive incorrect password attempts reaches the upper limit, the database administrator can reset the password on the **Clusters** page. After the password is reset, the account can be automatically unlocked. You can set the maximum number of incorrect password attempts (10 by default) by configuring the :ref:`failed_login_attempts <en-us_topic_0000001518033697__section926416313488>` parameter on the **Parameter** page of the cluster. For details, see :ref:`Modifying Database Parameters <dws_01_0152>`.


Resetting a Password
--------------------

#. Log in to the GaussDB(DWS) management console.

#. Click **Clusters**.

#. In the **Operation** column of the target cluster, choose **More** > **Reset Password**.

#. On the displayed **Reset Password** page, set a new password, confirm the password, and then click **OK**.

   The password complexity requirements are as follows:

   -  Contains 8 to 32 characters.
   -  Cannot be the username or the username spelled backwards.
   -  Must contain at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters (:literal:`~!`?,.:;-_'"(){}[]/<>@#%^&*+|\\=`)
   -  Passes the weak password check.

   -  Cannot be the same as the old password and cannot be the reverse of the old password.
   -  Cannot use a historical password.

   .. note::

      If the default database administrator account of the cluster is deleted or renamed, password resetting fails.
