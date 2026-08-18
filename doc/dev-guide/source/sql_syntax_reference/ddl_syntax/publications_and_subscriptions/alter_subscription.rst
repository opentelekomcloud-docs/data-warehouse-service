:original_name: dws_06_0287.html

.. _dws_06_0287:

ALTER SUBSCRIPTION
==================

Function
--------

**ALTER SUBSCRIPTION** modifies subscription attributes.

Precautions
-----------

-  This statement is supported by version 8.2.0.100 or later clusters.
-  Only the owner of a subscription can execute **ALTER SUBSCRIPTION**, and the new owner must be a system administrator.

Syntax
------

-  Update the connection information of a subscription.

   ::

      ALTER SUBSCRIPTION name CONNECTION 'conninfo';

-  Update the name of the publication on the publisher side.

   ::

      ALTER SUBSCRIPTION name SET PUBLICATION publication_name [, ...];

-  Enable a subscription.

   ::

      ALTER SUBSCRIPTION name ENABLE;

-  Disable a subscription.

   ::

      ALTER SUBSCRIPTION name DISABLE;

-  Set subscription parameters.

   ::

      ALTER SUBSCRIPTION name SET ( subscription_parameter [= value] [, ... ] );

-  Change the subscription owner.

   ::

      ALTER SUBSCRIPTION name OWNER TO new_owner;

-  Rename the subscription.

   ::

      ALTER SUBSCRIPTION name RENAME TO new_name;

Parameter Description
---------------------

.. table:: **Table 1** ALTER SUBSCRIPTION parameters

   +--------------------------------------------------+------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                                        | Description                                                                                                | Value Range                                                                                                                                         |
   +==================================================+============================================================================================================+=====================================================================================================================================================+
   | name                                             | Specifies the name of the subscription to be modified.                                                     | Name of an existing subscription.                                                                                                                   |
   +--------------------------------------------------+------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | CONNECTION 'conninfo'                            | Modifies the parameter attributes set by **CREATE SUBSCRIPTION** for connecting to the publisher database. | For details about the parameters, see :ref:`Parameter Description <en-us_topic_0000001811634793__section1549681213574>` in **CREATE SUBSCRIPTION**. |
   +--------------------------------------------------+------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | publication_name                                 | Specifies the name of a new publication.                                                                   | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.                                    |
   +--------------------------------------------------+------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | ENABLE                                           | Enables a previously disabled subscription and starts logical replication at the end of a transaction.     | ``-``                                                                                                                                               |
   +--------------------------------------------------+------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | DISABLE                                          | Disables a running subscription and stops logical replication at the end of a transaction.                 | ``-``                                                                                                                                               |
   +--------------------------------------------------+------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | SET ( publication_parameter [= value] [, ... ] ) | Modifies the publication parameters initially set by :ref:`CREATE PUBLICATION <dws_06_0285>`.              | For details about the parameter, see :ref:`Parameter Description <en-us_topic_0000001811634793__section1549681213574>` in **CREATE SUBSCRIPTION**.  |
   +--------------------------------------------------+------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | new_owner                                        | Specifies the name new subscription owner.                                                                 | Name of an existing user.                                                                                                                           |
   +--------------------------------------------------+------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
   | new_name                                         | Specifies the new name of the subscription.                                                                | A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.                                    |
   +--------------------------------------------------+------------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+

Examples
--------

-  Create the subscription **sub_demo**.

   .. code-block::

      DROP SUBSCRIPTION IF EXISTS sub_demo;
      CREATE SUBSCRIPTION sub_demo
          CONNECTION 'host=10.***.*.** port=10000 user=u_admin dbname=testdb password=********'
          PUBLICATION pub_sales_only;

-  Change the publication of the subscription to **insert_only**.

   ::

      ALTER SUBSCRIPTION sub_demo SET PUBLICATION insert_only;

-  Rename the subscription.

   ::

      ALTER SUBSCRIPTION sub_demo RENAME TO new_sub_demo;

-  Disable the subscription.

   .. code-block::

      ALTER SUBSCRIPTION new_sub_demo DISABLE;

Helpful Links
-------------

:ref:`CREATE SUBSCRIPTION <dws_06_0288>` and :ref:`DROP SUBSCRIPTION <dws_06_0289>`
