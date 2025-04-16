:original_name: DWS_DS_038.html

.. _DWS_DS_038:

Security Management
===================

.. important::

   Ensure that the operating system and the required software (refer to :ref:`System Requirements <dws_ds_004>` for more details) are updated with the latest patches to prevent vulnerabilities and other security issues.

Login History
-------------

-  When you log in to the database, Data Studio displays a window, describing the latest successful login and the failed login attempts during the last two successful logins.

|image1|

.. note::

   If the pop-up displays the message "Last login details not available", then it implies that the connected database does not support the last login display feature.

Password Expiry Notification
----------------------------

-  Your password will expire within 7 days from the date of notification. If the password expires, contact the database administrator to reset the password.
-  The password must be changed every 90 days.

Securing the Application In-Memory Data
---------------------------------------

While running Data Studio in a trusted environment, user must ensure to prevent malicious software scanning or accessing the memory which is used to store application data including sensitive information.

Data Encryption for Saved Data
------------------------------

You can ensure encryption of auto saved data by enabling encryption option from **Preferences** page. Refer to :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>` for steps to encrypt the saved data.

SQL History
-----------

-  SQL History scripts are not encrypted.
-  The SQL History list does not display sensitive queries that contain the following keywords:

   -  Alter Role
   -  Alter User
   -  Create Role
   -  Create User
   -  Identified by
   -  Password

-  Some query syntax examples are listed below:

   -  ALTER USER name [ WITH ] option [ ... ]]
   -  CREATE USER name [ [ WITH ] option [ ... ] ]
   -  CREATE ROLE name [ [ WITH ] option [ ... ] ]
   -  ALTER ROLE name [ [ WITH ] option [ ... ] ]

.. |image1| image:: /_static/images/en-us_image_0000001813599196.png
