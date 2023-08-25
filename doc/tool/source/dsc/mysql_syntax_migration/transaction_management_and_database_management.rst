:original_name: dws_07_8314.html

.. _dws_07_8314:

Transaction Management and Database Management
==============================================

This section describes how to migrate keywords and features related to MySQL transaction and database management.

Transaction Management
----------------------

#. **TRANSACTION**

   DSC will perform adaptation based on GaussDB features during MySQL transaction statement migration.

   **Input**

   .. code-block::

      ## Each statement applies only to the next single transaction performed within the session.
      SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
      SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
      SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
      SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
      SET TRANSACTION READ ONLY;
      SET TRANSACTION READ WRITE;
      SET TRANSACTION ISOLATION LEVEL READ COMMITTED,READ ONLY;
      SET TRANSACTION ISOLATION LEVEL SERIALIZABLE,READ WRITE;
      ## Each statement (with the SESSION keyword) applies to all subsequent transactions performed within the current session.
      START TRANSACTION;
      SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
      SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
      SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
      SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
      commit ;

   **Output**

   .. code-block::

      -- Each statement applies only to the next single transaction performed within the session.
      SET LOCAL TRANSACTION ISOLATION LEVEL READ COMMITTED;
      SET LOCAL TRANSACTION ISOLATION LEVEL REPEATABLE READ;
      SET LOCAL TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
      SET LOCAL TRANSACTION ISOLATION LEVEL SERIALIZABLE;
      SET LOCAL TRANSACTION READ ONLY;
      SET LOCAL TRANSACTION READ WRITE;
      SET LOCAL TRANSACTION ISOLATION LEVEL READ COMMITTED READ ONLY;
      SET LOCAL TRANSACTION ISOLATION LEVEL SERIALIZABLE READ WRITE;
      -- Each statement (with the SESSIONkeyword) applies to all subsequent transactions performed within the current session.
      START TRANSACTION;
      SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL READ COMMITTED;
      SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL READ COMMITTED;
      SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL SERIALIZABLE;
      SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL SERIALIZABLE;
      COMMIT WORK;

#. **LOCK**

   DSC will perform adaptation based on GaussDB features during the migration of MySQL table locking statements which are used in transaction processing.

   **Input**

   .. code-block::

      ## A.
      START TRANSACTION;
      LOCK TABLES `mt`.`runoob_tbl` WRITE,`mt`.`runoob_tb2` READ;
      commit;

      ## B.
      START TRANSACTION;
      LOCK TABLES `mt`.`runoob_tbl` WRITE;
      commit;

      ## C.
      START TRANSACTION;
      LOCK TABLES `mt`.`runoob_tbl` READ,`mt`.`runoob_tbl` AS t1 READ;
      commit;

   **Output**

   .. code-block::

      -- A.
      START TRANSACTION;
      LOCK TABLE "mt"."runoob_tbl" IN ACCESS EXCLUSIVE MODE;
      LOCK TABLE "mt"."runoob_tb2" IN ACCESS SHARE MODE;
      COMMIT WORK;

      -- B.
      START TRANSACTION;
      LOCK TABLE "mt"."runoob_tbl" IN ACCESS EXCLUSIVE MODE;
      COMMIT WORK;

      -- C.
      START TRANSACTION;
      LOCK TABLE "mt"."runoob_tbl" IN ACCESS SHARE MODE;
      COMMIT WORK;

Database Management
-------------------

#. **SET CHARACTER**

   DSC will replace MySQL **SET CHARACTER SET** with **SET SESSION NAMES** during migration. The following table lists character set mapping.

   .. table:: **Table 1**

      =================== =====================
      MySQL CHARACTER SET GaussDB SESSION NAMES
      =================== =====================
      ASCII               SQL_ASCII
      BIG5                BIG5
      CP1250              WIN1250
      CP1251              WIN1251
      CP1256              WIN1256
      CP1257              WIN1257
      CP932               SJIS
      EUCJPMS             EUC_JP
      EUCKR               EUC_KR
      GB2312              GB18030
      GBK                 GBK
      GREEK               ISO_8859_7
      HEBREW              ISO_8859_8
      KOI8R               KOI8R
      KOI8U               KOI8U
      LATIN1              LATIN1
      LATIN2              LATIN2
      LATIN5              LATIN5
      LATIN7              LATIN7
      SJIS                SJIS
      SWE7                UTF8
      TIS620              WIN874
      UTF8                UTF8
      UTF8MB4             UTF8
      =================== =====================

   **Input**

   .. code-block::

      SET CHARACTER SET 'ASCII';
      SET CHARACTER SET 'BIG5';
      SET CHARACTER SET 'CP1250';
      SET CHARACTER SET 'CP1251';
      SET CHARACTER SET 'CP1256';
      SET CHARACTER SET 'CP1257';
      SET CHARACTER SET 'CP932';
      SET CHARACTER SET 'EUCJPMS';
      SET CHARACTER SET 'EUCKR';
      SET CHARACTER SET 'GB2312';
      SET CHARACTER SET 'GBK';
      SET CHARACTER SET 'GREEK';
      SET CHARACTER SET 'HEBREW';
      SET CHARACTER SET 'KOI8R';
      SET CHARACTER SET 'KOI8U';
      SET CHARACTER SET 'LATIN1';
      SET CHARACTER SET 'LATIN2';
      SET CHARACTER SET 'LATIN5';
      SET CHARACTER SET 'LATIN7';
      SET CHARACTER SET 'SJIS';
      SET CHARACTER SET 'SWE7';
      SET CHARACTER SET 'TIS620';
      SET CHARACTER SET 'UTF8';
      SET CHARACTER SET 'UTF8MB4';
      ## MySQL does not support SET CHARACTER SET 'UCS2';.
      ## MySQL does not support SET CHARACTER SET 'UTF16';.
      ## MySQL does not support SET CHARACTER SET 'UTF16LE';.
      ## MySQL does not support SET CHARACTER SET 'UTF32';.

   **Output**

   .. code-block::

      SET SESSION NAMES 'SQL_ASCII';
      SET SESSION NAMES 'BIG5';
      SET SESSION NAMES 'WIN1250';
      SET SESSION NAMES 'WIN1251';
      SET SESSION NAMES 'WIN1256';
      SET SESSION NAMES 'WIN1257';
      SET SESSION NAMES 'SJIS';
      SET SESSION NAMES 'EUC_JP';
      SET SESSION NAMES 'EUC_KR';
      SET SESSION NAMES 'GB18030';
      SET SESSION NAMES 'GBK';
      SET SESSION NAMES 'ISO_8859_7';
      SET SESSION NAMES 'ISO_8859_8';
      SET SESSION NAMES 'KOI8R';
      SET SESSION NAMES 'KOI8U';
      SET SESSION NAMES 'LATIN1';
      SET SESSION NAMES 'LATIN2';
      SET SESSION NAMES 'LATIN5';
      SET SESSION NAMES 'LATIN7';
      SET SESSION NAMES 'SJIS';
      SET SESSION NAMES 'UTF8';
      SET SESSION NAMES 'WIN874';
      SET SESSION NAMES 'UTF8';
      SET SESSION NAMES 'UTF8';
      -- MySQL does not support SET CHARACTER SET 'UCS2';.
      -- MySQL does not support SET CHARACTER SET 'UTF16';.
      -- MySQL does not support SET CHARACTER SET 'UTF16LE';.
      -- MySQL does not support SET CHARACTER SET 'UTF32';.
