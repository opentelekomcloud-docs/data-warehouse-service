:original_name: dws_16_0209.html

.. _dws_16_0209:

.. _en-us_topic_0000001819336265:

SET CHARACTER
=============

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
