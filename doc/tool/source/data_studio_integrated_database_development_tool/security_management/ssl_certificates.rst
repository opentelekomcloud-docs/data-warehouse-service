:original_name: DWS_DS_154.html

.. _DWS_DS_154:

SSL Certificates
================

.. important::

   The information about using SSL certificates is for reference only. For details about the certificates and the security guidelines for managing the certificates and related files, see the database server documentation.

Data Studio can connect to the database using the Secure Sockets Layer (SSL) option. :ref:`Adding a Connection <dws_ds_34>` lists the files required.

= ====================== =============================================
# Certificate/Key        Description
= ====================== =============================================
1 Client SSL Certificate Provided by the system/database administrator
2 Client SSL Key         Provided by the system/database administrator
3 Root Certificate       Provided by the system/database administrator
= ====================== =============================================

SSL Certificate Generation and Server Configuration
---------------------------------------------------

Perform the following steps to generate a certificate:

#. **Establish a Certificate Authority (CA) environment**: Assume that user **omm** has been created and the CA path is **test**.

   Log in to SUSE Linux as user **root** and switch to user **omm**.

   Run the following commands:

   .. code-block::

      mkdir test
      cd /etc/ssl

   Copy the configuration file **openssl.cnf** to the **test** directory.

   Run the following commands:

   .. code-block::

      cp openssl.cnf ~/test
      cd ~/test

   Establish the CA environment under the **test** folder.

   Create a folder in the **demoCA./demoCA/newcerts./demoCA/private** directory.

   Run the following commands:

   .. code-block::

      mkdir ./demoCA ./demoCA/newcerts ./demoCA/private
      chmod 777 ./demoCA/private

   Create the **serial** file and write it to **01**.

   Run the following command:

   .. code-block::

      echo '01'>./demoCA/serial

   Create the **index.txt** file.

   Run the following command:

   .. code-block::

      touch /home/omm/test/demoCA/index.txt

   Modify parameters in the **openssl.cnf** configuration file.

   Run the following commands:

   .. code-block::

      dir = /home/omm/test/demoCA
      default_md = sha256

   The CA environment has been established.

#. **Generate a root private key**: Generate a CA private key.

   Run the following command:

   .. code-block::

      openssl genrsa -aes256 -out demoCA/private/cakey.pem 2048

   A 2048-bit RSA private key is generated.

#. **Generate a root certificate request file**: The root certificate application file name is **server.req**.

   Run the following commands:

   .. code-block::

      openssl req -config openssl.cnf -new -key demoCA/private/cakey.pem -out demoCA/careq.pem

   Enter the password of **demoCA/private/cakey.pem**.

   Enter the private key password of user **root**.

   You need to enter information that will be included in your certificate request.

   The information you need to enter is a Distinguished Name (DN).

   You can leave some fields blank.

   For a field that contains a default value, enter a period (.) to leave the field blank. Enter the following information in the generated server certificate and client certificate.

   .. code-block::

      Country Name (2 letter code) [AU]:CN
      State or Province Name (full name) [Some-State]:shanxi
      Locality Name (eg, city) []:xian
      Organization Name (eg, company) [Internet Widgits Pty Ltd]:Abc
      Organizational Unit Name (eg, section) []:hello
      -Common name can be any name
      Common Name (eg, YOUR name) []:world
      -Email is optional.
      Email Address []:
      A challenge password []:
      An optional company name []:

