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

      ALTER SUBSCRIPTION name CONNECTION 'conninfo'

-  Update the name of the publication on the publisher side.

   ::

      ALTER SUBSCRIPTION name SET PUBLICATION publication_name [, ...]

-  Enable a subscription.

   ::

      ALTER SUBSCRIPTION name ENABLE

-  Disable a subscription.

   ::

      ALTER SUBSCRIPTION name DISABLE

-  Set subscription parameters.

   ::

      ALTER SUBSCRIPTION name SET ( subscription_parameter [= value] [, ... ] )

-  Change the subscription owner.

   ::

      ALTER SUBSCRIPTION name OWNER TO new_owner

-  Rename the subscription.

   ::

      ALTER SUBSCRIPTION name RENAME TO new_name

Parameter Description
---------------------

-  **name**

   Specifies the name of the subscription you want to modify.

   Value range: A string. It must comply with the naming convention.

-  **CONNECTION 'conninfo'**

   Alters the connection attributes initially set by **CREATE SUBSCRIPTION**.

   For details about the parameters, see :ref:`conninfo <en-us_topic_0000001811634793__li17392131613264>` in the parameter description.

-  **publication_name**

   Specifies the name of a new publication.

   Value range: A string. It must comply with the naming convention.

-  **ENABLE**

   Enables a previously disabled subscription and starts logical replication at the end of a transaction.

-  **DISABLE**

   Disables a running subscription and stops logical replication at the end of a transaction.

-  **SET ( publication_parameter [= value] [, ... ] )**

   Modifies the publication parameters initially set by **CREATE PUBLICATION**. For details about the parameters, see :ref:`Parameter description <en-us_topic_0000001811634793__section1549681213574>` of CREATE SUBSCRIPTION.

-  **new_owner**

   Specifies the name new subscription owner.

-  **new_name**

   Specifies the new name of the subscription.

Examples
--------

Modify subscription connection information.

.. code-block::

   ALTER SUBSCRIPTION mysub CONNECTION 'host=192.168.1.51 port=5432 user=foo dbname=foodb password=xxxx';

Helpful Links
-------------

:ref:`CREATE SUBSCRIPTION <dws_06_0288>`, :ref:`DROP SUBSCRIPTION <dws_06_0289>`
