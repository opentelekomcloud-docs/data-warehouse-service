:original_name: dws_07_0106.html

.. _dws_07_0106:

gds_install
===========

Context
-------

gds_install is a script tool used to install GDS in batches, improving GDS deployment efficiency.

Precautions
-----------

-  Configure environment variables before executing the script. For details, see "Importing Data > Using a Foreign Table to Import Data in Parallel > Installing, Configuring, and Starting GDS" in the *Developer Guide*.
-  The script must be executed in the Python 3 environment.
-  This script must be run by user **root**.
-  Check the permission on the upper-layer directory to ensure that the GDS user has the read and write permissions on the installation operation directory, installation directory, and installation package.
-  Cross-platform installation and deployment are not supported.
-  The node where the command is executed must be one of the installation and deployment nodes.
-  The passwords of all nodes specified by the **--host** parameter must be the same so that the script can be executed to perform remote deployment successfully.

Syntax
------

.. code-block::

   gds_install -I /path/to/install_dir -U user -G user_group --pkg /path/to/pkg.tar.gz --host [/path/to/hostfile | ipaddr1,ipaddr2...] [--ping-host [/path/to/hostfile | ipaddr1,ipaddr2...]]

Parameter Description
---------------------

-  -I

   Installation directory.

   Default value: **/opt/${gds_user}/packages/**, in which **${gds_user}** indicates the operating system user of the GDS service.

-  -U

   GDS user.

-  -G

   Group to which the GDS user belongs.

-  --pkg

   Path of the GDS installation package, for example, **/path/to/GaussDB-8.1.3-REDHAT-x86_64bit-Gds.tar.gz**.

-  --host

   IP addresses of the nodes to be installed. The value can be a file name or a string.

   -  File format: Each row contains an IP address, for example:

      192.168.2.200

      192.168.2.201

   -  String format: IP addresses are separated by commas (,), for example:

      192.168.2.200,192.168.2.201

   .. note::

      The node where the command is executed must be one of the nodes to be deployed. The IP address of the node must be in the list.

-  --ping-host

   Destination IP address for the network ping check on each target node when **gds_check** is called.

   Value: IP address list in the file or string format. Generally, the value is the IP address of a DN, CN, or gateway.

   -  File format: Each row contains an IP address, for example:

      192.168.2.200

      192.168.2.201

   -  String format: IP addresses are separated by commas (,), for example:

      192.168.2.200,192.168.2.201

-  -V

   Version information.

-  -h, --help

   Help information.

Examples
--------

Install GDS on nodes 192.168.1.100 and 192.168.1.101, and specify **/opt/gdspackages/install_dir** as the installation directory. The GDS user is **gds_test:wheel**.

.. code-block::

   gds_install -I /opt/gdspackages/install_dir --host 192.168.1.100,192.168.1.101 -U gds_test -G wheel --pkg /home/gds_test/GaussDB-8.1.3-REDHAT-x86_64bit-Gds.tar.gz
