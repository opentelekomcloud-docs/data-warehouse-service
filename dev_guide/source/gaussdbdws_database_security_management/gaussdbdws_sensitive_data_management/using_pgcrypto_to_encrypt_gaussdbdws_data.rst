:original_name: dws_04_0993.html

.. _dws_04_0993:

Using pgcrypto to Encrypt GaussDB(DWS) Data
===========================================

GaussDB(DWS) 8.2.0 and later provides a built-in cryptographic module pgcrypto. The pgcrypto module allows database users to store certain columns of data after encryption, enhancing sensitive data security. Users without the encryption key cannot read the encrypted data stored in GaussDB(DWS).

The pgcrypto function runs inside database servers, which means that all data and passwords are transmitted in plaintext between pgcrypto and client applications. For security purposes, you are advised to use the SSL connection between the client and the GaussDB(DWS) server.

The functions in the pgcrypto module are as follows.

General Hash Functions
----------------------

-  digest()

   The digest() function can generate binary hash values by using a specified algorithm. The syntax is as follows:

   .. code-block::

      digest(data text, type text) returns bytea
      digest(data bytea, type text) returns bytea

   **data** indicates the original data, and **type** indicates the encryption algorithm (**md5**, **sha1**, **sha224**, **sha256**, **sha384**, **sha512**, or **sm3**). The return value of the function is a binary string.

   Example:

   Use the digest() function to encrypt the GaussDB(DWS) string using SHA256 for storage.

   .. code-block::

      select digest('GaussDB(DWS)', 'sha256');
                                     digest
      --------------------------------------------------------------------
       \xcc2d1b97c6adfba44bbce7386516f63f16fc6e6a10bd938861d3aba501ac8aab
      (1 row)

-  hmac()

   The hmac() function can calculate the MAC value for data with a key by using a specified algorithm. The syntax is as follows:

   .. code-block::

      hmac(data text, key text, type text) returns bytea
      hmac(data bytea, key bytea, type text) returns bytea

   **data** indicates the original data, **key** indicates the encryption key, and **type** indicates the encryption algorithm (**md5**, **sha1**, **sha224**, **sha256**, **sha384**, **sha512**, or **sm3**). The return value of the function is a binary string.

   Example:

   Use **key123** and the SHA256 algorithm to calculate the MAC value for the string **GaussDB(DWS)**.

   .. code-block::

      select hmac('GaussDB(DWS)', 'key123', 'sha256');
      hmac
      --------------------------------------------------------------------
      \x14e1d9e110e9b11ab8379dc02b49533d50a6f4deafe6d6cd451d06c106c97d83
      (1 row)

   If both the original data and its encryption result are modified, the digest() function cannot identify the changes. The hmac() function can identify the changes as long as the key is not disclosed.

   If the key is longer than the hash block, it will be hashed first, and the hash result will be used as the key.

Cryptographic Hash Functions
----------------------------

The crypt() and gen_salt() functions are used for password hashing. crypt() executes hashes to encrypt data, and gen_salt() generates salted hashes.

The algorithms in crypt() differ from the common MD5 and SHA1 hash algorithms in the following aspects:

-  The algorithms used in crypt() are slow. This is the only way to make it difficult for brute-force attackers to crack passwords, which only contain a small amount of data.
-  A random value (called salt) is used for encryption, so that users will get different ciphertexts even if they use the same passwords. This can protect passwords for cracking algorithms.
-  The encryption results include algorithm types. Passwords can be encrypted using different algorithms for different users.
-  Some of the algorithms are self-adaptive. They can slow down computing if it is too fast, and do not cause incompatibility issues with existing passwords.

The following table lists the algorithms supported by the crypt() function.

.. table:: **Table 1** Algorithms supported by crypt()

   +-----------+-------------------------+--------------+-----------+------------------------+-----------------------------+
   | Algorithm | Maximum Password Length | Adaptability | Salt Bits | Standard Output Length | Description                 |
   +===========+=========================+==============+===========+========================+=============================+
   | bf        | 72                      | Y            | 128       | 60                     | Blowfish-based 2a variation |
   +-----------+-------------------------+--------------+-----------+------------------------+-----------------------------+
   | md5       | unlimited               | x            | 48        | 34                     | MD5-based algorithm         |
   +-----------+-------------------------+--------------+-----------+------------------------+-----------------------------+
   | xdes      | 8                       | Y            | 24        | 20                     | Extended DES                |
   +-----------+-------------------------+--------------+-----------+------------------------+-----------------------------+
   | des       | 8                       | x            | 12        | 13                     | Native UNIX algorithm       |
   +-----------+-------------------------+--------------+-----------+------------------------+-----------------------------+

