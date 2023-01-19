:original_name: dws_04_0065.html

.. _dws_04_0065:

Setting the Validity Period of an Account
=========================================

Precautions
-----------

When creating a user, you need to specify the validity period of the user, including the start time and end time.

To enable a user not within the validity period to use its account, set a new validity period.

Procedure
---------

#. Run the following command to create a user and specify the start time and end time.

   ::

      CREATE USER joe WITH PASSWORD 'password' VALID BEGIN '2015-10-10 08:00:00' VALID UNTIL '2016-10-10 08:00:00';

#. If the user is not within the specified validity period, run the following command to set the start time and end time of a new validity period.

   ::

      ALTER USER joe WITH VALID BEGIN '2016-11-10 08:00:00' VALID UNTIL '2017-11-10 08:00:00';

.. note::

   If **VALID BEGIN** is not specified in the **CREATE ROLE** or **ALTER ROLE** statement, the start time of the validity period is not limited. If **VALID UNTIL** is not specified, the end time of the validity period is not limited. If both of the parameters are not specified, the user is always valid.
