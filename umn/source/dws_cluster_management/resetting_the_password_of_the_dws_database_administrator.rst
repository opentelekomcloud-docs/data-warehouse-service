:original_name: dws_01_0026.html

.. _dws_01_0026:

Resetting the Password of the DWS Database Administrator
========================================================

If you forget the password of the administrator account root or lock the account, you can reset the password on the **Dedicated Clusters** > **Clusters** page. The new password is applied immediately. **failed_login_attempts** indicates the maximum number of incorrect password attempts. Its default value is **10** and you can change its value on the **Parameter Modifications** page of the cluster. For details, see :ref:`Modifying GUC Parameters of the DWS Cluster <dws_01_0152>`.

Resetting a Password
--------------------

#. Log in to the DWS console.

#. Choose **Dedicated Clusters** > **Clusters**.

#. In the **Operation** column of the target cluster, choose **More** > **Reset Password**.

#. On the displayed **Reset Password** dialog box, set a new password, confirm the password, and then click **OK**.

   The password:

   -  Contains 12 to 32 characters.
   -  Cannot be the username or the username spelled backwards.
   -  Contains at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters (``~!?,.:;_(){}[]/<>@#%^&*+|\=-``)
   -  Passes the weak password check.

   -  Cannot be the same as the current password and cannot be the reverse of the current password.
   -  Cannot use a historical password.

   .. note::

      If the default database administrator account of the cluster is deleted or renamed, password resetting fails.
