:original_name: dws_06_0288.html

.. _dws_06_0288:

CREATE SUBSCRIPTION
===================

Function
--------

Adds a new subscription to the current database. The subscription name must be different from a name of any existing subscription in the database. A subscription represents a replication for connecting to the publication side.

Precautions
-----------

-  This statement is supported by clusters of version 8.2.0.100 or later.
-  A subscription can be created only by the system administrator.

Syntax
------

::

   CREATE SUBSCRIPTION name
       CONNECTION 'conninfo'
       PUBLICATION publication_name [, ...]
       [ WITH ( subscription_parameter [= value] [, ... ] ) ]

.. _en-us_topic_0000001510520997__section1549681213574:

Parameter Description
---------------------

-  **name**

   Specifies the name of a subscription.

   Value range: A string. It must comply with the naming convention.

-  .. _en-us_topic_0000001510520997__li17392131613264:

   **conninfo**

   Specifies the string for connecting to the publication side.

   Example: **host=1.1.1.1,2.2.2.2 port=10000,20000 dbname=postgres user=repusr1 password=password_123**

   -  **host**

      IP address of the publication side. You can specify the IP addresses of both the primary and standby nodes of the publication side. Separate the IP addresses with commas (,).

   -  **port**

      The publication port cannot be the primary port. Instead, it must be the primary port number plus 1. Otherwise, the port number conflicts with the thread pool. You can specify the ports of both the primary and standby nodes of the publication side. Separate the ports with commas (,).

      .. caution::

         The number of hosts must be the same as that of ports.

   -  **dbname**

      Specifies the database where a publication is deployed.

   -  **user** and **password**

      Specifies the username and password used to connect to the publication side. The user has the system administrator permission (**SYSADMIN**) or O&M administrator permission (**OPRADMIN**).

-  **publication_name**

   Specifies the name of the publication you want to subscribe to on the publication side. A subscription corresponds to multiple publications.

-  **WITH ( subscription_parameter [= value] [, ... ] )**

   Specifies the optional parameters for a subscription. The following parameters are supported:

   -  **enabled**

      Specifies whether a subscription should be actively replicated, or whether it should be just set but not started.

      The value can be **true** or **false**.

      The default value is **true**.

   -  **create_slot**

      Specifies whether a replication slot is created on the publisher.

      The value can be **true** or **false**.

      The default value is **true**.

   -  **slot_name**

      Specifies the name of the replication slot.

      Value range: a string

      Default value: the subscription name

Examples
--------

Create a subscription to tables of the publication **mypublication** on a remote server.

.. code-block::

   CREATE SUBSCRIPTION mysub
       CONNECTION 'host=192.168.1.50 port=5432 user=foo dbname=foodb password=xxxx'
       PUBLICATION mypublication;

Helpful Links
-------------

:ref:`ALTER SUBSCRIPTION <dws_06_0287>` :ref:`DROP SUBSCRIPTION <dws_06_0289>`
