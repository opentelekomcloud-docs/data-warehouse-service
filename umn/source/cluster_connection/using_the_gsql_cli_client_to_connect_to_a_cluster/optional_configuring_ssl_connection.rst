:original_name: dws_01_0076.html

.. _dws_01_0076:

(Optional) Configuring SSL Connection
=====================================

GaussDB(DWS) supports connections in SSL authentication mode so that data transmitted between the GaussDB(DWS) client and the database can be encrypted. The SSL mode delivers higher security than the common mode. By default, the SSL function is enabled in a cluster to allow SSL or non-SSL connections from the client. For security purposes, you are advised to enable SSL connection. If you want to use SSL connection, enable **Require SSL Connection** for the cluster.

On the **Security Settings** page of the cluster, you can enable or disable **Require SSL Connection**.

.. note::

   -  After you have changed the security setting parameters and the settings take effect, the cluster may be restarted, which makes the cluster unavailable temporarily.
   -  To modify the cluster's security configuration, ensure that the following conditions are met:

      -  The cluster status is **Available** or **Unbalanced**.
      -  The value of **Task Information** cannot be **Creating snapshot**, **Resizing**, **Configuring**, or **Restarting**.

The following parts are included in this section:

-  :ref:`Configuring SSL Connection <en-us_topic_0000001134400700__section478703071283>`
-  :ref:`Combinations of SSL Connection Parameters on the Client and Server <en-us_topic_0000001134400700__section1916311515557>`

.. _en-us_topic_0000001134400700__section478703071283:

Configuring SSL Connection
--------------------------

#. Log in to the GaussDB(DWS) management console.

#. In the navigation pane on the left, click **Clusters**.

#. In the cluster list, click the name of a cluster. On the page that is displayed, click **Security Settings**.

   By default, **Configuration Status** is set to **Synchronized**, which indicates that the latest database result is displayed.

#. In the **SSL Connection** area, enable **Require SSL Connection** (recommended).

   |image1| indicates that the server requires SSL connection.

   |image2| indicates that no SSL connection is required (default).


   .. figure:: /_static/images/en-us_image_0000001134400860.png
      :alt: **Figure 1** SSL connection

      **Figure 1** SSL connection

   .. note::

      -  If the gsql client or ODBC driver provided by GaussDB(DWS) is used, GaussDB(DWS) supports the TLSv1.2 SSL protocol.
      -  If the JDBC driver provided by GaussDB(DWS) is used, GaussDB(DWS) supports SSL protocols, such as SSLv3, TLSv1, TLSv1.1, and TLSv1.2. The SSL protocol used between the client and the database depends on the Java Development Kit (JDK) version used by the client. Generally, JDK supports multiple SSL protocols.

#. Click **Apply**.

   The system automatically saves the SSL connection settings. On the **Security Settings** page, **Configuration Status** is **Applying**. After **Configuration Status** changes to **Synchronized**, the settings have been saved and taken effect.

.. _en-us_topic_0000001134400700__section1916311515557:

Combinations of SSL Connection Parameters on the Client and Server
------------------------------------------------------------------

Whether the client uses the SSL encryption connection mode and whether to verify the server certificate depend on client parameter **sslmode** and server (cluster) parameters **ssl** and **require_ssl**. The parameters are described as follows:

-  **ssl (Server)**

   The **ssl** parameter indicates whether to enable the SSL function. **on** indicates that the function is enabled, and **off** indicates that the function is disabled.

   -  The default value is **on** for clusters whose version is 1.3.1 or later, and you cannot set this parameter on the GaussDB(DWS) management console.
   -  For clusters whose version is earlier than 1.3.1, the default value is **on**. You can set this parameter in the **SSL Connection** area on the cluster's **Security Settings** page of the GaussDB(DWS) management console.

-  **require_ssl (Server)**

   The **require_ssl** parameter specifies whether the server forcibly requires SSL connection. This parameter is valid only when **ssl** is set to **on**. **on** indicates that the server forcibly requires SSL connection. **off** indicates that the server does not require SSL connection.

   -  The default value is **off** for clusters whose version is 1.3.1 or later. You can set the **require_ssl** parameter in the **Require SSL Connection** area of the cluster's **Security Settings** page on the GaussDB(DWS) management console.
   -  For clusters whose version is earlier than 1.3.1, the default value is **off**, and you cannot set this parameter on the GaussDB(DWS) management console.

-  **sslmode (Client)**

   You can set this parameter in the SQL client tool.

   -  In the gsql command line client, this parameter is the **PGSSLMODE** parameter.
   -  On the Data Studio client, this parameter is the **SSL Mode** parameter.

The combinations of client parameter **sslmode** and server parameters **ssl** and **require_ssl** are as follows.

.. table:: **Table 1** Combinations of SSL connection parameters on the client and server

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

.. |image1| image:: /_static/images/en-us_image_0000001180320287.png
.. |image2| image:: /_static/images/en-us_image_0000001180440227.jpg
