:original_name: dws_gsql_011.html

.. _dws_gsql_011:

Establishing Secure TCP/IP Connections in SSL Mode
==================================================

GaussDB(DWS) supports the standard SSL. As a highly secure protocol, SSL authenticates bidirectional identification between the server and client using digital signatures and digital certificates to ensure secure data transmission. To support SSL connection, GaussDB(DWS) has obtained the formal certificates and keys for the server and client from the CA certification center. It is assumed that the key and certificate for the server are **server.key** and **server.crt** respectively; the key and certificate for the client are **client.key** and **client.crt** respectively, and the name of the CA root certificate is **cacert.pem**.

The SSL connection mode is more secure. By default, the SSL feature in a cluster allows SSL and non-SSL connections from the client. For security purposes, you are advised to connect to the cluster via SSL from the client. Ensure the certificate, private key, and root certificate of the GaussDB(DWS) server have been configured by default. To forcibly use an SSL connection, configure the **require_ssl** parameter in the **Require SSL Connection** area of the cluster's **Security Settings** page on the GaussDB(DWS) console. Require SSL Connection on the **Security Settings** page of the cluster. For more information, see :ref:`Configuring SSL Connection <en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_section131774823014>` and :ref:`Combinations of SSL Connection Parameters on the Client and Server <en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_section1916311515557>`.

The client or JDBC/ODBC driver needs to use SSL connection. Configure related SSL connection parameters in the client or application code. The GaussDB(DWS) console provides the SSL certificate required by the client. The SSL certificate contains the default certificate, private key, root certificate, and private key password encryption file required by the client. Download the SSL certificate to the host where the client is installed, and specify the path of the certificate on the client. For more information, see :ref:`Configuring Digital Certificate Parameters Related to SSL Authentication on the gsql Client <en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_s6d3b0bb119894929810147678d9c67a5>` and :ref:`SSL Authentication Modes and Client Parameters <en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_s3a228fb4ac9c48ec8bc34e812c8879e8>`.

.. note::

   Using the default certificate may pose security risks. To improve system security, you are advised to periodically change the certificate to prevent password cracking. If you need to replace the certificate, contact the database customer service.

.. _en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_section131774823014:

Configuring SSL Connection
--------------------------

**Prerequisites**

-  Changes made to security configuration parameters require a cluster restart to take effect. Otherwise, the cluster will be temporarily unavailable.

-  To modify the cluster's security configuration, ensure that the following conditions are met:

   -  The cluster status is **Available**, **To be restarted**, or **Unbalanced**.
   -  The **Task Information** cannot be set to **Creating snapshot**, **Scaling out**, **Configuring**, or **Restarting**.

**Procedure**

#. Log in to the GaussDB(DWS) console.

#. Choose **Dedicated Clusters** > **Clusters** in the navigation pane.

#. In the cluster list, click the name of a cluster. On the page that is displayed, click **Security Settings**.

   By default, **Configuration Status** is **Synchronized**, which indicates that the latest database result is displayed.

#. In the **SSL Connection** area, enable **Require SSL Connection** (recommended).

   |image1| indicates the function is enabled. The **require_ssl** is set to **1**, indicating that the server forcibly requires the SSL connection.

   |image2| indicates the function is disabled (default value). The **require_ssl** parameter is set to **0**, indicating that the server does not require SSL connections. For how to configure the **require_ssl** parameter, see :ref:`require_ssl (Server) <en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_li107621516191913>`.

   .. note::

      -  If the gsql client or ODBC driver provided by GaussDB(DWS) is used, GaussDB(DWS) supports the TLSv1.2 SSL protocol.
      -  If the JDBC driver provided by GaussDB(DWS) is used, GaussDB(DWS) supports SSL protocols, such as SSLv3, TLSv1, TLSv1.1, and TLSv1.2. The SSL protocol used between the client and the database depends on the Java Development Kit (JDK) version used by the client. Generally, JDK supports multiple SSL protocols.