-  crypt()

   The syntax of crypt() is as follows:

   .. code-block::

      crypt(password text, salt text) returns text

   This function returns a hash value of the password string in crypt(3) format. The salt parameter is generated by the gen_salt() function.

   For the same password, the crypt() function returns a different result each time, because the gen_salt() function generates a different salt each time. During password verification, the previously generated hash result can be used as the salt.

   For example, to set a new password, run the following command:

   .. code-block::

      UPDATE ... SET pswhash = crypt('new password', gen_salt('bf',10));

   The hash values of the entered password and the stored password are compared.

   .. code-block::

      SELECT (pswhash = crypt('entered password', pswhash)) AS pswmatch FROM ... ;

   If the entered password is correct, **true** is returned.

   Example:

   .. code-block::

      create table userpwd(userid int8, pwd text);
      CREATE TABLE

      insert into userpwd values (1, crypt('this is a pwd', gen_salt('bf',10)));
      INSERT 0 1

      select crypt('this is a pwd', pwd)=pwd as result from userpwd where userid =1;
       result
      --------
       t
      (1 row)

      select crypt('this is a wrong pwd', pwd)=pwd as result from userpwd where userid =1;
       result
      --------
       f
      (1 row)

-  gen_salt()

   The gen_salt() function is used to generate random parameters for **crypt**. The syntax is as follows:

   .. code-block::

      gen_salt(type text [, iter_count integer ]) returns text

   This function generates a random salt string each time. The string determines the algorithm used by the **crypt()** function. The **type** parameter specifies a hash algorithm (**des**, **xdes**, **md5**, or **bf**) for generating a string. For the xdes and bf algorithms, **iter_count** indicates the number of iterations. A large value indicates a long encryption or cracking time.

   ::

      SELECT gen_salt('des'), gen_salt('xdes'), gen_salt('md5'), gen_salt('bf');
       gen_salt | gen_salt  |  gen_salt   |           gen_salt
      ----------+-----------+-------------+-------------------------------
       qh       | _J9..uEUi | $1$SNgqyKAi | $2a$06$B/Etc3J8zYBV49LrDU97MO
      (1 row)

   The salt generated by an algorithm has a fixed format. For example, in **$2a$06$** in the bf algorithm result, **2a** indicates the 2a variation of Blowfish, and **06** indicates the number of iterations. If **iter_count** is ignored, the default number of iterations will be used. The valid **iter_count** values depend on the algorithm used, as shown in the table below. For the xdes algorithm, the number of iterations must be an odd number.

   .. table:: **Table 2** Iteration counts of crypt()

      ========= ============= ==== ========
      Algorithm Default Value Min. Max.
      ========= ============= ==== ========
      xdes      725           1    16777215
      bf        6             4    31
      ========= ============= ==== ========

PGP Encryption Functions
------------------------

The PGP encryption function of GaussDB(DWS) complies with the OpenPGP (RFC 4880) standard, which includes requirements for symmetric key (private key) encryption and asymmetric key (public key) encryption.

An encrypted PGP message consists of the following parts:

-  Session key (encrypted symmetric key or public key) of the message
-  Data encrypted using the session key

For symmetric key (password) encryption:

#. The key is encrypted using the String2Key (S2K) algorithm, which is like a slowed down crypt() algorithm with a random salt. A full-length binary key will be generated.
#. If a separate session key is required, a random key will be generated. If it is not required, the S2K key will be used as the session key.
#. If the S2K key is directly used for a session, this key will be put in the session key packet. Otherwise, the S2K key will be used to encrypt the session key, and the encryption result will be put in the session key packet.

For public key encryption:

#. A random session key is generated.
#. This random key is encrypted using the public key and then put in the session key packet.

In either case, the data encryption process is as follows:

#. (Optional) Compress data, convert data to UTF-8, or convert newline characters.
#. A block consisting of random bytes is added before the data, serving as a random initial value (IV).
#. A random prefix and the SHA1 hash value suffix are added to the data.
#. The entire content is encrypted using the session key and then placed in the data packet.

**Supported PGP encryption functions**

