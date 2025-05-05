:original_name: dws_07_0107.html

.. _dws_07_0107:

gds_uninstall
=============

Background
----------

gds_uninstall is a script tool used to uninstall GDS in batches.

Precautions
-----------

-  Set environment variables before executing the script. For details, see "Importing Data > Using a Foreign Table to Import Data In Parallel > Installing, Configuring, and Starting GDS" in the *Developer Guide*.
-  The script must be executed in the Python 3 environment.
-  You must execute the **gds_uninstall** script as the **root** user.
-  The **--host** and **-U** parameters must be contained.
-  Currently, cross-platform uninstallation is not supported.
-  The passwords of all nodes specified by the **--host** parameter must be the same to ensure that the script can be remotely uninstalled.

Syntax
------

.. code-block::

   gds_uninstall --host [/path/to/hostfile | ipaddr1,ipaddr2...] -U gds_user [--delete-user | --delete-user-and-group]

Parameter Description
---------------------

-  --host

   IP addresses of the nodes to be uninstalled. The value can be a file name or a string:

   -  File format: Each IP address occupies a row, for example:

      192.168.2.200

      192.168.2.201

   -  String format: IP addresses are separated by commas (,), for example:

      192.168.2.200,192.168.2.201

-  -U

   GDS user.

-  --delete-user

   The user is deleted when GDS is uninstalled. The user to be deleted cannot be the **root** user.

-  --delete-user-and-group

   When GDS is uninstalled, the user and the user group to which the user belongs are deleted. You can delete a user group only when the user to be deleted is the only user of the user group. The user group cannot be the **root** user group.

-  -V

   Version information.

-  -h, --help

   Help information.

Example
-------

Uninstall the GDS folders and environment variables installed and deployed by the **gds_test** user on nodes 192.168.1.100 and 192.168.1.101.

.. code-block::

   gds_uninstall -U gds_test --host 192.168.1.100,192.168.1.101

The user is deleted when GDS is uninstalled.

.. code-block::

   gds_uninstall -U gds_test --host 192.168.1.100,192.168.1.101 --delete-user

During the uninstallation, the user and user group are deleted at the same time.

.. code-block::

   gds_uninstall -U gds_test --host 192.168.1.100,192.168.1.101 --delete-user-and-group