#. Click **Apply**.

   The system automatically saves the SSL connection settings. On the **Security Settings** page, **Configuration Status** is **Applying**. After **Configuration Status** changes to **Synchronized**, the settings have been saved and taken effect.

.. _en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_s6d3b0bb119894929810147678d9c67a5:

Configuring Digital Certificate Parameters Related to SSL Authentication on the gsql Client
-------------------------------------------------------------------------------------------

After a data warehouse cluster is deployed, the SSL authentication mode is enabled by default. The server certificate, private key, and root certificate have been configured by default. You need to configure the client parameters.

#. Log in to the GaussDB(DWS) console. In the navigation pane, choose **Client Connections**.

#. In the **Driver** area, click **download an SSL certificate**.

#. Use a file transfer tool (such as WinSCP) to upload the SSL certificate to the host where the client is installed.

   For example, save the downloaded certificate **dws_ssl_cert.zip** to the **/home/dbadmin/dws_ssl/** directory.

#. Use an SSH remote connection tool (such as PuTTY) to log in to the host where the gsql client is installed and run the following commands to go to the directory where the SSL certificate is stored and decompress the SSL certificate:

   .. code-block::

      cd /home/dbadmin/dws_ssl/
      unzip dws_ssl_cert.zip

#. Run the export command and configure digital certificate parameters related to SSL authentication on the host where the gsql client is installed.

   There are two SSL authentication modes: bidirectional authentication and unidirectional authentication. The client environment variables to be configured vary according to the authentication mode. For details, see :ref:`SSL Authentication Modes and Client Parameters <en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_s3a228fb4ac9c48ec8bc34e812c8879e8>`.

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

.. _en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_s3a228fb4ac9c48ec8bc34e812c8879e8:

SSL Authentication Modes and Client Parameters
----------------------------------------------

There are two SSL authentication modes: bidirectional authentication and unidirectional authentication. :ref:`Table 1 <en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_table267519441727>` shows the differences between these two modes. You are advised to use bidirectional authentication for security purposes.

.. _en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_table267519441727:

.. table:: **Table 1** Authentication modes

   +--------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Authentication Mode                        | Description                                                                                                                                                                                                                                                         | Environment Variables Configured on a Client | Maintenance                                                                                                                                                                                                                                 |
   +============================================+=====================================================================================================================================================================================================================================================================+==============================================+=============================================================================================================================================================================================================================================+
   | Bidirectional authentication (recommended) | The client verifies the server's certificate and the server verifies the client's certificate. The connection can be set up only after the verifications are successful.                                                                                            | Set the following environment variables:     | This authentication mode is applicable to scenarios that require high data security. When using this mode, you are advised to set the **PGSSLMODE** client variable to **verify-ca** for network data security purposes.                    |
   |                                            |                                                                                                                                                                                                                                                                     |                                              |                                                                                                                                                                                                                                             |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLCERT                                 |                                                                                                                                                                                                                                             |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLKEY                                  |                                                                                                                                                                                                                                             |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLROOTCERT                             |                                                                                                                                                                                                                                             |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLMODE                                 |                                                                                                                                                                                                                                             |
   +--------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Unidirectional authentication              | The client verifies the server's certificate, whereas the server does not verify the client's certificate. The server loads the certificate information and sends it to the client. The client verifies the server's certificate according to the root certificate. | Set the following environment variables:     | To prevent TCP-based security attacks, you are advised to use the SSL certificate authentication. In addition to configuring the client root certificate, you are advised to set the **PGSSLMODE** variable to **verify-ca** on the client. |
   |                                            |                                                                                                                                                                                                                                                                     |                                              |                                                                                                                                                                                                                                             |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLROOTCERT                             |                                                                                                                                                                                                                                             |
   |                                            |                                                                                                                                                                                                                                                                     | -  PGSSLMODE                                 |                                                                                                                                                                                                                                             |
   +--------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Configure environment variables related to SSL authentication on the client. For details, see :ref:`Table 2 <en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_t8b0644779e4c40009b6fb1ad6a8ea986>`.

.. note::

   The path of environment variables is set to */home/dbadmin*\ **/dws_ssl/** as an example. Replace it with the actual path.

.. _en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_t8b0644779e4c40009b6fb1ad6a8ea986:

.. table:: **Table 2** Client parameters

   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Environment Variable  | Description                                                                                                                                                                                 | Value Description                                                                                                                                                                                 |
   +=======================+=============================================================================================================================================================================================+===================================================================================================================================================================================================+
   | PGSSLCERT             | Specifies the certificate files for a client, including the public key. Certificates prove the legal identity of the client and the public key is sent to the peer end for data encryption. | The absolute path of the files must be specified, for example:                                                                                                                                    |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             | .. code-block::                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             |    export PGSSLCERT='/home/dbadmin/dws_ssl/sslcert/client.crt'                                                                                                                                    |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             | (No default value)                                                                                                                                                                                |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PGSSLKEY              | Specifies the private key file for the client to decrypt digital signatures and data encrypted using the public key.                                                                        | The absolute path of the files must be specified, for example:                                                                                                                                    |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             | .. code-block::                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             |    export PGSSLKEY='/home/dbadmin/dws_ssl/sslcert/client.key'                                                                                                                                     |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             | (No default value)                                                                                                                                                                                |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PGSSLMODE             | Specifies whether to negotiate with the server about SSL connection and specifies the priority of the SSL connection.                                                                       | Values and meanings:                                                                                                                                                                              |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             | -  **disable**: only tries to establish a non-SSL connection.                                                                                                                                     |
   |                       |                                                                                                                                                                                             | -  **allow**: tries to establish a non-SSL connection first, and then an SSL connection if the first attempt fails.                                                                               |
   |                       |                                                                                                                                                                                             | -  **prefer**: tries to establish an SSL connection first, and then a non-SSL connection if the first attempt fails.                                                                              |
   |                       |                                                                                                                                                                                             | -  **require**: only tries to establish an SSL connection. If there is a CA file, perform the verification according to the scenario in which the parameter is set to **verify-ca**.              |
   |                       |                                                                                                                                                                                             | -  **verify-ca**: tries to establish an SSL connection and check whether the server certificate is issued by a trusted CA.                                                                        |
   |                       |                                                                                                                                                                                             | -  **verify-full**: GaussDB(DWS) does not support this mode.                                                                                                                                      |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             | Default value: **prefer**                                                                                                                                                                         |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             | .. note::                                                                                                                                                                                         |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             |    When an external client accesses a cluster, the error message "ssl SYSCALL error" is displayed on some nodes. In this case, run **export PGSSLMODE="allow"** or **export PGSSLMODE="prefer"**. |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PGSSLROOTCERT         | Specifies the root certificate file for issuing client certificates. The root certificate is used to verify the server certificate.                                                         | The absolute path of the files must be specified, for example:                                                                                                                                    |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             | .. code-block::                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             |    export PGSSLROOTCERT='/home/dbadmin/dws_ssl/sslcert/certca.pem'                                                                                                                                |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             | Default value: null                                                                                                                                                                               |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | PGSSLCRL              | Specifies the certificate revocation list file, which is used to check whether a server certificate is in the list. If the certificate is in the list, it is invalid.                       | The absolute path of the files must be specified, for example:                                                                                                                                    |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             | .. code-block::                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             |    export PGSSLCRL='/home/dbadmin/dws_ssl/sslcert/sslcrl-file.crl'                                                                                                                                |
   |                       |                                                                                                                                                                                             |                                                                                                                                                                                                   |
   |                       |                                                                                                                                                                                             | Default value: null                                                                                                                                                                               |
   +-----------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_section1916311515557:

Combinations of SSL Connection Parameters on the Client and Server
------------------------------------------------------------------

Whether the client uses the SSL encryption connection mode and whether to verify the server certificate depend on client parameter **sslmode** and server (cluster) parameters **ssl** and **require_ssl**. The parameters are as follows:

-  **ssl (Server)**

   The **ssl** parameter indicates whether to enable the SSL function. **on** indicates that the function is enabled, and **off** indicates that the function is disabled.

   -  The default value is **on** for clusters whose version is 1.3.1 or later, and you cannot set this parameter on the GaussDB(DWS) console.
   -  For clusters whose version is earlier than 1.3.1, the default value is **on**. You can set this parameter in the **SSL Connection** area on the cluster's **Security Settings** page of the GaussDB(DWS) management console.

-  .. _en-us_topic_0000001813438712__en-us_topic_0000001487341076_en-us_topic_0000001372520154_li107621516191913:

   **require_ssl (Server)**

   The **require_ssl** parameter specifies whether the server forcibly requires SSL connection. This parameter is valid only when **ssl** is set to **on**. **on** indicates that the server forcibly requires SSL connection. **off** indicates that the server does not require SSL connection.

   -  The default value is **off** for clusters whose version is 1.3.1 or later. You can set the **require_ssl** parameter in the **Require SSL Connection** area of the cluster's **Security Settings** page on the GaussDB(DWS) console.
   -  For clusters whose version is earlier than 1.3.1, the default value is **off**, and you cannot set this parameter on the GaussDB(DWS) console.

-  **sslmode (Client)**

   You can set this parameter in the SQL client tool.

   -  In the gsql command line client, this parameter is the **PGSSLMODE** parameter.
   -  On the Data Studio client, this parameter is the **SSL Mode** parameter.

The combinations of client parameter **sslmode** and server parameters **ssl** and **require_ssl** are as follows.

.. table:: **Table 3** Combinations of SSL connection parameters on the client and server

   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   | ssl (Server) | sslmode (Client) | require_ssl (Server) | Result                                                                                                                 |
   +==============+==================+======================+========================================================================================================================+
   | on           | disable          | on                   | The server requires SSL, but the client disables SSL for the connection. As a result, the connection cannot be set up. |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | disable          | off                  | The connection is not encrypted.                                                                                       |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | allow            | on                   | The connection is encrypted.                                                                                           |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | allow            | off                  | The connection is not encrypted.                                                                                       |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | prefer           | on                   | The connection is encrypted.                                                                                           |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | prefer           | off                  | The connection is encrypted.                                                                                           |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | require          | on                   | The connection is encrypted.                                                                                           |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | require          | off                  | The connection is encrypted.                                                                                           |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | verify-ca        | on                   | The connection is encrypted and the server certificate is verified.                                                    |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | verify-ca        | off                  | The connection is encrypted and the server certificate is verified.                                                    |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   | off          | disable          | on                   | The connection is not encrypted.                                                                                       |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | disable          | off                  | The connection is not encrypted.                                                                                       |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | allow            | on                   | The connection is not encrypted.                                                                                       |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | allow            | off                  | The connection is not encrypted.                                                                                       |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | prefer           | on                   | The connection is not encrypted.                                                                                       |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | prefer           | off                  | The connection is not encrypted.                                                                                       |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | require          | on                   | The client requires SSL, but SSL is disabled on the server. Therefore, the connection cannot be set up.                |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | require          | off                  | The client requires SSL, but SSL is disabled on the server. Therefore, the connection cannot be set up.                |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | verify-ca        | on                   | The client requires SSL, but SSL is disabled on the server. Therefore, the connection cannot be set up.                |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+
   |              | verify-ca        | off                  | The client requires SSL, but SSL is disabled on the server. Therefore, the connection cannot be set up.                |
   +--------------+------------------+----------------------+------------------------------------------------------------------------------------------------------------------------+

.. |image1| image:: /_static/images/en-us_image_0000002239145517.png
.. |image2| image:: /_static/images/en-us_image_0000002204065540.png
