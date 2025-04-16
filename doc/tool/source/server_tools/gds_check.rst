:original_name: dws_07_0104.html

.. _dws_07_0104:

gds_check
=========

Context
-------

gds_check is used to check the GDS deployment environment, including the OS parameters, network environment, and disk usage. It also supports the recovery of system parameters. This helps detect potential problems during GDS deployment and running, improving the execution success rate.

Precautions
-----------

-  Set environment variables before executing the script. For details, see "Importing Data > Using a Foreign Table to Import Data In Parallel > Installing, Configuring, and Starting GDS" in the *Developer Guide*.
-  The script must be executed in the Python 3 environment.
-  This script must be run by user **root**.
-  The **-t** and **--host** parameters must be specified.
-  If **--host** specifies the network address 0.0.0.0 or 127.0.0.1, the MTU and NIC multi-queue are not checked.
-  The NIC multi-queue check and recovery require that the NIC be at least 10 GE.
-  The passwords of all nodes specified by the **--host** parameter must be the same so that the script can perform remote check successfully.
-  During the recovery, set the OS configuration items according to the recommended values. For details, see the following table.

   .. table:: **Table 1** OS configuration items

      ============================ =================
      Parameter                    Recommended Value
      ============================ =================
      net.core.somaxconn           65535
      net.ipv4.tcp_max_syn_backlog 65535
      net.core.netdev_max_backlog  65535
      net.ipv4.tcp_retries1        5
      net.ipv4.tcp_retries2        12
      net.ipv4.ip_local_port_range 26000 to 65535
      MTU                          1500
      net.core.wmem_max            21299200
      net.core.rmem_max            21299200
      net.core.wmem_default        21299200
      net.core.rmem_default        21299200
      max handler                  1000000
      vm.swappiness                10
      ============================ =================

   .. table:: **Table 2** Disk check

      ================ ==============================================
      Check Item       Warning
      ================ ==============================================
      Disk space usage Greater than or equal to 70% and less than 90%
      Inode usage      Greater than or equal to 70% and less than 90%
      ================ ==============================================

   .. table:: **Table 3** Network conditions

      +----------------------+----------------------------------------------------------------------------------------+
      | Check Item           | Error                                                                                  |
      +======================+========================================================================================+
      | Network connectivity | 100% packet loss                                                                       |
      +----------------------+----------------------------------------------------------------------------------------+
      | NIC multi-queue      | When NIC multi-queue is enabled and different CPUs are bound, **fix** can be modified. |
      +----------------------+----------------------------------------------------------------------------------------+

Syntax
------

-  **check** command

   .. code-block::

      gds_check -t check --host [/path/to/hostfile | ipaddr1,ipaddr2...] --ping-host [/path/to/pinghostfile | ipaddr1,ipaddr2...] [--detail]

-  **fix** command

   .. code-block::

      gds_check -t fix --host [/path/to/hostfile | ipaddr1,ipaddr2...] [--detail]

Parameter Description
---------------------

-  -t

   Operation type, indicating check or recovery.

   The value can be **check** or **fix**.

-  --host

   IP addresses of the nodes to be checked or recovered.

   Value: IP address list in the file or character string format

   -  File format: Each IP address occupies a row, for example:

      192.168.1.200

      192.168.1.201

   -  Character string format: IP addresses are separated by commas (,), for example:

      192.168.1.200,192.168.1.201

-  --ping-host

   Destination IP address for the network ping check on each node to be checked.

   Value: IP address list in the file or character string format. Generally, the value is the IP address of a DN, CN, or gateway.

   -  File format: Each IP address occupies a row, for example:

      192.168.2.200

      192.168.2.201

   -  Character string format: IP addresses are separated by commas (,), for example:

      192.168.2.200,192.168.2.201

-  --detail

   Displays detailed information about check and repair items and saves the information to logs.

-  -V

   Version information.

-  -h, --help

   Help information.

Examples
--------

Perform a check. Both **--host** and -**-ping-host** are in character string format.

.. code-block::

   gds_check -t check --host 192.168.1.100,192.168.1.101 --ping-host 192.168.2.100

Perform a check. **--host** is in character string format and **--ping-host** is in file format.

.. code-block::

   gds_check -t check --host 192.168.1.100,192.168.1.101 --ping-host /home/gds/iplist

   cat /home/gds/iplist
   192.168.2.100
   192.168.2.101

Perform a check. **--host** is in file format and **--ping-host** is in character string format.

.. code-block::

   gds_check -t check --host  /home/gds/iplist --ping-host 192.168.1.100,192.168.1.101

Perform a recovery. **--host** is in character string format.

.. code-block::

   gds_check -t fix --host 192.168.1.100,192.168.1.101

Run the following command to perform the check, print the detailed information, and save the information to logs:

.. code-block::

   gds_check -t check --host 192.168.1.100 --detail

Run the following command to perform the repair, print the detailed information, and save the information to logs:

.. code-block::

   gds_check -t fix --host 192.168.1.100 --detail
