:original_name: dws_06_0151.html

.. _dws_06_0151:

CLEAN CONNECTION
================

Function
--------

**CLEAN CONNECTION** clears database connections when a database is abnormal. You may use this statement to delete a specific user's connections to a specified database.

Precautions
-----------

None

Syntax
------

::

   CLEAN CONNECTION
       TO { COORDINATOR ( nodename [, ... ] ) | NODE ( nodename [, ... ] )| ALL [ CHECK ] [ FORCE ] }
       [ FOR DATABASE dbname ]
       [ TO USER username ];

Parameter Description
---------------------

-  **CHECK**

   This parameter can be specified only when the node list is specified as **TO ALL**. Setting this parameter will check whether a database is accessed by other sessions before its connections are cleared. If any sessions are detected before **DROP DATABASE** is executed, an error will be reported and the database will not be deleted.

-  **FORCE**

   This parameter can be specified only when the node list is specified as **TO ALL**. Setting this parameter will send SIGTERM signals to all the threads related to the specified **dbname** and **username** and forcibly shut them down.

-  **COORDINATOR ( nodename [, ... ] ) \| NODE ( nodename [, ... ] ) \| ALL**

   Deletes connections on a specified node. There are three scenarios:

   -  Deletes connections to a specified CN.
   -  Deletes connections to a specified DN.
   -  Deletes connections to all CNs and DNs.

   Value range: **nodename** is an existing node name.

-  **dbname**

   Deletes connections to a specific database. If this parameter is not specified, connections to all databases will be deleted.

   Value range: an existing database name

-  **username**

   Deletes connections of a specific user. If this parameter is not specified, connections of all users will be deleted.

   Value range: an existing user name

   .. note::

      Either **dbname** or **username** must be specified.

Examples
--------

Clean connections to nodes dn1 and dn2 for the **template1** database.

::

   CLEAN CONNECTION TO NODE (dn1,dn2) FOR DATABASE template1;

Clean user **jack**'s connections to dn1.

::

   CLEAN CONNECTION TO NODE (dn1) TO USER jack;

Delete all connections to the **gaussdb** database.

::

   CLEAN CONNECTION TO ALL FORCE FOR DATABASE gaussdb;
