:original_name: dws_01_0038.html

.. _dws_01_0038:

Establishing Secure TCP/IP Connections in SSL Mode
==================================================

If the client or JDBC/ODBC driver needs to use SSL connection, you must configure related SSL connection parameters in the client or application code. The GaussDB(DWS) management console provides the SSL certificate required by the client. The SSL certificate contains the default certificate, private key, root certificate, and private key password encryption file required by the client. Download the SSL certificate to the host where the client resides and specify the path of the certificate on the client.

.. note::

   Using the default certificate may pose security risks. To improve system security, you are advised to periodically change the certificate to prevent password cracking. If you need to replace the certificate, contact the database customer service.

For more information about SSL certificates, see :ref:`(Optional) Downloading the SSL Certificate <dws_01_0083>`. The following parts are included in this section:

-  :ref:`Configuring Digital Certificate Parameters Related to SSL Authentication on the gsql Client <en-us_topic_0000001134560524__s6d3b0bb119894929810147678d9c67a5>`
-  :ref:`SSL Authentication Modes and Client Parameters <en-us_topic_0000001134560524__s3a228fb4ac9c48ec8bc34e812c8879e8>`

.. _en-us_topic_0000001134560524__s6d3b0bb119894929810147678d9c67a5:

Configuring Digital Certificate Parameters Related to SSL Authentication on the gsql Client
-------------------------------------------------------------------------------------------

After a data warehouse cluster is deployed, the SSL authentication mode is enabled by default. The server certificate, private key, and root certificate have been configured by default. You need to configure the client parameters.

#. Log in to the GaussDB(DWS) management console and click **Connections** to download the SSL certificate.

   For more information about SSL certificates, see :ref:`(Optional) Downloading the SSL Certificate <dws_01_0083>`.


   .. figure:: /_static/images/en-us_image_0000001134560708.png
      :alt: **Figure 1** Downloading the SSL certificate file

      **Figure 1** Downloading the SSL certificate file