#. **Generate a self-signed root certificate.**

   Run the following command:

   .. code-block::

      openssl ca -config openssl.cnf -out demoCA/cacert.pem -keyfile demoCA/private/cakey.pem -selfsign -infiles demoCA/careq.pem

   Use the configurations of **openssl.cnf**.

   Enter the password of **demoCA/private/cakey.pem**.

   Enter the private key password of user **root**.

   Check whether the request matches the signature.

   .. code-block::

      Signature ok
      Certificate Details:
      Serial Number: 1 (0x1)
      Validity
      Not Before: Feb 28 02:17:11 2017 GMT
      Not After : Feb 28 02:17:11 2018 GMT
      Subject:
      countryName = CN
      stateOrProvinceName = shanxi
      organizationName = Abc
      organizationalUnitName = hello
      commonName = world
      X509v3 extensions:
      X509v3 Basic Constraints:
      CA:FALSE
      Netscape Comment:
      OpenSSL Generated Certificate
      X509v3 Subject Key Identifier:
      F9:91:50:B2:42:8C:A8:D3:41:B0:E4:42:CB:C2:BE:8D:B7:8C:17:1F
      X509v3 Authority Key Identifier:
      keyid:F9:91:50:B2:42:8C:A8:D3:41:B0:E4:42:CB:C2:BE:8D:B7:8C:17:1F
      Certificate is to be certified until Feb 28 02:17:11 2018 GMT (365 days)
      Sign the certificate? [y/n]:y
      1 out of 1 certificate requests certified, commit? [y/n]y
      Write out database with 1 new entries
      Data Base Updated

   A CA root certificate named **demoCA/cacert.pem** has been issued.

#. **Generate a server certificate private key**: Generate the private key file **server.key**.

   Run the following command:

   .. code-block::

      openssl genrsa -aes256 -out server.key 2048

#. **Generate a server certificate request file**: Generate the server certificate request file **server.req**.

   Run the following command:

   .. code-block::

      openssl req -config openssl.cnf -new -key server.key -out server.req

   Enter the password of **server.key**.

   You need to enter information that will be included in your certificate request.

   The information you need to enter is a Distinguished Name (DN).

   You can leave some fields blank.

   For a field that contains a default value, enter a period (.) to leave the field blank.

   Configure the following information and ensure that the configuration is the same as that upon CA creation.

   .. code-block::

      Country Name (2 letter code) [AU]:CN
      State or Province Name (full name) [Some-State]:shanxi
      Locality Name (eg, city) []:xian
      Organization Name (eg, company) [Internet Widgits Pty Ltd]:Abc
      Organizational Unit Name (eg, section) []:hello
      -Common name can be any name
      Common Name (eg, YOUR name) []:world
      Email Address []:
      -- The following information is optional.
      A challenge password []:
      An optional company name []:

#. **Generate a server certificate**: Set the **demoCA/index.txt.attr** attribute to **no**.

   .. code-block::

      vi demoCA/index.txt.attr

   Issue the generated server certificate request file. After it is issued, the official server certificate **server.crt** is generated.

   .. code-block::

      openssl ca -config openssl.cnf -in server.req -out server.crt -days 3650 -md sha256

   Use the configurations of **/etc/ssl/openssl.cnf**.

   Enter the password of **/demoCA/private/cakey.pem**.

   Check whether the request matches the signature.

   .. code-block::

      Signature ok
      Certificate Details:
      Serial Number: 2 (0x2)
      Validity
      Not Before: Feb 27 10:11:12 2017 GMT
      Not After : Feb 25 10:11:12 2027 GMT
      Subject:
      countryName = CN
      stateOrProvinceName = shanxi
      organizationName = Abc
      organizationalUnitName = hello
      commonName = world
      X509v3 extensions:
      X509v3 Basic Constraints:
      CA:FALSE
      Netscape Comment:
      OpenSSL Generated Certificate
      X509v3 Subject Key Identifier:
      EB:D9:EE:C0:D2:14:48:AD:EB:BB:AD:B6:29:2C:6C:72:96:5C:38:35
      X509v3 Authority Key Identifier:
      keyid:84:F6:A1:65:16:1F:28:8A:B7:0D:CB:7E:19:76:2A:8B:F5:2B:5C:6A
      Certificate is to be certified until Feb 25 10:11:12 2027 GMT (3650 days)
      -- Choose y to sign and issue the certificate.
      Sign the certificate? [y/n]:y
      -- Select y, the certificate singing and issuing is complete.
      1 out of 1 certificate requests certified, commit? [y/n]y
      Write out database with 1 new entries
      Data Base Updated

   **Enable password protection for the private key**: If the password protection for the server private key is not enabled, you need to use gs_guc to encrypt the password.

   .. code-block::

      gs_guc encrypt -M server -K root private key password -D ./

   After the password is encrypted using gs_guc, two private key password protection files **server.key.cipher** and **server.key.rand** are generated.

