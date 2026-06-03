:original_name: dws_03_0092.html

.. _dws_03_0092:

How Do I Change the Password of a DWS Database Account When It Expires?
=======================================================================

Below are the methods for changing the password of a database account:

-  To change the password of user **dbadmin**, log in to the DWS console, locate the target cluster and choose **More** > **Reset Password** in the **Operation** column.


   .. figure:: /_static/images/en-us_image_0000001687122453.png
      :alt: **Figure 1** Resetting the password of user dbadmin

      **Figure 1** Resetting the password of user dbadmin

-  You can also connect to the database and run the **ALTER USER** command to change the password validity period of a database account (common user and administrator dbadmin).

   ::

      ALTER USER username PASSWORD EXPIRATION 90;
