:original_name: dws_04_0551.html

.. _dws_04_0551:

DBMS_LOB
========

Related Interfaces
------------------

:ref:`Table 1 <en-us_topic_0000001145494799__t652a1b67fb4746cb846f73477c3bb77f>` provides all interfaces supported by the **DBMS_LOB** package.

.. _en-us_topic_0000001145494799__t652a1b67fb4746cb846f73477c3bb77f:

.. table:: **Table 1** DBMS_LOB

   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | API                                                                                           | Description                                                                                                                                                 |
   +===============================================================================================+=============================================================================================================================================================+
   | :ref:`DBMS_LOB.GETLENGTH <en-us_topic_0000001145494799__l112821beeb424f68a87f11bbcb23ba8c>`   | Obtains and returns the specified length of a LOB object.                                                                                                   |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.OPEN <en-us_topic_0000001145494799__li7581543191913>`                          | Opens a LOB and returns a LOB descriptor.                                                                                                                   |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.READ <en-us_topic_0000001145494799__l48e5b599124e4753818164902a609713>`        | Loads a part of LOB contents to BUFFER area according to the specified length and initial position offset.                                                  |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.WRITE <en-us_topic_0000001145494799__l1fd7a1bbcff5458890a384d2f8152a31>`       | Copies contents in BUFFER area to LOB according to the specified length and initial position offset.                                                        |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.WRITEAPPEND <en-us_topic_0000001145494799__lfcc14fd73f834169bb03ceb2e832f817>` | Copies contents in BUFFER area to the end part of LOB according to the specified length.                                                                    |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.COPY <en-us_topic_0000001145494799__l2af2f8b7366643ca91559e18eaea4692>`        | Copies contents in BLOB to another BLOB according to the specified length and initial position offset.                                                      |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.ERASE <en-us_topic_0000001145494799__l8d03841cf8fb4c4b899a356efcb34f0e>`       | Deletes contents in BLOB according to the specified length and initial position offset.                                                                     |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.CLOSE <en-us_topic_0000001145494799__ta292aad6be2b4dfcb2cc8efe0c540879>`       | Closes a LOB descriptor.                                                                                                                                    |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.INSTR <en-us_topic_0000001145494799__li1442318419148>`                         | Returns the position of the Nth occurrence of a character string in LOB.                                                                                    |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.COMPARE <en-us_topic_0000001145494799__li19579245181414>`                      | Compares two LOBs or a certain part of two LOBs.                                                                                                            |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.SUBSTR <en-us_topic_0000001145494799__li381175591417>`                         | Reads the substring of a LOB and returns the number of read bytes or the number of characters.                                                              |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.TRIM <en-us_topic_0000001145494799__li374125861411>`                           | Truncates the LOB of a specified length. After the execution is complete, the length of the LOB is set to the length specified by the **newlen** parameter. |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.CREATETEMPORARY <en-us_topic_0000001145494799__li145817213157>`                | Creates a temporary BLOB or CLOB.                                                                                                                           |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_LOB.APPEND <en-us_topic_0000001145494799__li194203411159>`                         | Adds the content of a LOB to another LOB.                                                                                                                   |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001145494799__l112821beeb424f68a87f11bbcb23ba8c:

   DBMS_LOB.GETLENGTH

   Specifies the length of a LOB type object obtained and returned by the stored procedure **GETLENGTH**.

   The function prototype of **DBMS_LOB.GETLENGTH** is:

   ::

      DBMS_LOB.GETLENGTH (
      lob_loc    IN   BLOB)
      RETURN INTEGER;

      DBMS_LOB.GETLENGTH (
      lob_loc    IN   CLOB)
      RETURN INTEGER;

   .. table:: **Table 2** DBMS_LOB.GETLENGTH interface parameters

      ========= ==============================================
      Parameter Description
      ========= ==============================================
      lob_loc   LOB type object whose length is to be obtained
      ========= ==============================================

-  .. _en-us_topic_0000001145494799__li7581543191913:

   DBMS_LOB.OPEN

   A stored procedure opens a LOB and returns a LOB descriptor. This process is used only for compatibility.

   The function prototype of **DBMS_LOB.OPEN** is:

   ::

      DBMS_LOB.LOB (
      lob_loc INOUT BLOB,
      open_mode IN BINARY_INTEGER);

      DBMS_LOB.LOB (
      lob_loc INOUT CLOB,
      open_mode IN BINARY_INTEGER);

   .. table:: **Table 3** DBMS_LOB.OPEN interface parameters

      +-----------------------------+------------------------------------------------------------+
      | Parameter                   | Description                                                |
      +=============================+============================================================+
      | lob_loc                     | BLOB or CLOB descriptor that is opened                     |
      +-----------------------------+------------------------------------------------------------+
      | open_mode IN BINARY_INTEGER | Open mode (currently, DBMS_LOB.LOB_READWRITE is supported) |
      +-----------------------------+------------------------------------------------------------+

-  .. _en-us_topic_0000001145494799__l48e5b599124e4753818164902a609713:

   DBMS_LOB.READ

   The stored procedure **READ** loads a part of LOB contents to BUFFER according to the specified length and initial position offset.

   The function prototype of **DBMS_LOB.READ** is:

   ::

      DBMS_LOB.READ (
      lob_loc     IN           BLOB,
      amount      IN           INTEGER,
      offset      IN           INTEGER,
      buffer      OUT          RAW);

      DBMS_LOB.READ (
      lob_loc    IN            CLOB,
      amount     IN OUT        INTEGER,
      offset     IN            INTEGER,
      buffer     OUT           VARCHAR2);

   .. table:: **Table 4** DBMS_LOB.READ interface parameters

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                               |
      +===================================+===========================================================================================================================+
      | lob_loc                           | LOB type object to be loaded                                                                                              |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------+
      | amount                            | Load data length                                                                                                          |
      |                                   |                                                                                                                           |
      |                                   | .. note::                                                                                                                 |
      |                                   |                                                                                                                           |
      |                                   |    If the read length is negative, the error message "ERROR: argument 2 is null, invalid, or out of range." is displayed. |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------+
      | offset                            | Indicates where to start reading the LOB contents, that is, the offset bytes to initial position of LOB contents.         |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------+
      | buffer                            | Target buffer to store the loaded LOB contents                                                                            |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001145494799__l1fd7a1bbcff5458890a384d2f8152a31:

   DBMS_LOB.WRITE

   The stored procedure **WRITE** copies contents in BUFFER to LOB variables according to the specified length and initial position offset.

   The function prototype of **DBMS_LOB.WRITE** is:

   ::

      DBMS_LOB.WRITE (
      lob_loc    IN OUT     BLOB,
      amount     IN         INTEGER,
      offset     IN         INTEGER,
      buffer     IN         RAW);

      DBMS_LOB.WRITE (
      lob_loc   IN OUT      CLOB,
      amount    IN          INTEGER,
      offset    IN          INTEGER,
      buffer    IN          VARCHAR2);

   .. table:: **Table 5** DBMS_LOB.WRITE interface parameters

      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                       |
      +===================================+===================================================================================================================+
      | lob_loc                           | LOB type object to be written                                                                                     |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------+
      | amount                            | Write data length                                                                                                 |
      |                                   |                                                                                                                   |
      |                                   | .. note::                                                                                                         |
      |                                   |                                                                                                                   |
      |                                   |    If the write data is shorter than 1 or longer than the contents to be written, an error is reported.           |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------+
      | offset                            | Indicates where to start writing the LOB contents, that is, the offset bytes to initial position of LOB contents. |
      |                                   |                                                                                                                   |
      |                                   | .. note::                                                                                                         |
      |                                   |                                                                                                                   |
      |                                   |    If the offset is shorter than 1 or longer than the maximum length of LOB type contents, an error is reported.  |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------+
      | buffer                            | Content to be written                                                                                             |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001145494799__lfcc14fd73f834169bb03ceb2e832f817:

   DBMS_LOB.WRITEAPPEND

   The stored procedure **WRITEAPPEND** copies contents in BUFFER to the end part of LOB according to the specified length.

   The function prototype of **DBMS_LOB.WRITEAPPEND** is:

   ::

      DBMS_LOB.WRITEAPPEND (
      lob_loc     IN OUT     BLOB,
      amount      IN         INTEGER,
      buffer      IN         RAW);

      DBMS_LOB.WRITEAPPEND (
      lob_loc     IN OUT     CLOB,
      amount      IN         INTEGER,
      buffer      IN         VARCHAR2);

   .. table:: **Table 6** DBMS_LOB.WRITEAPPEND interface parameters

      +-----------------------------------+---------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                             |
      +===================================+=========================================================================================================+
      | lob_loc                           | LOB type object to be written                                                                           |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------+
      | amount                            | Write data length                                                                                       |
      |                                   |                                                                                                         |
      |                                   | .. note::                                                                                               |
      |                                   |                                                                                                         |
      |                                   |    If the write data is shorter than 1 or longer than the contents to be written, an error is reported. |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------+
      | buffer                            | Content to be written                                                                                   |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001145494799__l2af2f8b7366643ca91559e18eaea4692:

   DBMS_LOB.COPY

   The stored procedure **COPY** copies contents in BLOB to another BLOB according to the specified length and initial position offset.

   The function prototype of **DBMS_LOB.COPY** is:

   ::

      DBMS_LOB.COPY (
      dest_lob      IN OUT     BLOB,
      src_lob       IN         BLOB,
      amount        IN         INTEGER,
      dest_offset   IN         INTEGER  DEFAULT 1,
      src_offset    IN         INTEGER  DEFAULT 1);

   .. table:: **Table 7** DBMS_LOB.COPY interface parameters

      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                            |
      +===================================+========================================================================================================================+
      | dest_lob                          | BLOB type object to be pasted                                                                                          |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------+
      | src_lob                           | BLOB type object to be copied                                                                                          |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------+
      | amount                            | Length of the copied data                                                                                              |
      |                                   |                                                                                                                        |
      |                                   | .. note::                                                                                                              |
      |                                   |                                                                                                                        |
      |                                   |    If the copied data is shorter than 1 or longer than the maximum length of BLOB type contents, an error is reported. |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------+
      | dest_offset                       | Indicates where to start pasting the BLOB contents, that is, the offset bytes to initial position of BLOB contents.    |
      |                                   |                                                                                                                        |
      |                                   | .. note::                                                                                                              |
      |                                   |                                                                                                                        |
      |                                   |    If the offset is shorter than 1 or longer than the maximum length of BLOB type contents, an error is reported.      |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------+
      | src_offset                        | Indicates where to start copying the BLOB contents, that is, the offset bytes to initial position of BLOB contents.    |
      |                                   |                                                                                                                        |
      |                                   | .. note::                                                                                                              |
      |                                   |                                                                                                                        |
      |                                   |    If the offset is shorter than 1 or longer than the length of source BLOB, an error is reported.                     |
      +-----------------------------------+------------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001145494799__l8d03841cf8fb4c4b899a356efcb34f0e:

   DBMS_LOB.ERASE

   The stored procedure **ERASE** deletes contents in BLOB according to the specified length and initial position offset.

   The function prototype of **DBMS_LOB.ERASE** is:

   ::

      DBMS_LOB.ERASE (
      lob_loc          IN OUT   BLOB,
      amount           IN OUT   INTEGER,
      offset           IN       INTEGER DEFAULT 1);

   .. table:: **Table 8** DBMS_LOB.ERASE interface parameters

      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                             |
      +===================================+=========================================================================================================================+
      | lob_loc                           | BLOB type object whose contents are to be deleted                                                                       |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------+
      | amount                            | Length of contents to be deleted                                                                                        |
      |                                   |                                                                                                                         |
      |                                   | .. note::                                                                                                               |
      |                                   |                                                                                                                         |
      |                                   |    If the deleted data is shorter than 1 or longer than the maximum length of BLOB type contents, an error is reported. |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------+
      | offset                            | Indicates where to start deleting the BLOB contents, that is, the offset bytes to initial position of BLOB contents.    |
      |                                   |                                                                                                                         |
      |                                   | .. note::                                                                                                               |
      |                                   |                                                                                                                         |
      |                                   |    If the offset is shorter than 1 or longer than the maximum length of BLOB type contents, an error is reported.       |
      +-----------------------------------+-------------------------------------------------------------------------------------------------------------------------+

-  DBMS_LOB.CLOSE

   The procedure **CLOSE** disables the enabled contents of LOB according to the specified length and initial position offset.

   The function prototype of **DBMS_LOB.CLOSE** is:

   ::

      DBMS_LOB.CLOSE(
      src_lob       IN              BLOB);

      DBMS_LOB.CLOSE (
      src_lob      IN               CLOB);

   .. _en-us_topic_0000001145494799__ta292aad6be2b4dfcb2cc8efe0c540879:

   .. table:: **Table 9** DBMS_LOB.CLOSE interface parameters

      ========= ==============================
      Parameter Description
      ========= ==============================
      src_loc   LOB type object to be disabled
      ========= ==============================

-  .. _en-us_topic_0000001145494799__li1442318419148:

   DBMS_LOB.INSTR

   This function returns the Nth occurrence position in LOB. If invalid values are entered, **NULL** is returned. The invalid values include offset < 1 or offset > LOBMAXSIZE, nth < 1, and nth > LOBMAXSIZE.

   The function prototype of **DBMS_LOB.INSTR** is:

   ::

      DBMS_LOB.INSTR (
      lob_loc     IN     BLOB,
      pattern     IN     RAW,
      offset      IN     INTEGER := 1,
      nth         IN     INTEGER := 1)
      RETURN INTEGER;

      DBMS_LOB.INSTR (
      lob_loc    IN     CLOB,
      pattern    IN     VARCHAR2 ,
      offset     IN     INTEGER := 1,
      nth        IN     INTEGER := 1)
      RETURN INTEGER;

   .. table:: **Table 10** DBMS_LOB.INSTR interface parameters

      +-----------+-------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter | Description                                                                                                                               |
      +===========+===========================================================================================================================================+
      | lob_loc   | LOB descriptor to be searched for                                                                                                         |
      +-----------+-------------------------------------------------------------------------------------------------------------------------------------------+
      | pattern   | Matched pattern. It is RAW for BLOB and TEXT for CLOB.                                                                                    |
      +-----------+-------------------------------------------------------------------------------------------------------------------------------------------+
      | offset    | For BLOB, the absolute offset is in the unit of byte. For CLOB, the offset is in the unit of character. The matching start position is 1. |
      +-----------+-------------------------------------------------------------------------------------------------------------------------------------------+
      | nth       | Number of pattern matching times. The minimum value is 1.                                                                                 |
      +-----------+-------------------------------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001145494799__li19579245181414:

   DBMS_LOB.COMPARE

   This function compares two LOBs or a certain part of two LOBs.

   -  If the two parts are equal, **0** is returned. Otherwise, a non-zero value is returned.
   -  If the first CLOB is smaller than the second, **-1** is returned. If the first CLOB is larger than the second, **1** is returned.
   -  If any of the **amount**, **offset_1**, and **offset_2** parameters is invalid, **NULL** is returned. The valid offset range is 1 to LOBMAXSIZE.

   The function prototype of **DBMS_LOB.READ** is:

   ::

      DBMS_LOB.COMPARE (
      lob_1     IN     BLOB,
      lob_2     IN     BLOB,
      amount    IN     INTEGER := DBMS_LOB.LOBMAXSIZE,
      offset_1  IN     INTEGER := 1,
      offset_2  IN     INTEGER := 1)
      RETURN INTEGER;

      DBMS_LOB.COMPARE (
      lob_1     IN     CLOB,
      lob_2     IN     CLOB,
      amount    IN     INTEGER := DBMS_LOB.LOBMAXSIZE,
      offset_1  IN     INTEGER := 1,
      offset_2  IN     INTEGER := 1)
      RETURN INTEGER;

   .. table:: **Table 11** DBMS_LOB.COMPARE interface parameters

      +-----------+-----------------------------------------------------------------------------------------+
      | Parameter | Description                                                                             |
      +===========+=========================================================================================+
      | lob_1     | First LOB descriptor to be compared                                                     |
      +-----------+-----------------------------------------------------------------------------------------+
      | lob_2     | Second LOB descriptor to be compared                                                    |
      +-----------+-----------------------------------------------------------------------------------------+
      | amount    | Number of characters or bytes to be compared. The maximum value is DBMS_LOB.LOBMAXSIZE. |
      +-----------+-----------------------------------------------------------------------------------------+
      | offset_1  | Offset of the first LOB descriptor. The initial position is 1.                          |
      +-----------+-----------------------------------------------------------------------------------------+
      | offset_2  | Offset of the second LOB descriptor. The initial position is 1.                         |
      +-----------+-----------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001145494799__li381175591417:

   DBMS_LOB.SUBSTR

   This function reads the substring of a LOB and returns the number of read bytes or the number of characters. If amount > 1, amount < 32767, offset < 1, or offset > LOBMAXSIZE, **NULL** is returned.

   The function prototype of **DBMS_LOB.SUBSTR** is:

   ::

      DBMS_LOB.SUBSTR (
      lob_loc     IN     BLOB,
      amount      IN     INTEGER := 32767,
      offset      IN     INTEGER := 1)
      RETURN RAW;

      DBMS_LOB.SUBSTR (
      lob_loc    IN     CLOB,
      amount     IN     INTEGER := 32767,
      offset     IN     INTEGER := 1)
      RETURN VARCHAR2;

   .. table:: **Table 12** DBMS_LOB.SUBSTR interface parameters

      +-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter | Description                                                                                                                                                 |
      +===========+=============================================================================================================================================================+
      | lob_loc   | LOB descriptor of the substring to be read. For BLOB, the return value is the number of read bytes. For CLOB, the return value is the number of characters. |
      +-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | offset    | Number of bytes or characters to be read.                                                                                                                   |
      +-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | buffer    | Number of characters or bytes offset from the start position.                                                                                               |
      +-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001145494799__li374125861411:

   DBMS_LOB.TRIM

   This stored procedure truncates the LOB of a specified length. After this stored procedure is executed, the length of the LOB is set to the length specified by the **newlen** parameter. If an empty LOB is truncated, no execution result is displayed. If the specified length is longer than the length of LOB, an exception occurs.

   The function prototype of **DBMS_LOB.TRIM** is:

   ::

      DBMS_LOB.TRIM (
      lob_loc     IN OUT     BLOB,
      newlen      IN         INTEGER);

      DBMS_LOB.TRIM (
      lob_loc    IN          OUT CLOB,
      newlen     IN          INTEGER);

   .. table:: **Table 13** DBMS_LOB.TRIM interface parameters

      +-----------+---------------------------------------------------------------------------------------------------------------------+
      | Parameter | Description                                                                                                         |
      +===========+=====================================================================================================================+
      | lob_loc   | BLOB type object to be read                                                                                         |
      +-----------+---------------------------------------------------------------------------------------------------------------------+
      | newlen    | After truncation, the new LOB length for BLOB is in the unit of byte and that for CLOB is in the unit of character. |
      +-----------+---------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001145494799__li145817213157:

   DBMS_LOB.CREATETEMPORARY

   This stored procedure creates a temporary BLOB or CLOB and is used only for syntax compatibility.

   The function prototype of **DBMS_LOB.CREATETEMPORARY** is:

   ::

      DBMS_LOB.CREATETEMPORARY (
      lob_loc    IN OUT      BLOB,
      cache      IN          BOOLEAN,
      dur        IN          INTEGER);

      DBMS_LOB.CREATETEMPORARY (
      lob_loc    IN OUT     CLOB,
      cache      IN         BOOLEAN,
      dur        IN         INTEGER);

   .. table:: **Table 14** DBMS_LOB.CREATETEMPORARY interface parameters

      ========= =====================================================
      Parameter Description
      ========= =====================================================
      lob_loc   LOB descriptor
      cache     This parameter is used only for syntax compatibility.
      dur       This parameter is used only for syntax compatibility.
      ========= =====================================================

-  .. _en-us_topic_0000001145494799__li194203411159:

   DBMS_LOB.APPEND

   The stored procedure **READ** loads a part of BLOB contents to BUFFER according to the specified length and initial position offset.

   The function prototype of **DBMS_LOB.APPEND** is:

   ::

      DBMS_LOB.APPEND (
      dest_lob    IN OUT       BLOB,
      src_lob     IN           BLOB);

      DBMS_LOB.APPEND (
      dest_lob    IN OUT       CLOB,
      src_lob     IN           CLOB);

   .. table:: **Table 15** DBMS_LOB.APPEND interface parameters

      ========= ============================
      Parameter Description
      ========= ============================
      dest_lob  LOB descriptor to be written
      src_lob   LOB descriptor to be read
      ========= ============================

Examples
--------

::

   -- Obtain the length of the character string.
   SELECT DBMS_LOB.GETLENGTH('12345678');

   DECLARE
   myraw  RAW(100);
   amount INTEGER :=2;
   buffer INTEGER :=1;
   begin
   DBMS_LOB.READ('123456789012345',amount,buffer,myraw);
   dbms_output.put_line(myraw);
   end;
   /

   CREATE TABLE blob_Table (t1 blob) DISTRIBUTE BY REPLICATION;
   CREATE TABLE blob_Table_bak (t2 blob) DISTRIBUTE BY REPLICATION;
   INSERT INTO blob_Table VALUES('abcdef');
   INSERT INTO blob_Table_bak VALUES('22222');

   DECLARE
   str varchar2(100) := 'abcdef';
   source raw(100);
   dest blob;
   copyto blob;
   amount int;
   PSV_SQL varchar2(100);
   PSV_SQL1 varchar2(100);
   a int :=1;
   len int;
   BEGIN
   source := utl_raw.cast_to_raw(str);
   amount := utl_raw.length(source);

   PSV_SQL :='select * from blob_Table for update';
   PSV_SQL1 := 'select * from blob_Table_bak for update';

   EXECUTE IMMEDIATE PSV_SQL into dest;
   EXECUTE IMMEDIATE PSV_SQL1 into copyto;

   DBMS_LOB.WRITE(dest, amount, 1, source);
   DBMS_LOB.WRITEAPPEND(dest, amount, source);

   DBMS_LOB.ERASE(dest, a, 1);
   DBMS_OUTPUT.PUT_LINE(a);
   DBMS_LOB.COPY(copyto, dest, amount, 10, 1);
   DBMS_LOB.CLOSE(dest);
   RETURN;
   END;
   /

   --Delete the table.
   DROP TABLE blob_Table;
   DROP TABLE blob_Table_bak;
