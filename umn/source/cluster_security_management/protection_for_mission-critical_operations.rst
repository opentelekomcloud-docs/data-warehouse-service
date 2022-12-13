:original_name: dws_01_0701.html

.. _dws_01_0701:

Protection for Mission-Critical Operations
==========================================

Scenario
--------

GaussDB(DWS) protects mission-critical operations. If you want to perform a mission-critical operation on the management console, you must enter a credential for identity verification. You can perform the operation only after your identity is verified. For account security, you are advised to enable operation protection. The setting will take effect for both the account and its users.

Currently, the following operations are supported: scaling out a cluster, deleting a cluster, restarting a cluster, adding a CN, and deleting a CN.

Enabling Operation Protection
-----------------------------

Operation protection is disabled by default. To enable it, perform the following steps:

#. Log in to the GaussDB(DWS) console.

#. Move the cursor to the username in the upper right corner of the page and click **Security Settings** from the drop-down list.


   .. figure:: /_static/images/en-us_image_0000001134401104.png
      :alt: **Figure 1** Security Settings

      **Figure 1** Security Settings

#. On the **Security Settings** page, click the **Critical Operations** tab. Click **Enable** in the **Operation Protection** area.


   .. figure:: /_static/images/en-us_image_0000001180440471.png
      :alt: **Figure 2** Critical Operations

      **Figure 2** Critical Operations

#. On the **Operation Protection** page, select **Enable** to enable operation protection.

   .. note::

      -  When IAM users created using your account perform a critical operation, they will be prompted to choose a verification method from email, SMS, and virtual MFA device.

         -  If a user is only associated with a mobile number, only SMS verification will be available.
         -  If a user is only associated with an email address, only email verification will be available.
         -  If a user is not associated with an email address, mobile number, or virtual MFA device, the user will need to associate an email address, mobile number, or virtual MFA device with their account before the user can perform any critical operations.

      -  Change your phone number or email address for verification in **My Account** on the management console.

#. After operation protection is enabled, when you perform a mission-critical operation, the system will protect the operation.

   For example, when you delete a cluster, a verification dialog box for mission-critical operation protection is displayed. You need to select a mode to perform verification. This helps avoid risks and losses caused by misoperations.


   .. figure:: /_static/images/en-us_image_0000001134560886.png
      :alt: **Figure 3** Identity Verification

      **Figure 3** Identity Verification

Disabling Operation Protection
------------------------------

To disable operation protection, perform the following steps:

#. Log in to the GaussDB(DWS) console.

#. Move the cursor to the username in the upper right corner of the page and click **Security Settings** from the drop-down list.


   .. figure:: /_static/images/en-us_image_0000001134401104.png
      :alt: **Figure 4** Security Settings

      **Figure 4** Security Settings

#. On the **Security Settings** page, click the **Critical Operations** tab. Click **Change** in the **Operation Protection** area.


   .. figure:: /_static/images/en-us_image_0000001180320535.png
      :alt: **Figure 5** Modifying operation protection settings

      **Figure 5** Modifying operation protection settings

#. On the **Operation Protection** page, select **Disable** and click **OK**.
