:original_name: dws_04_0554.html

.. _dws_04_0554:

UTL_RAW
=======

Related Interfaces
------------------

:ref:`Table 1 <en-us_topic_0000001811609461__tbb333794a6d24e4b909eba1954dfe32e>` provides all interfaces supported by the **UTL_RAW** package.

.. _en-us_topic_0000001811609461__tbb333794a6d24e4b909eba1954dfe32e:

.. table:: **Table 1** UTL_RAW

   +-----------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------+
   | API                                                                                                       | Description                                                           |
   +===========================================================================================================+=======================================================================+
   | :ref:`UTL_RAW.CAST_FROM_BINARY_INTEGER <en-us_topic_0000001811609461__t8f43a09ba6a544b19feb45523271d118>` | Converts an INTEGER type value to a binary representation (RAW type). |
   +-----------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------+
   | :ref:`UTL_RAW.CAST_TO_BINARY_INTEGER <en-us_topic_0000001811609461__t5d8a43c2658f486181cf6003be0a5559>`   | Converts a binary representation (RAW type) to an INTEGER type value. |
   +-----------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------+
   | :ref:`UTL_RAW.LENGTH <en-us_topic_0000001811609461__ldd90efc6657941b09f2374d918657451>`                   | Obtains the length of the RAW type object.                            |
   +-----------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------+
   | :ref:`UTL_RAW.CAST_TO_RAW <en-us_topic_0000001811609461__l96640547e66f4cfaac327a3d86b71404>`              | Converts a VARCHAR2 type value to a binary expression (RAW type).     |
   +-----------------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------+

.. important::

   The external representation of the RAW type data is hexadecimal and its internal storage form is binary. For example, the representation of the **RAW** type data **11001011** is 'CB'. The input of the actual type conversion is 'CB'.

-  UTL_RAW.CAST_FROM_BINARY_INTEGER

   The stored procedure **CAST_FROM_BINARY_INTEGER** converts an **INTEGER** type value to a binary representation (**RAW** type).

   The **UTL_RAW.CAST_FROM_BINARY_INTEGER** function prototype is:

   ::

      UTL_RAW.CAST_FROM_BINARY_INTEGER (
      n           IN  INTEGER,
      endianess   IN  INTEGER)
      RETURN RAW;

   .. _en-us_topic_0000001811609461__t8f43a09ba6a544b19feb45523271d118:

   .. table:: **Table 2** UTL_RAW.CAST_FROM_BINARY_INTEGER interface parameters

      +-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter | Description                                                                                                                                       |
      +===========+===================================================================================================================================================+
      | n         | Specifies the INTEGER type value to be converted to the RAW type.                                                                                 |
      +-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | endianess | Specifies the **INTEGER** type value **1** or **2** of the byte sequence. (**1** indicates **BIG_ENDIAN** and **2** indicates **LITTLE-ENDIAN**.) |
      +-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+

-  UTL_RAW.CAST_TO_BINARY_INTEGER

   The stored procedure CAST_TO_BINARY_INTEGER converts an INTEGER type value in a binary representation (RAW type) to the INTEGER type.

   The UTL_RAW.CAST_TO_BINARY_INTEGER function prototype is:

   ::

      UTL_RAW.CAST_TO_BINARY_INTEGER (
      r          IN  RAW,
      endianess  IN  INTEGER)
      RETURN BINARY_INTEGER;

   .. _en-us_topic_0000001811609461__t5d8a43c2658f486181cf6003be0a5559:

   .. table:: **Table 3** UTL_RAW.CAST_TO_BINARY_INTEGER interface parameters

      +-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter | Description                                                                                                                                       |
      +===========+===================================================================================================================================================+
      | r         | Specifies an INTEGER type value in a binary representation (RAW type).                                                                            |
      +-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+
      | endianess | Specifies the **INTEGER** type value **1** or **2** of the byte sequence. (**1** indicates **BIG_ENDIAN** and **2** indicates **LITTLE-ENDIAN**.) |
      +-----------+---------------------------------------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001811609461__ldd90efc6657941b09f2374d918657451:

   UTL_RAW.LENGTH

   The stored procedure LENGTH returns the length of a RAW type object.

   The UTL_RAW.LENGTH function prototype is:

   ::

      UTL_RAW.LENGTH(
      r      IN RAW)
      RETURN INTEGER;

   .. table:: **Table 4** UTL_RAW.LENGTH interface parameters

      ========= ============================
      Parameter Description
      ========= ============================
      r         Specifies a RAW type object.
      ========= ============================

-  .. _en-us_topic_0000001811609461__l96640547e66f4cfaac327a3d86b71404:

   UTL_RAW.CAST_TO_RAW

   The stored procedure CAST_TO_RAW converts a VARCHAR2 type object to the RAW type.

   The UTL_RAW.CAST_TO_RAW function prototype is:

   ::

      UTL_RAW.CAST_TO_RAW(
      c      IN VARCHAR2)
      RETURN RAW;

   .. table:: **Table 5** UTL_RAW.CAST_TO_RAW interface parameters

      ========= =================================================
      Parameter Description
      ========= =================================================
      c         Specifies a VARCHAR2 type object to be converted.
      ========= =================================================

Example
-------

Perform operations on RAW data in a stored procedure:

::

   CREATE OR REPLACE PROCEDURE proc_raw
   AS
   str varchar2(100) := 'abcdef';
   source raw(100);
   amount integer;
   BEGIN
   source := utl_raw.cast_to_raw(str);--Convert the type.
   amount := utl_raw.length(source);--Obtain the length.
   dbms_output.put_line(amount);
   END;
   /

Call the stored procedure:

::

   CALL proc_raw();
