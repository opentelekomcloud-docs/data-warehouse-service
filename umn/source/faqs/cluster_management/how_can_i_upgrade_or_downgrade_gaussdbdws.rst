:original_name: dws_03_0031.html

.. _dws_03_0031:

How Can I Upgrade or Downgrade GaussDB(DWS)?
============================================

Cluster patching or upgrading is automatic because GaussDB(DWS) upgrades its own version.

For service patch:

-  Duration: less than 10 minutes
-  Impact on services: 1 to 3 minutes of interruptions

For service upgrade:

-  Duration: less than 30 minutes
-  Impact on services: The database cannot be accessed.

Downgrade is not supported.

.. note::

   The following figure describes the database version.

   |image1|

   -  During service patch, the last digit of database version *X.X.X* is changed, for example, the database is upgraded from 1.1.0 to 1.1.1.
   -  During service upgrade, the first two digits of database version *X.X.X* are changed, for example, the database is upgraded from 1.1.0 to 1.2.0.

.. |image1| image:: /_static/images/en-us_image_0000001192706523.png
