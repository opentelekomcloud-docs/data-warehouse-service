:original_name: dws_01_0026.html

.. _dws_01_0026:

Resetting the Password the GaussDB(DWS) Database Administrator
==============================================================

GaussDB(DWS) allows you to reset the password of the database administrator. If a database administrator forgets their password or the account is locked because the number of consecutive incorrect password attempts reaches the upper limit, the database administrator can reset the password on the **Dedicated Clusters** > **Clusters** page. After the password is reset, the account can be automatically unlocked. You can set the maximum number of incorrect password attempts (10 by default) by configuring the **failed_login_attempts** parameter on the **Parameter** page of the cluster. For details, see :ref:`Modifying GUC Parameters of the GaussDB(DWS) Cluster <dws_01_0152>`.

Resetting a Password
--------------------

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters**.

#. In the **Operation** column of the target cluster, choose **More** > **Reset Password**.

#. On the displayed **Reset Password** page, set a new password, confirm the password, and then click **OK**.

   The password complexity requirements are as follows:

   -  Contains 12 to 32 characters.
   -  Cannot be the username or the username spelled backwards.
   -  Contains at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters (``~!?,.:;_(){}[]/<>@#%^&*+|\=-``)
   -  Passes the weak password check.

   -  Cannot be the same as the old password and cannot be the reverse of the old password.
   -  Cannot use a historical password.

   .. note::

      If the default database administrator account of the cluster is deleted or renamed, password resetting fails.
