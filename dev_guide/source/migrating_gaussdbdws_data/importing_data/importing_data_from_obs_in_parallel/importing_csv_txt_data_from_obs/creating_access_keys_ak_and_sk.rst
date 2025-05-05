:original_name: dws_04_0183.html

.. _dws_04_0183:

Creating Access Keys (AK and SK)
================================

In this example, OBS data is imported to GaussDB(DWS) databases. When users who have registered with the cloud platform access OBS using clients, call APIs, or SDKs, access keys (AK/SK) are required for user authentication. Therefore, if you want to connect to the GaussDB(DWS) database through a client or a JDBC/ODBC application to access OBS, obtain the access keys (AK and SK) first.

-  Access Key ID (AK): indicates the ID of the access key, which is a unique identifier used in conjunction with a Secret Access Key to sign requests cryptographically.
-  Secret Access Key (SK): indicates the key used with its associated AK to cryptographically sign requests and identify request senders to prevent requests from being modified.


Creating Access Keys (AK and SK)
--------------------------------

Before creating an AK/SK pair, ensure that your account (used to log in to the management console) has passed real-name authentication.

To create an AK/SK pair on the management console, perform the following steps:

#. Log in to the GaussDB(DWS) console.

#. Click the username in the upper right corner and choose **My Credentials** from the drop-down list.

#. Select the **Access Keys** tab.

   If an access key already exists in the access key list, you can directly use it. However, you can view only **Access Key ID** in the access key list. You can download the key file containing the AK and SK only when adding an access key. If you do not have the key file, click **Create Access Key** to create one.

   .. note::

      -  Each user can create a maximum of two valid access keys. If there are already two access keys, delete them and create one. To delete an access key, you need to enter the current login password and email address or SMS verification code. Deletion is successful only after the verification is passed.
      -  To ensure account security, change your access keys periodically and keep them secure.

#. Click **Add Access Key**.

#. In the displayed **Add Access Key** dialog box, enter the password and its verification code and click **OK**.

   .. note::

      -  If you have not bound an email address or a mobile number, enter only the login password.
      -  If you have bound an email address and a mobile phone number, you can use either of them for verification.

#. In the displayed **Download Access Key** dialog box, click **OK** to save the access keys to your browser's default download path.

   .. note::

      -  Keep the access keys secure to prevent them from being leaked.
      -  If you click **Cancel** in the dialog box, the access keys will not be downloaded, and you cannot download them later. In this case, re-create access keys.

#. Open the downloaded **credentials.csv** file to obtain the access keys (AK and SK).

Precautions
-----------

If you find that your AK/SK pair is abnormally used (for example, the AK/SK pair is lost or leaked) or will be no longer used, delete your AK/SK pair in the access key list or contact the administrator to reset your AK/SK pair.

When deleting the access keys, you need to enter the login password and either an email or mobile verification code.

.. note::

   Deleted AK/SK pairs cannot be restored.