-  pgp_sym_encrypt()

   Description: Encrypts a symmetric key.

   Syntax:

   .. code-block::

      pgp_sym_encrypt(data text, psw text [, options text ]) returns bytea
      pgp_sym_encrypt_bytea(data bytea, psw text [, options text ]) returns bytea

   **data** indicates the data to be encrypted, **psw** indicates the PGP symmetric key, and **options** is used to set options. For details, see :ref:`Table 3 <en-us_topic_0000001460562880__table571713162917>`.

-  pgp_sym_decrypt()

   Description: Decrypts a message encrypted using a PGP symmetric key.

   Syntax:

   .. code-block::

      pgp_sym_decrypt(msg bytea, psw text [, options text ]) returns text
      pgp_sym_decrypt_bytea(msg bytea, psw text [, options text ]) returns bytea

   **msg** indicates the data to be decrypted, **psw** indicates the PGP symmetric key, and **options** is used to set options. For details, see :ref:`Table 3 <en-us_topic_0000001460562880__table571713162917>`. To avoid generating invalid characters, you are not allowed to use the **pgp_sym_decrypt** function to decrypt bytea data. You can use the **pgp_sym_decrypt_bytea** function instead.

-  pgp_pub_encrypt()

   Description: Encrypts a public key.

   Syntax:

   .. code-block::

      pgp_pub_encrypt(data text, key bytea [, options text ]) returns bytea
      pgp_pub_encrypt_bytea(data bytea, key bytea [, options text ]) returns bytea

   **data** indicates the data to be encrypted. **key** indicates the PGP public key. If a private key is used as input, an error will be returned. **options** is used to set options. For details, see :ref:`Table 3 <en-us_topic_0000001460562880__table571713162917>`.

-  pgp_pub_decrypt()

   Description: Decrypts a message encrypted using a PGP public key.

   Syntax:

   .. code-block::

      pgp_pub_decrypt(msg bytea, key bytea [, psw text [, options text ]]) returns text
      pgp_pub_decrypt_bytea(msg bytea, key bytea [, psw text [, options text ]]) returns bytea

   You can decrypt a message encrypted using a public key. The **key** must be the private key corresponding to the public key used for encryption. If the private key is password protected, specify the password in **psw**. If you have not specified any password but want to specify this option now, provide an empty password.

   To avoid generating invalid characters, you are not allowed to use the pgp_pub_decrypt function to decrypt bytea data. You can use **pgp_pub_decrypt_bytea** function instead.

   The **key** must be the private key corresponding to the public key used for encryption. If the private key is password protected, specify the password in **psw**. If you have not specified any password but want to specify this option now, provide an empty password. The options **parameter** is used to set options. For details, see :ref:`Table 3 <en-us_topic_0000001460562880__table571713162917>`.

-  pgp_key_id()

   Description: Extracts the key ID of the PGP public or private key. If an encrypted message is used as the input, the ID of the key used to encrypt the message will be returned.

   Syntax:

   .. code-block::

      pgp_key_id(bytea) returns text

   This function can return two special key IDs:

   -  **SYMKEY**, indicating that a message is encrypted using a symmetric key.
   -  **ANYKEY**, indicating that a message is encrypted using the public key, but the key ID has been deleted. To decrypt the message in this case, you need to try all the keys until you find the correct private key. pgcrypto does not produce such encrypted messages.

   .. note::

      Different keys may have the same ID. This situation rarely occurs. In this case, the client application needs to try different keys for decryption, in the same way it deals with **ANYKEY**.

-  armor()

   Description: Converts binary data into PGP ASCII-armor format by the CRC calculation and formatting of a Base64 string.

   Syntax:

   .. code-block::

      armor(data bytea [ , keys text[], values text[] ]) returns text

-  dearmor()

   Description: Performs the reverse conversion.

   Syntax:

   .. code-block::

      dearmor(data text) returns bytea

   Converts the encrypted data bytea to the PGP ASCII-armor format, or the other way around.

   **data** indicates the data to be converted. If multiple pairs of keys and values are specified, an armor header will be generated for each key-value pair and added to the output. The two arrays are both one-dimensional arrays with the same length, and cannot contain non-ASCII characters.

