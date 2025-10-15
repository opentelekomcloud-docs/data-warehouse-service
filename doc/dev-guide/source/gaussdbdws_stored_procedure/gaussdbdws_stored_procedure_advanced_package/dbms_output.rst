:original_name: dws_04_0553.html

.. _dws_04_0553:

DBMS_OUTPUT
===========

Related Interfaces
------------------

:ref:`Table 1 <en-us_topic_0000001764491700__table1347845844912>` provides all interfaces supported by the **DBMS_OUTPUT** package.

.. _en-us_topic_0000001764491700__table1347845844912:

.. table:: **Table 1** DBMS_OUTPUT

   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | API                                                                                           | Description                                                                                                                                                                                                                                           |
   +===============================================================================================+=======================================================================================================================================================================================================================================================+
   | :ref:`DBMS_OUTPUT.PUT_LINE <en-us_topic_0000001764491700__t959bb9f8e0a54b888b7a5192cc8bbc93>` | Outputs the specified text. The text length cannot exceed 32,767 bytes.                                                                                                                                                                               |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_OUTPUT.PUT <en-us_topic_0000001764491700__td94dd0ce396b4c16a53432e1db6ab74c>`      | Outputs the specified text to the front of the specified text without adding a line break. The text length cannot exceed 32,767 bytes.                                                                                                                |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_OUTPUT.ENABLE <en-us_topic_0000001764491700__tcd7564a05a764cfd9cf5a54a0f5bdc86>`   | Sets the buffer area size. If this interface is not specified, the maximum buffer size is 20,000 bytes and the minimum buffer size is 2,000 bytes. If the specified buffer size is less than 2,000 bytes, the default minimum buffer size is applied. |
   +-----------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-  DBMS_OUTPUT.PUT_LINE

The **PUT_LINE** procedure writes a row of text carrying a line end symbol in the buffer. The **DBMS_OUTPUT.PUT_LINE** function prototype is:

::

   DBMS_OUTPUT.PUT_LINE (
   item IN VARCHAR2);

.. _en-us_topic_0000001764491700__t959bb9f8e0a54b888b7a5192cc8bbc93:

.. table:: **Table 2** **DBMS_OUTPUT.PUT_LINE** interface parameters

   ========= ==================================================
   Parameter Description
   ========= ==================================================
   item      Specifies the text that was written to the buffer.
   ========= ==================================================

-  DBMS_OUTPUT.PUT

The stored procedure **PUT** outputs the specified text to the front of the specified text without adding a linefeed. The **DBMS_OUTPUT.PUT** function prototype is:

::

   DBMS_OUTPUT.PUT (
   item IN VARCHAR2);

.. _en-us_topic_0000001764491700__td94dd0ce396b4c16a53432e1db6ab74c:

.. table:: **Table 3** **DBMS_OUTPUT.PUT** interface parameters

   ========= ==========================================================
   Parameter Description
   ========= ==========================================================
   item      Specifies the text that was written to the specified text.
   ========= ==========================================================

-  DBMS_OUTPUT.ENABLE

The stored procedure **ENABLE** sets the output buffer size. If the size is not specified, it contains a maximum of 20,000 bytes. The **DBMS_OUTPUT.ENABLE** function prototype is:

::

   DBMS_OUTPUT.ENABLE (
   buf IN INTEGER);

.. _en-us_topic_0000001764491700__tcd7564a05a764cfd9cf5a54a0f5bdc86:

.. table:: **Table 4** DBMS_OUTPUT.ENABLE interface parameters

   ========= ==========================
   Parameter Description
   ========= ==========================
   buf       Sets the buffer area size.
   ========= ==========================

Examples
--------

::

   BEGIN
       DBMS_OUTPUT.ENABLE(50);
       DBMS_OUTPUT.PUT ('hello, ');
       DBMS_OUTPUT.PUT_LINE('database!');-- Displaying "hello, database!"
   END;
   /
