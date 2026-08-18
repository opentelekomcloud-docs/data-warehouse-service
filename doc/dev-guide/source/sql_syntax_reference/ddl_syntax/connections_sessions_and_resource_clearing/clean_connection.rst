:original_name: dws_06_0151.html

.. _dws_06_0151:

CLEAN CONNECTION
================

Function
--------

**CLEAN CONNECTION** clears database connections. You may use this statement to delete a specific user's connections to a specified database.

It is mainly used to handle database exceptions, clear idle or invalid connections, and check connection access before dropping a database to prevent operation failures.

Precautions
-----------

You must specify either **dbname** or **username**.

Syntax
------

::

   CLEAN CONNECTION
       TO { COORDINATOR ( nodename [, ... ] ) | NODE ( nodename [, ... ] )| ALL [ CHECK ] [ FORCE ] }
       [ FOR DATABASE dbname ]
       [ TO USER username ];

Parameter Description
---------------------

.. table:: **Table 1** CLEAN CONNECTION parameters

   +------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
   | Parameter                                                              | Description                                                                                                                                              | Value Range                            |
   +========================================================================+==========================================================================================================================================================+========================================+
   | CHECK                                                                  | This parameter can be specified only when the node list is specified as **TO ALL**.                                                                      | ``-``                                  |
   |                                                                        |                                                                                                                                                          |                                        |
   |                                                                        | Setting this parameter will check whether a database is accessed by other sessions before its connections are cleared.                                   |                                        |
   |                                                                        |                                                                                                                                                          |                                        |
   |                                                                        | If any sessions are detected before **DROP DATABASE** is executed, an error will be reported and the database will not be deleted.                       |                                        |
   +------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
   | FORCE                                                                  | This parameter can be specified only when the node list is specified as **TO ALL**.                                                                      | ``-``                                  |
   |                                                                        |                                                                                                                                                          |                                        |
   |                                                                        | If this parameter is specified, all threads related to the specified **dbname** and **username** will receive the SIGTERM signal and be forcibly closed. |                                        |
   +------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
   | COORDINATOR ( nodename [, ... ] ) \| NODE ( nodename [, ... ] ) \| ALL | Deletes connections on a specified node. There are three scenarios:                                                                                      | **nodename** is an existing node name. |
   |                                                                        |                                                                                                                                                          |                                        |
   |                                                                        | #. Deletes connections on a specified CN.                                                                                                                |                                        |
   |                                                                        | #. Deletes connections on a specified DN.                                                                                                                |                                        |
   |                                                                        | #. Deletes connections on all nodes, including CNs and DNs.                                                                                              |                                        |
   +------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
   | dbname                                                                 | Deletes connections on a specific database.                                                                                                              | Valid database name.                   |
   |                                                                        |                                                                                                                                                          |                                        |
   |                                                                        | If this parameter is not specified, connections to all databases will be deleted.                                                                        |                                        |
   +------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
   | username                                                               | Deletes connections on a specific user.                                                                                                                  | Valid username.                        |
   |                                                                        |                                                                                                                                                          |                                        |
   |                                                                        | If this parameter is not specified, connections of all users will be deleted.                                                                            |                                        |
   +------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+

Examples
--------

Clean connections to nodes dn1 and dn2 for the **template1** database.

::

   CLEAN CONNECTION TO NODE (dn1,dn2) FOR DATABASE template1;

Clean user **jack**'s connections to dn1.

::

   CLEAN CONNECTION TO NODE (dn1) TO USER jack;

Delete all connections to the gaussdb database.

::

   CLEAN CONNECTION TO ALL FORCE FOR DATABASE gaussdb;
