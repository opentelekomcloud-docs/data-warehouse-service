:original_name: DWS_DS_008.html

.. _DWS_DS_008:

Configuring SSL Connection
==========================

Data Studio can connect to the database using the Secure Sockets Layer [SSL] option. To use the SSL connection mode, you must configure related parameters on the client or in the application code. The GaussDB(DWS) management console provides the SSL certificate required by the client. The SSL certificate contains the default certificate, private key, root certificate, and private key password encryption file required by the client.

Server Configuration
--------------------

After a cluster is deployed, GaussDB(DWS) enables the SSL authentication mode by default. The server certificate, private key, and root certificate have been configured.

SSL Certificate and Client Configuration
----------------------------------------

You need to configure the client.

#. You can download an SSL certificate from GaussDB(DWS).

   Log in to the GaussDB(DWS) management console. In the navigation pane, choose **Connections**. In the **Driver** area, click **download an SSL certificate**.

#. Decompress the downloaded **dws_ssl_cert.zip** package to obtain the certificate file. Click the **SSL** tab on the Data Studio client and set the following parameters:

   .. table:: **Table 1** Configuring SSL parameters

      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                                                                                                   |
      +===================================+===============================================================================================================================================================================================+
      | Client SSL Certificate            | Select the **sslcert\\client.crt** file in the decompressed SSL certificate directory.                                                                                                        |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Client SSL Key                    | Only the PK8 format is supported. Select the **sslcert\\client.key.pk8** file in the directory where the SSL certificate is decompressed.                                                     |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Root Certificate                  | When **SSL Mode** is set to **verify-ca** or **verify-full**, the root certificate must be configured. Select the **sslcert\\cacert.pem** file in the decompressed SSL certificate directory. |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | SSL Password                      | Set the password for the client SSL key in PK8 format.                                                                                                                                        |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | SSL Mode                          | GaussDB(DWS) supports the following SSL modes:                                                                                                                                                |
      |                                   |                                                                                                                                                                                               |
      |                                   | -  **require**: The SSL factory does not require verification, nor does it check the certificate validity.                                                                                    |
      |                                   | -  **verify-ca**: The certificate authority (CA) will be verified using the corresponding SSL factory.                                                                                        |
      |                                   | -  **verify-full**: The CA and database will be verified using the corresponding SSL factory.                                                                                                 |
      |                                   |                                                                                                                                                                                               |
      |                                   | GaussDB(DWS) does not support the **verify-full** mode.                                                                                                                                       |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      -  You can select a valid **Client SSL Certificate** and **Client SSL Key** to export DDL and data from Data Studio using secure connections.
      -  If the selected **Client SSL Certificate** and **Client SSL Key** are invalid, the export will fail. For details, see :ref:`Troubleshooting <dws_ds_039>`.
      -  If you deselect **Enable SSL** and proceed, the **Connection Security Alert** dialog box is displayed. Refer to :ref:`Table 1 <en-us_topic_0000001813438860__table1510418570339>` to determine whether to display this dialog box.

         -  **Continue**: Continues to use insecure connections.
         -  **Cancel**: Enables SSL.
         -  **Do not show again**: If you select this option, the **Connection Security Alert** dialog box is not displayed for the subsequent connections of logged Data Studio instances.

      -  Data Studio prompts you to enter the client key upon the first access to the **gs_dump** feature.


   .. figure:: /_static/images/en-us_image_0000001860199137.png
      :alt: **Figure 1** SSL parameters

      **Figure 1** SSL parameters