-  pgp_armor_headers()

   Description: Returns the armor header in the data.

   .. code-block::

      pgp_armor_headers(data text, key out text, value out text) returns setof record

   The return result is a data row set consisting of key and value columns. Any non-ASCII characters contained in the set are regarded as UTF-8 characters.

   **Using GnuPG to generate PGP keys**

   Generate a key.

   .. code-block::

      gpg --gen-key

   DSA and Elgamal keys are recommended.

   To use an RSA key, you must create a DSA or RSA key as the master key used only for signature, and then specify **gpg --edit-key** to add an RSA encryption subkey.

   List keys.

   .. code-block::

      gpg --list-secret-keys

   Export a public key in ASCII-protected format.

   .. code-block::

      gpg -a --export KEYID > public.key

   Export a private key in ASCII-protected format.

   .. code-block::

      gpg -a --export-secret-keys KEYID > secret.key

   Before using these keys as the input to the PGP function, run dearmor() on them. Alternatively, if you can process binary data, remove **-a** from the command.

   .. important::

      The PGP encryption function has the following restrictions:

      -  Signatures are not supported. This function does not check whether the encryption subkey belongs to the master key.
      -  The encryption key cannot be used as the master key. This constraint does not impose much impact, because it is rarely violated.
      -  Only one subkey is allowed. This may be a problem, because multiple subkeys are often required. General GPG and PGP keys cannot be used as pgcrypto encryption keys. Their usage is totally different.

   **PGP function parameters**

   The option names in the pgcrypto function are similar to those in the GnuPG function. Option values are set using equal signs (=), and the options are separated by commas (,). Example:

   .. code-block::

      pgp_sym_encrypt(data, psw, 'compress-algo=1, cipher-algo=aes256')

   Options other than **convert-crlf** can be used only for encryption functions. The decryption function obtains parameters from PGP data.

   The most common options are **compress-algo** and **unicode-mode**. You can retain the default values for other options.

   .. _en-us_topic_0000001460562880__table571713162917:

   .. table:: **Table 3** pgcrypto encryption options

      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+
      | Option          | Description                                                                                                                                                                                                                                                           | Default Value                              | Value                                                                | Function                                                           |
      +=================+=======================================================================================================================================================================================================================================================================+============================================+======================================================================+====================================================================+
      | cipher-algo     | Cryptographic algorithm                                                                                                                                                                                                                                               | aes128                                     | bf, aes128, aes192, aes256, 3des, cast5                              | pgp_sym_encrypt, pgp_pub_encrypt                                   |
      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+
      | compress-algo   | Compression algorithm                                                                                                                                                                                                                                                 | 0                                          | -  **0**: not compressed                                             | pgp_sym_encrypt, pgp_pub_encrypt                                   |
      |                 |                                                                                                                                                                                                                                                                       |                                            | -  **1**: ZIP compression                                            |                                                                    |
      |                 |                                                                                                                                                                                                                                                                       |                                            | -  **2**: ZLIB compression (ZIP + Metadata + CRC)                    |                                                                    |
      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+
      | compress-level  | Compression level. A high level indicates the compression will be slow, but the data size after compression will be small. **0** disables compression.                                                                                                                | 6                                          | 0, 1-9                                                               | pgp_sym_encrypt, pgp_pub_encrypt                                   |
      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+
      | convert-crlf    | Indicates whether to convert **\\n** to **\\r\\n** during encryption, and whether to convert **\\r\\n** to **\\n** during decryption. RFC4880 requires that **\\r\\n** must be used as the newline character in text data storage.                                    | 0                                          | 0, 1                                                                 | pgp_sym_encrypt, pgp_pub_encrypt, pgp_sym_decrypt, pgp_pub_decrypt |
      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+
      | disable-mdc     | SHA-1 is not used to protect data. It is used only for compatibility with old PGP products.                                                                                                                                                                           | 0                                          | 0, 1                                                                 | pgp_sym_encrypt, pgp_pub_encrypt                                   |
      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+
      | sess-key        | A separate session key is used. Public key encryption always uses a separate session key. This option is used for symmetric key encryption, which directly uses the S2K key by default.                                                                               | 0                                          | 0, 1                                                                 | pgp_sym_encrypt                                                    |
      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+
      | s2k-mode        | S2K algorithm                                                                                                                                                                                                                                                         | 3                                          | -  **0**: Salt is not used. This setting is not recommended.         | pgp_sym_encrypt                                                    |
      |                 |                                                                                                                                                                                                                                                                       |                                            | -  **1**: Salt is used, but the number of iterations is fixed.       |                                                                    |
      |                 |                                                                                                                                                                                                                                                                       |                                            | -  **3**: Salt is used, and the number of iterations can be changed. |                                                                    |
      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+
      | s2k-count       | Number of iterations of the S2K algorithm                                                                                                                                                                                                                             | A random value between 65,536 and 253,952. | 1024 <= Value <= 65,011,712                                          | **pgp_sym_encrypt** and **s2k-mode=3**                             |
      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+
      | s2k-digest-algo | Digest algorithm used during S2K calculation                                                                                                                                                                                                                          | sha1                                       | md5, sha1                                                            | pgp_sym_encrypt                                                    |
      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+
      | s2k-cipher-algo | Password used to encrypt a separate session key                                                                                                                                                                                                                       | cipher-algo algorithm                      | bf, aes, aes128, aes192, aes256                                      | pgp_sym_encrypt                                                    |
      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+
      | unicode-mode    | Whether to convert text data between database internal encoding and UTF-8. If the database already uses UTF-8 encoding, no conversion will be performed, but the message will be marked as UTF-8. If this parameter is not specified, the message will not be marked. | 0                                          | 0, 1                                                                 | pgp_sym_encrypt, pgp_pub_encrypt                                   |
      +-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------+----------------------------------------------------------------------+--------------------------------------------------------------------+

