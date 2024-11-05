:original_name: dws_04_0995.html

.. _dws_04_0995:

Encrypting and Decrypting GaussDB(DWS) Strings
==============================================

GaussDB(DWS) supports encryption and decryption of strings using the following functions:

-  gs_encrypt(encryptstr, keystr, cryptotype, cryptomode, hashmethod)

   Description: Encrypts an **encryptstr** string using the **keystr** key based on the encryption algorithm specified by **cryptotype** and **cryptomode** and the HMAC algorithm specified by **hashmethod**, and returns the encrypted string. **cryptotype** can be **aes128**, **aes192**, **aes256**, or **sm4**. **cryptomode** is **cbc**. **hashmethod** can be **sha256**, **sha384**, **sha512**, or **sm3**. Currently, the following types of data can be encrypted: numerals supported in the database; character type; RAW in binary type; and DATE, TIMESTAMP, and SMALLDATETIME in date/time type. The **keystr** length is related to the encryption algorithm and contains 1 to **KeyLen** bytes. If **cryptotype** is **aes128** or **sm4**, **KeyLen** is **16**; if **cryptotype** is **aes192**, **KeyLen** is **24**; if **cryptotype** is **aes256**, **KeyLen** is **32**.

   Return type: text

   Length of the return value: at least 4 x [(maclen + 56)/3] bytes and no more than 4 x [(Len + maclen + 56)/3] bytes, where **Len** indicates the string length (in bytes) before the encryption and **maclen** indicates the length of the HMAC value. If **hashmethod** is **sha256** or **sm3**, **maclen** is **32**; if **hashmethod** is **sha384**, **maclen** is **48**; if **hashmethod** is **sha512**, **maclen** is **64**. That is, if **hashmethod** is **sha256** or **sm3**, the returned string contains 120 to 4 x [(Len + 88)/3] bytes; if **hashmethod** is **sha384**, the returned string contains 140 to 4 x [(Len + 104)/3] bytes; if **hashmethod** is **sha512**, the returned string contains 160 to 4 x [(Len + 120)/3] bytes.

   Example:

   ::

      SELECT gs_encrypt('GaussDB(DWS)', '1234', 'aes128', 'cbc',  'sha256');
                                                              gs_encrypt
      --------------------------------------------------------------------------------------------------------------------------
       AAAAAAAAAACcFjDcCSbop7D87sOa2nxTFrkE9RJQGK34ypgrOPsFJIqggI8tl+eMDcQYT3po98wPCC7VBfhv7mdBy7IVnzdrp0rdMrD6/zTl8w0v9/s2OA==
      (1 row)

   .. note::

      -  A decryption password is required during the execution of this function. For security purposes, the gsql tool does not record this function in the execution history. That is, the execution history of this function cannot be found in **gsql** by paging up and down.
      -  Do not use the **ge_encrypt** and **gs_encrypt_aes128** functions for the same data table.

-  gs_decrypt(decryptstr, keystr,cryptotype, cryptomode, hashmethod)

   Description: Decrypts a **decryptstr** string using the **keystr** key based on the encryption algorithm specified by **cryptotype** and **cryptomode** and the HMAC algorithm specified by **hashmethod**, and returns the decrypted string. The **keystr** used for decryption must be consistent with that used for encryption. **keystr** cannot be empty.

   Return type: text

   Example:

   ::

      SELECT gs_decrypt('AAAAAAAAAACcFjDcCSbop7D87sOa2nxTFrkE9RJQGK34ypgrOPsFJIqggI8tl+eMDcQYT3po98wPCC7VBfhv7mdBy7IVnzdrp0rdMrD6/zTl8w0v9/s2OA==', '1234', 'aes128', 'cbc', 'sha256');
        gs_decrypt
      --------------
       GaussDB(DWS)
      (1 row)

   .. note::

      -  A decryption password is required during the execution of this function. For security purposes, the gsql tool does not record this function in the execution history. That is, the execution history of this function cannot be found in **gsql** by paging up and down.
      -  This function works with the **gs_encrypt** function, and the two functions must use the same encryption algorithm and HMAC algorithm.

-  gs_encrypt_aes128(encryptstr,keystr)

   Description: Encrypts **encryptstr** strings using **keystr** as the key and returns encrypted strings. The length of **keystr** ranges from 1 to 16 bytes. Currently, the following types of data can be encrypted: numerals supported in the database; character type; RAW in binary type; and DATE, TIMESTAMP, and SMALLDATETIME in date/time type.

   Return type: text

   Length of the return value: At least 92 bytes and no more than (4*[*Len*/3]+68) bytes, where *Len* indicates the length of the data before encryption (unit: byte).

   Example:

   ::

      SELECT gs_encrypt_aes128('DWS','1234');
                                            gs_encrypt_aes128
      ----------------------------------------------------------------------------------------------
       ZrCp794vO5I9qJ+jHFf/sQqRyMBy0lKIDGP5S8RJXzgmpXoa/e4EgmK82P5y5xe1bOXbJeoNxyHagK9OhPVVeJDbn/M=
      (1 row)

   .. note::

      -  A decryption password is required during the execution of this function. For security purposes, the gsql tool does not record this function in the execution history. That is, the execution history of this function cannot be found in **gsql** by paging up and down.
      -  Do not use the **ge_encrypt** and **gs_encrypt_aes128** functions for the same data table.

-  gs_decrypt_aes128(decryptstr,keystr)

   Description: Decrypts a **decryptstr** string using the **keystr** key and returns the decrypted string. The **keystr** used for decryption must be consistent with that used for encryption. **keystr** cannot be empty.

   Return type: text

   Example:

   ::

      SELECT gs_decrypt_aes128('ZrCp794vO5I9qJ+jHFf/sQqRyMBy0lKIDGP5S8RJXzgmpXoa/e4EgmK82P5y5xe1bOXbJeoNxyHagK9OhPVVeJDbn/M=','1234');
       gs_decrypt_aes128
      -------------------
       DWS
      (1 row)

   .. note::

      -  A decryption password is required during the execution of this function. For security purposes, the gsql tool does not record this function in the execution history. That is, the execution history of this function cannot be found in **gsql** by paging up and down.
      -  This function works with the **gs_encrypt_aes128** function.

-  gs_hash(hashstr, hashmethod)

   Description: Obtains the digest string of a **hashstr** string based on the algorithm specified by **hashmethod**. **hashmethod** can be **sha256**, **sha384**, **sha512**, or **sm3**.

   Return type: text

   Length of the return value: 64 bytes if **hashmethod** is **sha256** or **sm3**; 96 bytes if **hashmethod** is **sha384**; 128 bytes if **hashmethod** is **sha512**

   Example:

   ::

      SELECT gs_hash('GaussDB(DWS)', 'sha256');
                                                   gs_hash
      --------------------------------------------------------------------------------------------------
       e59069daa6541ae20af7c747662702c731b26b8abd7a788f4d15611aa0db608efdbb5587ba90789a983f85dd51766609
      (1 row)

-  md5(string)

   Description: Encrypts a string in MD5 mode and returns a value in hexadecimal form.

   .. note::

      MD5 is insecure and is not recommended.

   Return type: text

   Example:

   ::

      SELECT md5('ABC');
                     md5
      ----------------------------------
       902fbdd2b1df0c4f70b4a5d23525e932
      (1 row)
