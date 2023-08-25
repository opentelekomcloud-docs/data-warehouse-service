:original_name: DWS_DS_154.html

.. _DWS_DS_154:

SSL Certificates
================

.. important::

   The information on using SSL certificates is for reference only. For details on the certificates and for security guidelines for managing the certificates and related files, refer to the database server documentation.

Data Studio can connect to the database using the Secure Sockets Layer [SSL] option. :ref:`Adding a Connection <dws_ds_34>` lists the files required.

= ====================== =========================================
# Certificate/Key        Description
= ====================== =========================================
1 Client SSL Certificate Provided by System/Database Administrator
2 Client SSL Key         Provided by System/Database Administrator
3 Root Certificate       Provided by System/Database Administrator
= ====================== =========================================

Server Configuration
--------------------

After a GaussDB(DWS) cluster is deployed, the SSL authentication mode is enabled by default. The server certificate, private key, and root certificate have been configured by default.

SSL Certificate and Client Configuration
----------------------------------------

You need to configure the client.

#. You can download an SSL certificate from GaussDB(DWS).

   Log in to the GaussDB(DWS) management console. In the navigation pane, choose **Connections**. In the **Driver** area, click **download an SSL certificate**.

   |image1|

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
      | SSL Password                      | SSL key password in PK8 format on the client.                                                                                                                                                 |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | SSL Mode                          | GaussDB(DWS) supports the following SSL modes:                                                                                                                                                |
      |                                   |                                                                                                                                                                                               |
      |                                   | -  **require**: The SSL factory does not require verification, nor does it check the certificate validity.                                                                                    |
      |                                   | -  **verify-ca**: The SSL factory checks whether the CA is correct.                                                                                                                           |
      |                                   | -  **verify-full**: The SSL factory checks whether the CA and database are correct.                                                                                                           |
      |                                   |                                                                                                                                                                                               |
      |                                   | GaussDB(DWS) does not support the **verify-full** mode.                                                                                                                                       |
      +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

   .. note::

      -  Selecting **Client SSL Certificate** and **Client SSL Key** ensures secured connection for export of DDL and data using Data Studio.
      -  Selecting invalid file for Client SSL Certificate and/or Client SSL Key will result in export failure. For details, see :ref:`Troubleshooting <dws_ds_145>`.
      -  If you deselect **Enable SSL** check box and proceed, then **Connection Security Alert** dialog box is displayed. Refer to :ref:`Security Disclaimer <en-us_topic_0000001234042179__en-us_topic_0185264975_section8272203611226>` for information to display this security alert or not.

         -  **Continue**: Continues to use insecure connections.
         -  **Cancel**: Enables SSL.
         -  **Do not show again**: If you select this option, the **Connection Security Alert** dialog box is not displayed for the subsequent connections of logged Data Studio instances.

      -  Data Studio prompts you to enter the client key upon the first access to the **gs_dump** feature.


   .. figure:: /_static/images/en-us_image_0000001579758605.png
      :alt: **Figure 1** SSL parameters

      **Figure 1** SSL parameters

.. |image1| image:: /_static/images/en-us_image_0000001529038632.png
