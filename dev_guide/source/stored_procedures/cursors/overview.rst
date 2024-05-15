:original_name: dws_04_0546.html

.. _dws_04_0546:

Overview
========

To process SQL statements, the stored procedure process assigns a memory segment to store context association. Cursors are handles or pointers to context areas. With cursors, stored procedures can control alterations in context areas.

.. important::

   If JDBC is used to call a stored procedure whose returned value is a cursor, the returned cursor is not available.

Cursors are classified into explicit cursors and implicit cursors. :ref:`Table 1 <en-us_topic_0000001188482094__tc43c8c4ed32a432c864256c4fca6147a>` shows the usage conditions of explicit and implicit cursors for different SQL statements.

.. _en-us_topic_0000001188482094__tc43c8c4ed32a432c864256c4fca6147a:

.. table:: **Table 1** Cursor usage conditions

   ========================================= ====================
   SQL Statement                             Cursor
   ========================================= ====================
   Non-query statements                      Implicit
   Query statements with single-line results Implicit or explicit
   Query statements with multi-line results  Explicit
   ========================================= ====================