Raw Encryption Functions
------------------------

Raw encryption functions only run a cipher over data. They don't have any advanced features of PGP encryption. Therefore they have the following problems:

-  They use user key directly as cipher key.
-  No integrity check is performed to check whether the encrypted data was modified.
-  You need to associate all encryption parameters yourself, including IV.
-  Text data cannot be processed.

With the introduction of PGP encryption, these raw encryption functions are not recommended.

.. code-block::

   encrypt(data bytea, key bytea, type text) returns bytea
   decrypt(data bytea, key bytea, type text) returns bytea
   encrypt_iv(data bytea, key bytea, iv bytea, type text) returns bytea
   decrypt_iv(data bytea, key bytea, iv bytea, type text) returns bytea

**data** indicates the data to be encrypted, and **type** indicates the encryption/decryption method. The syntax of the **type** parameter is as follows:

.. code-block::

   algorithm [ - mode ] [ /pad: padding ]

The options of **algorithm** are as follows:

-  **bf**: Blowfish algorithm. Synonyms: **BF**, **BF-CBC**; **BLOWFISH**, **BF-CBC**; **BLOWFISH-CBC**, **BF-CBC**; **BLOWFISH-ECB**, **BF-ECB**; **BLOWFISH-CFB**, **BF-CFB**
-  **aes**: AES algorithm (Rijndael-128, -192, or -256). **Synonyms**: **AES**, **AES-CBC**, **RIJNDAEL**, **AES-CBC**, **RIJNDAEL**, **AES-CBC**, **RIJNDAEL-CBC**, **AES-CBC**, **RIJNDAEL-ECB**, **AES-ECB**
-  DES algorithm. Synonyms: **DES**, **DES-CBC**; **3DES**, **DES3-CBC**, **3DES-ECB**, **DES3-ECB**; **3DES-CBC**, **DES3-CBC**
-  **sm4**: SM4 algorithm. Synonym: **SM4-CBC**
-  CAST5 algorithm. Synonym: **CAST5-CBC**

The options of **mode** are as follows:

-  **cbc**: The next block depends on the previous block. (This is the default value.)
-  **ecb**: Each block is encrypted separately. (This value is used only for tests.)

The options of **padding** are as follows:

-  **pkcs**: The data can be of any length. (This is the default value.)
-  **none**: The data must be a multiple of cipher block size.

For example, the encryption results of the following functions are the same:

.. code-block::

   encrypt(data, 'fooz', 'bf')
   encrypt(data, 'fooz', 'bf-cbc/pad:pkcs')

For the **encrypt_iv** and **decrypt_iv** functions, the **iv** parameter indicates the initial value for the CBC mode. This parameter is ignored for ECB. It is truncated or padded with zeroes if not exactly block size. It defaults to all zeroes in the functions without this parameter.

Random Data Functions
---------------------

-  The gen_random_bytes() function is used to generate cryptographically strong random bytes.

   .. code-block::

      gen_random_bytes(count integer) returns bytea

   **count** indicates the number of returned bytes. The value range is 1 to 1024.

   Example:

   .. code-block::

      SELECT gen_random_bytes(16);
                gen_random_bytes
      ------------------------------------
       \x1f1eddc11153afdde0f9e1229f8f4caf
      (1 row)

-  The gen_random_uuid() function is used to return a random UUID of version 4.

   .. code-block::

      SELECT gen_random_uuid();
      gen_random_uuid
      --------------------------------------
      2bd664a2-b760-4859-8af6-8d09ccc5b830
