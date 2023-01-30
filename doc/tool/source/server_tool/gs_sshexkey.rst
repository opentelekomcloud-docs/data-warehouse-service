:original_name: dws_07_0108.html

.. _dws_07_0108:

gs_sshexkey
===========

Context
-------

During cluster installation, you need to execute commands and transfer files among hosts in the cluster. Therefore, mutual trust relationships must be established among the hosts before the installation. **gs_sshexkey**, provided by GaussDB(DWS), helps you establish such relationships.

.. important::

   The mutual trust relationships among **root** users have security risks. You are advised to delete the mutual trust relationships among **root** users once operations completed.

Prerequisites
-------------

-  The SSH service has been enabled.

-  You have verified that SSH ports will not be disabled by firewalls.

-  Each host name and IP address have been correctly configured in the XML file.

-  Communication among all the hosts must be normal.

-  If the mutual trust relationship is to be established for common users, the same user needs to be created and password set on each host.

-  If the SELinux OS has been installed and started on each host, the security context of the **/root** directory needs to be set to the default value **system_u:object_r:home_root_t:s0** and that of the **/home** directory set to the default value **system_u:object_r:admin_home_t:s0**.

   To check whether the SELinux OS has been installed and started, run the **getenforce** command. If the command output is **Enforcing**, the SELinux OS has been installed and started.

   To check the security contexts of the directories, run the following commands:

   .. code-block::

      ls -ldZ  /root | awk '{print $4}'

   .. code-block::

      ls -ldZ  /home | awk '{print $4}'

   To restore the security contexts of the directories, run the following commands:

   .. code-block::

      restorecon -r -vv /home/

   .. code-block::

      restorecon -r -vv /root/

Syntax
------

-  Establish mutual trust relationships.

   .. code-block::

      gs_sshexkey -f HOSTFILE [-W PASSWORD] [...] [--skip-hostname-set] [-l LOGFILE]

-  Show help information.

   .. code-block::

      gs_sshexkey -? | --help

-  Show version number.

   .. code-block::

      gs_sshexkey -V | --version

Parameter Description
---------------------

-  -f

   Lists the IP addresses of all the hosts among which mutual trust relationships need to be established.

   .. note::

      Ensure that **hostfile** contains only correct IP addresses and no other information.

-  -W, --password=PASSWORD

   Specifies the password of the user who will establish mutual trust relationships. If this parameter is not specified, you will be prompted to enter the password when the mutual trust relationship is established. If the password of each host is different, you need to specify multiple **-W** parameters. The password sequence must correspond to the IP address sequence. In interactive mode, you need to enter the password of the host in sequence.

   .. note::

      The password cannot contain the following characters: ;'$

-  -l

   Specifies the path of saving log files.

   Value range: any existing, accessible absolute path

-  --skip-hostname-set

   Specifies whether to write the mapping relationship between the host name and IP address of the **-f** parameter file to the **/etc/hosts** file. If this parameter is specified, the relationship is not written to the file.

-  -?, --help

   Shows help information.

-  -V, --version

   Shows version number.

Examples
--------

The following examples describe how to establish mutual trust relationships for user **root**:

-  In non-interactive mode, if the user passwords are the same, run the following commands to establish mutual trust.

   **Gauss@123** indicates the password of user **root**.

   .. code-block::

      ./gs_sshexkey -f /opt/software/hostfile -W Gauss@123
      Checking network information.
      All nodes in the network are Normal.
      Successfully checked network information.
      Creating SSH trust.
      Creating the local key file.
      Appending local ID to authorized_keys.
      Successfully appended local ID to authorized_keys.
      Updating the known_hosts file.
      Successfully updated the known_hosts file.
      Appending authorized_key on the remote node.
      Successfully appended authorized_key on all remote node.
      Checking common authentication file content.
      Successfully checked common authentication content.
      Distributing SSH trust file to all node.
      Successfully distributed SSH trust file to all node.
      Verifying SSH trust on all hosts.
      Successfully verified SSH trust on all hosts.
      Successfully created SSH trust.

-  In non-interactive mode, if the user passwords are different, run the following commands to establish mutual trust.

   **Gauss@234** indicates the **root** password of the first host in the host list, and **Gauss@345** indicates the **root** password of the second host in the host list.

   .. code-block::

      ./gs_sshexkey -f /opt/software/hostfile -W Gauss@123 -W Gauss@234 -W Gauss@345
      Checking network information.
      All nodes in the network are Normal.
      Successfully checked network information.
      Creating SSH trust.
      Creating the local key file.
      Appending local ID to authorized_keys.
      Successfully appended local ID to authorized_keys.
      Updating the known_hosts file.
      Successfully updated the known_hosts file.
      Appending authorized_key on the remote node.
      Successfully appended authorized_key on all remote node.
      Checking common authentication file content.
      Successfully checked common authentication content.
      Distributing SSH trust file to all node.
      Successfully distributed SSH trust file to all node.
      Verifying SSH trust on all hosts.
      Successfully verified SSH trust on all hosts.
      Successfully created SSH trust.

-  In interactive mode, if the user passwords are the same, run the following commands to establish mutual trust.

   .. code-block::

      gs_sshexkey -f /opt/software/hostfile
      Please enter password for current user[root].
      Password:
      Checking network information.
      All nodes in the network are Normal.
      Successfully checked network information.
      Creating SSH trust.
      Creating the local key file.
      Appending local ID to authorized_keys.
      Successfully appended local ID to authorized_keys.
      Updating the known_hosts file.
      Successfully updated the known_hosts file.
      Appending authorized_key on the remote node.
      Successfully appended authorized_key on all remote node.
      Checking common authentication file content.
      Successfully checked common authentication content.
      Distributing SSH trust file to all node.
      Successfully distributed SSH trust file to all node.
      Verifying SSH trust on all hosts.
      Successfully verified SSH trust on all hosts.
      Successfully created SSH trust.

-  In interactive mode, if the user passwords are different, run the following commands to establish mutual trust.

   .. code-block::

      gs_sshexkey -f /opt/software/hostfile
      Please enter password for current user[root].
      Password:
      Notice :The password of some nodes is incorrect.
      Please enter password for current user[root] on the node[10.180.10.112].
      Password:
      Please enter password for current user[root] on the node[10.180.10.113].
      Password:
      Checking network information.
      All nodes in the network are Normal.
      Successfully checked network information.
      Creating SSH trust.
      Creating the local key file.
      Appending local ID to authorized_keys.
      Successfully appended local ID to authorized_keys.
      Updating the known_hosts file.
      Successfully updated the known_hosts file.
      Appending authorized_key on the remote node.
      Successfully appended authorized_key on all remote node.
      Checking common authentication file content.
      Successfully checked common authentication content.
      Distributing SSH trust file to all node.
      Successfully distributed SSH trust file to all node.
      Verifying SSH trust on all hosts.
      Successfully verified SSH trust on all hosts.
      Successfully created SSH trust.
