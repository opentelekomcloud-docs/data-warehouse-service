:original_name: dws_04_0096.html

.. _dws_04_0096:

Processing Data in a Result Set
===============================

Setting a Result Set Type
-------------------------

Different types of result sets are applicable to different application scenarios. Applications select proper types of result sets based on requirements. Before executing an SQL statement, you must create a statement object. Some methods of creating statement objects can set the type of a result set. :ref:`Table 1 <en-us_topic_0000001510402461__tf4a21db4c1e4442387c08601671ce2f4>` lists result set parameters. The related Connection methods are as follows:

::

   //Create a Statement object. This object will generate a ResultSet object with a specified type and concurrency.
   createStatement(int resultSetType, int resultSetConcurrency);

   //Create a PreparedStatement object. This object will generate a ResultSet object with a specified type and concurrency.
   prepareStatement(String sql, int resultSetType, int resultSetConcurrency);

   //Create a CallableStatement object. This object will generate a ResultSet object with a specified type and concurrency.
   prepareCall(String sql, int resultSetType, int resultSetConcurrency);

.. _en-us_topic_0000001510402461__tf4a21db4c1e4442387c08601671ce2f4:

.. table:: **Table 1** Result set types

   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                                                                                                   |
   +===================================+===============================================================================================================================================================================================================================================================================================================================================+
   | resultSetType                     | Indicates the type of a result set. There are three types of result sets:                                                                                                                                                                                                                                                                     |
   |                                   |                                                                                                                                                                                                                                                                                                                                               |
   |                                   | -  **ResultSet.TYPE_FORWARD_ONLY**: The ResultSet object can only be navigated forward. It is the default value.                                                                                                                                                                                                                              |
   |                                   | -  **ResultSet.TYPE_SCROLL_SENSITIVE**: You can view the modified result by scrolling to the modified row.                                                                                                                                                                                                                                    |
   |                                   | -  **ResultSet.TYPE_SCROLL_INSENSITIVE**: The ResultSet object is insensitive to changes in the underlying data source.                                                                                                                                                                                                                       |
   |                                   |                                                                                                                                                                                                                                                                                                                                               |
   |                                   | .. note::                                                                                                                                                                                                                                                                                                                                     |
   |                                   |                                                                                                                                                                                                                                                                                                                                               |
   |                                   |    After a result set has obtained data from the database, the result set is insensitive to data changes made by other transactions, even if the result set type is **ResultSet.TYPE_SCROLL_SENSITIVE**. To obtain up-to-date data of the record pointed by the cursor from the database, call the refreshRow() method in a ResultSet object. |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | resultSetConcurrency              | Indicates the concurrency type of a result set. There are two types of concurrency.                                                                                                                                                                                                                                                           |
   |                                   |                                                                                                                                                                                                                                                                                                                                               |
   |                                   | -  **ResultSet.CONCUR_READ_ONLY**: The data in a result set cannot be updated except that an updated statement has been created in the result set data.                                                                                                                                                                                       |
   |                                   | -  **ResultSet.CONCUR_UPDATEABLE**: changeable result set. The concurrency type for a result set object can be updated if the result set is scrollable.                                                                                                                                                                                       |
   +-----------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Positioning a Cursor in a Result Set
------------------------------------

ResultSet objects include a cursor pointing to the current data row. The cursor is initially positioned before the first row. The next method moves the cursor to the next row from its current position. When a ResultSet object does not have a next row, a call to the next method returns **false**. Therefore, this method is used in the while loop for result set iteration. However, the JDBC driver provides more cursor positioning methods for scrollable result sets, which allows positioning cursor in the specified row. :ref:`Table 2 <en-us_topic_0000001510402461__t7c74083b23ab4f64a5f834fed336b1d3>` lists these methods.

.. _en-us_topic_0000001510402461__t7c74083b23ab4f64a5f834fed336b1d3:

.. table:: **Table 2** Methods for positioning a cursor in a result set

   +---------------+--------------------------------------------------------------+
   | Method        | Description                                                  |
   +===============+==============================================================+
   | next()        | Moves cursor to the next row from its current position.      |
   +---------------+--------------------------------------------------------------+
   | previous()    | Moves cursor to the previous row from its current position.  |
   +---------------+--------------------------------------------------------------+
   | beforeFirst() | Places cursor before the first row.                          |
   +---------------+--------------------------------------------------------------+
   | afterLast()   | Places cursor after the last row.                            |
   +---------------+--------------------------------------------------------------+
   | first()       | Places cursor to the first row.                              |
   +---------------+--------------------------------------------------------------+
   | last()        | Places cursor to the last row.                               |
   +---------------+--------------------------------------------------------------+
   | absolute(int) | Places cursor to a specified row.                            |
   +---------------+--------------------------------------------------------------+
   | relative(int) | Moves cursor forward or backward a specified number of rows. |
   +---------------+--------------------------------------------------------------+

Obtaining the cursor position from a result set
-----------------------------------------------

This cursor positioning method will be used to change the cursor position for a scrollable result set. JDBC driver provides a method to obtain the cursor position in a result set. :ref:`Table 3 <en-us_topic_0000001510402461__tfd5153ee41c24ba2b4651df5d3284791>` lists the method.

.. _en-us_topic_0000001510402461__tfd5153ee41c24ba2b4651df5d3284791:

.. table:: **Table 3** Method for obtaining the cursor position in a result set

   =============== ==================================================
   Method          Description
   =============== ==================================================
   isFirst()       Checks whether the cursor is in the first row.
   isLast()        Checks whether the cursor is in the last row.
   isBeforeFirst() Checks whether the cursor is before the first row.
   isAfterLast()   Checks whether the cursor is after the last row.
   getRow()        Gets the current row number of the cursor.
   =============== ==================================================

Obtaining data from a result set
--------------------------------

ResultSet objects provide a variety of methods to obtain data from a result set. :ref:`Table 4 <en-us_topic_0000001510402461__te04a9fc2304947d4aea4b6d93aaae1b5>` lists the common methods for obtaining data. If you want to know more about other methods, see JDK official documents.

.. _en-us_topic_0000001510402461__te04a9fc2304947d4aea4b6d93aaae1b5:

.. table:: **Table 4** Common methods for obtaining data from a result set

   +--------------------------------------+------------------------------------------------------------------------------------------------+
   | Method                               | Description                                                                                    |
   +======================================+================================================================================================+
   | int getInt(int columnIndex)          | Retrieves the value of the column designated by a column index in the current row as an int.   |
   +--------------------------------------+------------------------------------------------------------------------------------------------+
   | int getInt(String columnLabel)       | Retrieves the value of the column designated by a column label in the current row as an int.   |
   +--------------------------------------+------------------------------------------------------------------------------------------------+
   | String getString(int columnIndex)    | Retrieves the value of the column designated by a column index in the current row as a String. |
   +--------------------------------------+------------------------------------------------------------------------------------------------+
   | String getString(String columnLabel) | Retrieves the value of the column designated by a column label in the current row as a String. |
   +--------------------------------------+------------------------------------------------------------------------------------------------+
   | Date getDate(int columnIndex)        | Retrieves the value of the column designated by a column index in the current row as a Date.   |
   +--------------------------------------+------------------------------------------------------------------------------------------------+
   | Date getDate(String columnLabel)     | Retrieves the value of the column designated by a column name in the current row as a Date.    |
   +--------------------------------------+------------------------------------------------------------------------------------------------+