#. Use a file transfer tool (such as WinSCP) to upload the SSL certificate to the host where the client is installed.

   For example, save the downloaded certificate **dws_ssl_cert.zip** to the **/home/dbadmin/dws_ssl/** directory.

#. Use an SSH remote connection tool (such as PuTTY) to log in to the host where the gsql client is installed and run the following commands to go to the directory where the SSL certificate is stored and decompress the SSL certificate:

   .. code-block::

      cd /home/dbadmin/dws_ssl/
      unzip dws_ssl_cert.zip

#. Run the export command and configure digital certificate parameters related to SSL authentication on the host where the gsql client is installed.

   There are two SSL authentication modes: bidirectional authentication and unidirectional authentication. Different authentication modes require different client environment variables. For details, see :ref:`SSL Authentication Modes and Client Parameters <en-us_topic_0000001134560524__s3a228fb4ac9c48ec8bc34e812c8879e8>`.

   The following parameters must be configured for bidirectional authentication:

   .. code-block::

      export PGSSLCERT="/home/dbadmin/dws_ssl/sslcert/client.crt"
      export PGSSLKEY="/home/dbadmin/dws_ssl/sslcert/client.key"
      export PGSSLMODE="verify-ca"
      export PGSSLROOTCERT="/home/dbadmin/dws_ssl/sslcert/cacert.pem"

   The following parameters must be configured for unidirectional authentication:

   .. code-block::

      export PGSSLMODE="verify-ca"
      export PGSSLROOTCERT="/home/dbadmin/dws_ssl/sslcert/cacert.pem"

   .. important::

      -  You are advised to use bidirectional authentication for security purposes.
      -  The environment variables configured for a client must contain the absolute file paths.

#. Change the client private key permissions.

   The permissions on the client's root certificate, private key, certificate, and encrypted private key file must be **600**. If the permissions do not meet the requirement, the client cannot connect to the cluster in SSL mode.

   .. code-block::

      chmod 600 client.key
      chmod 600 client.crt
      chmod 600 client.key.cipher
      chmod 600 client.key.rand
      chmod 600 cacert.pem

.. _en-us_topic_0000001134560524__s3a228fb4ac9c48ec8bc34e812c8879e8:

SSL Authentication Modes and Client Parameters
----------------------------------------------

There are two SSL authentication modes: bidirectional authentication and unidirectional authentication. Table :ref:`Table 1 <en-us_topic_0000001134560524__table267519441727>` shows the differences between these two modes. You are advised to use bidirectional authentication for security purposes.

.. _en-us_topic_0000001134560524__table267519441727:

.. table:: **Table 1** Authentication modes

   +--------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Authentication Mode                        | Description                                                                                                                                                                                                                                                         | Environment Variables Configured on a Client | Maintenance                                                                                                                                                                                                                              |
   +============================================+=====================================================================================================================================================================================================================================================================+==============================================+==========================================================================================================================================================================================================================================+
   | Bidirectional authentication (recommended) | The client verifies the server's certificate and the server verifies the client's certificate. The connection can be set up only after the verifications are successful.                                                                                            | Set the following environment variables:     | This authentication mode is applicable to scenarios that require high data security. When using this mode, you are advised to set the **PGSSLMODE** client variable to **verify-ca** for network data security purposes.                 |
   |                                            |                                                                                                                                                                                                                                                                     |                                              |                                                                                                                                                                                                                                          |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLCERT                                 |                                                                                                                                                                                                                                          |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLKEY                                  |                                                                                                                                                                                                                                          |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLROOTCERT                             |                                                                                                                                                                                                                                          |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLMODE                                 |                                                                                                                                                                                                                                          |
   +--------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Unidirectional authentication              | The client verifies the server's certificate, whereas the server does not verify the client's certificate. The server loads the certificate information and sends it to the client. The client verifies the server's certificate according to the root certificate. | Set the following environment variables:     | To prevent TCP-based link spoofing, you are advised to use the SSL certificate authentication. In addition to configuring the client root certificate, you are advised to set the **PGSSLMODE** variable to **verify-ca** on the client. |
   |                                            |                                                                                                                                                                                                                                                                     |                                              |                                                                                                                                                                                                                                          |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLROOTCERT                             |                                                                                                                                                                                                                                          |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLMODE                                 |                                                                                                                                                                                                                                          |
   +--------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Configure environment variables related to SSL authentication on the client. For details, see :ref:`Table 2 <en-us_topic_0000001134560524__t8b0644779e4c40009b6fb1ad6a8ea986>`.

.. note::

   The path of environment variables is set to */home/dbadmin*\ **/dws_ssl/** as an example. Replace it with the actual path.

.. _en-us_topic_0000001134560524__t8b0644779e4c40009b6fb1ad6a8ea986:

.. table:: **Table 2** Client parameters

   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Environment Variable  | Description                                                                                                                                                                                   | Value Range                                                                                                                                                                          |
   +=======================+===============================================================================================================================================================================================+======================================================================================================================================================================================+
   | PGSSLCERT             | Specifies the certificate files for a client, including the public key. Certificates prove the legal identity of the client and the public key is sent to the remote end for data encryption. | The absolute path of the files must be specified, for example:                                                                                                                       |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               | .. code-block::                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               |    export PGSSLCERT='/home/dbadmin/dws_ssl/sslcert/client.crt'                                                                                                                       |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               | (No default value)                                                                                                                                                                   |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PGSSLKEY              | Specifies the client private key file used to decrypt the digital signatures and the data encrypted using the public key.                                                                     | The absolute path of the files must be specified, for example:                                                                                                                       |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               | .. code-block::                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               |    export PGSSLKEY='/home/dbadmin/dws_ssl/sslcert/client.key'                                                                                                                        |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               | (No default value)                                                                                                                                                                   |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PGSSLMODE             | Specifies whether to negotiate with the server about SSL connection and specifies the priority of the SSL connection.                                                                         | Values and meanings:                                                                                                                                                                 |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               | -  **disable**: only tries to establish a non-SSL connection.                                                                                                                        |
   |                       |                                                                                                                                                                                               | -  **allow**: tries to establish a non-SSL connection first, and then an SSL connection if the first attempt fails.                                                                  |
   |                       |                                                                                                                                                                                               | -  **prefer**: tries to establish an SSL connection first, and then a non-SSL connection if the first attempt fails.                                                                 |
   |                       |                                                                                                                                                                                               | -  **require**: only tries to establish an SSL connection. If there is a CA file, perform the verification according to the scenario in which the parameter is set to **verify-ca**. |
   |                       |                                                                                                                                                                                               | -  **verify-ca**: tries to establish an SSL connection and check whether the server certificate is issued by a trusted CA.                                                           |
   |                       |                                                                                                                                                                                               | -  **verify-full**: GaussDB(DWS) does not support this mode.                                                                                                                         |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               | Default value: **prefer**                                                                                                                                                            |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PGSSLROOTCERT         | Specifies the root certificate file for issuing client certificates. The root certificate is used to verify the server certificate.                                                           | The absolute path of the files must be specified, for example:                                                                                                                       |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               | .. code-block::                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               |    export PGSSLROOTCERT='/home/dbadmin/dws_ssl/sslcert/certca.pem'                                                                                                                   |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               | Default value: null                                                                                                                                                                  |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PGSSLCRL              | Specifies the certificate revocation list file, which is used to check whether a server certificate is in the list. If the certificate is in the list, it is invalid.                         | The absolute path of the files must be specified, for example:                                                                                                                       |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               | .. code-block::                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               |    export PGSSLCRL='/home/dbadmin/dws_ssl/sslcert/sslcrl-file.crl'                                                                                                                   |
   |                       |                                                                                                                                                                                               |                                                                                                                                                                                      |
   |                       |                                                                                                                                                                                               | Default value: null                                                                                                                                                                  |
   +-----------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