#. **Generate a client certificate and private key**: Generate a client private key.

   .. code-block::

      openssl genrsa -aes256 -out client.key 2048

   Generate a client certificate request file.

   .. code-block::

      openssl req -config openssl.cnf -new -key client.key -out client.req

   After the generated client certificate request file is signed and issued, the official client certificate **client.crt** will be generated.

   .. code-block::

      openssl ca -config openssl.cnf -in client.req -out client.crt -days 3650 -md sha256

   .. note::

      If **METHOD** is set to **cert** in the **pg_hba.conf** file of the server, the client must use the **username** (common user) configured in the license file **client.crt** to connect to a database. If **METHOD** is set to **md5** or **sha256**, the client does not have this restriction.

   If the password protection for the client private key is not removed, you need to use gs_guc to encrypt the password.

   .. code-block::

      gs_guc encrypt -M client -K root private key password -D ./

   After the password is encrypted using gs_guc, two private key password protection files **client.key.cipher** and **client.key.rand** are generated.

Replacing Certificates
----------------------

Default security certificates and keys required for SSL connection are configured in LibrA. Before the operation, obtain official certificates and keys for the server and client from the CA.

#. Prepare for a certificate and a key. The conventions for configuration file names on the server are as follows:

   .. code-block::

      l Certificate name: server.crt
      l Key name: server.key
      l Key password and encrypted file: server.key.cipher and server.key.rand
      Conventions for configuration file names on the client:
      l Certificate name: client.crt
      l Key name: client.key
      l Key password and encrypted file: client.key.cipher and client.key.rand
      l Certificate name: cacert.pem
      l Names of files on in the revoked certificate list: sslcrl-file.crl

#. Create a package.

   Package name: **db-cert-replacement.zip**

   Package format: ZIP

   Package file list: **server.crt**, **server.key**, **server.key.cipher**, **server.key.rand**, **client.crt**, **client.key**, **client.key.cipher**, **client.key.rand**, and **cacert.pem**

   If you need to configure the certificate revocation list (CRL), the package file list must contain **sslcrl-file.crl**.

   Run the following commands:

   .. code-block::

      zip db-cert-replacement.zip client.crt client.key client.key.cipher client.key.rand server.crt server.key server.key.cipher server.key.rand
      zip -u ../db-cert-replacement.zip cacert.pem

#. Call the certificate replacement interface to replace a certificate. Upload the package **db-cert-replacement.zip** to any path of a cluster user, for example, **/home/gaussdba/test/db-cert-replacement.zip**.

   Run the following command to replace the certificate in Coodinator (CN):

   .. code-block::

      gs_om -t cert --cert-file=/home/gaussdba/test/db-cert-replacement.zip

   Starting SSL cert files replace.

   Backing up old SSL cert files.

   Backup SSL cert files on BLR1000029898 successfully.

   Backup SSL cert files on BLR1000029896 successfully.

   Backup SSL cert files on BLR1000029897 successfully.

   Backup gds SSL cert files on successfully.

   BLR1000029898 replace SSL cert files successfully.

   BLR1000029896 replace SSL cert files successfully.

   BLR1000029897 replace SSL cert files successfully.

   Replace SSL cert files successfully.

   Distribute cert files on all coordinators successfully.

   You can run the **gs_om -t cert --rollback** command to remotely call the interface and the **gs_om -t cert --rollback -L** command.

Client Configuration
--------------------

#. Run the following command on the client key file:

   .. code-block::

      openssl pkcs8 -topk8 -inform PEM -outform DER -in Client.key -out client.pk8

#. Copy the created **client.pk8**, **client.crt**, and **cacert.pem** files to the client.

   .. note::

      When you select **Client SSL Key** on Data Studio, the key file cannot be selected and only the **\*.pk8** file can be selected. However, this file is not included in the downloaded certificate.

#. Configure **Two way** SSL authentication for the client on the server.

   .. code-block::

      hostssl      all           all           10.18.158.95/32        cert

   Configure **One way** SSL authentication for the client on the server.

   .. code-block::

      hostssl      all           all           10.18.158.95/32        sha256

#. During the login to Data Studio, password is not validated in the two-way SSL authentication.

   |image1|

   You need to enter the SSL password.

.. |image1| image:: /_static/images/en-us_image_0000001145833103.png
