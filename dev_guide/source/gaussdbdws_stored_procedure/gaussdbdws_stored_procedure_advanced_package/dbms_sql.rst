:original_name: dws_04_0556.html

.. _dws_04_0556:

DBMS_SQL
========

Related Interfaces
------------------

:ref:`Table 1 <en-us_topic_0000001460882592__table11636114145319>` lists interfaces supported by the **DBMS_SQL** package.

.. _en-us_topic_0000001460882592__table11636114145319:

.. table:: **Table 1** DBMS_SQL

   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | API                                                                                   | Description                                                                                             |
   +=======================================================================================+=========================================================================================================+
   | :ref:`DBMS_SQL.OPEN_CURSOR <en-us_topic_0000001460882592__li1531162015334>`           | Opens a cursor.                                                                                         |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.CLOSE_CURSOR <en-us_topic_0000001460882592__li156795412510>`           | Closes an open cursor.                                                                                  |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.PARSE <en-us_topic_0000001460882592__li2228125392012>`                 | Transmits a group of SQL statements to a cursor. Currently, only the **SELECT** statement is supported. |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.EXECUTE <en-us_topic_0000001460882592__li55721712815>`                 | Performs a set of dynamically defined operations on the cursor.                                         |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.FETCHE_ROWS <en-us_topic_0000001460882592__li126241517289>`            | Reads a row of cursor data.                                                                             |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.DEFINE_COLUMN <en-us_topic_0000001460882592__li93441630193318>`        | Dynamically defines a column.                                                                           |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.DEFINE_COLUMN_CHAR <en-us_topic_0000001460882592__li15241105533618>`   | Dynamically defines a column of the CHAR type.                                                          |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.DEFINE_COLUMN_INT <en-us_topic_0000001460882592__li1061171414373>`     | Dynamically defines a column of the INT type.                                                           |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.DEFINE_COLUMN_LONG <en-us_topic_0000001460882592__li3604521153710>`    | Dynamically defines a column of the LONG type.                                                          |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.DEFINE_COLUMN_RAW <en-us_topic_0000001460882592__li17405103123711>`    | Dynamically defines a column of the RAW type.                                                           |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.DEFINE_COLUMN_TEXT <en-us_topic_0000001460882592__li163691742193714>`  | Dynamically defines a column of the TEXT type.                                                          |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.DEFINE_COLUMN_UNKNOWN <en-us_topic_0000001460882592__li8476651143718>` | Dynamically defines a column of an unknown type.                                                        |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.COLUMN_VALUE <en-us_topic_0000001460882592__li182631611152817>`        | Reads a dynamically defined column value.                                                               |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.COLUMN_VALUE_CHAR <en-us_topic_0000001460882592__li55491765289>`       | Reads a dynamically defined column value of the CHAR type.                                              |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.COLUMN_VALUE_INT <en-us_topic_0000001460882592__li169604123012>`       | Reads a dynamically defined column value of the INT type.                                               |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.COLUMN_VALUE_LONG <en-us_topic_0000001460882592__li9209325173117>`     | Reads a dynamically defined column value of the LONG type.                                              |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.COLUMN_VALUE_RAW <en-us_topic_0000001460882592__li1644815212328>`      | Reads a dynamically defined column value of the RAW type.                                               |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.COLUMN_VALUE_TEXT <en-us_topic_0000001460882592__li5561542153219>`     | Reads a dynamically defined column value of the TEXT type.                                              |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.COLUMN_VALUE_UNKNOWN <en-us_topic_0000001460882592__li13946783337>`    | Reads a dynamically defined column value of an unknown type.                                            |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+
   | :ref:`DBMS_SQL.IS_OPEN <en-us_topic_0000001460882592__li17449205852910>`              | Checks whether a cursor is opened.                                                                      |
   +---------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------+

.. note::

   -  You are advised to use **dbms_sql.define_column** and **dbms_sql.column_value** to define columns.
   -  If the size of the result set is greater than the value of **work_mem**, the result set will be flushed to disk. The value of **work_mem** must be no greater than 512 MB.

-  .. _en-us_topic_0000001460882592__li1531162015334:

   DBMS_SQL.OPEN_CURSOR

   This function opens a cursor and is the prerequisite for the subsequent dbms_sql operations. This function does not transfer any parameter. It automatically generates cursor IDs in an ascending order and returns values to integer variables.

   The function prototype of **DBMS_SQL.OPEN_CURSOR** is:

   ::

      DBMS_SQL.OPEN_CURSOR (
      )
      RETURN INTEGER;

-  .. _en-us_topic_0000001460882592__li156795412510:

   DBMS_SQL.CLOSE_CURSOR

   This function closes a cursor. It is the end of each dbms_sql operation. If this function is not invoked when the stored procedure ends, the memory is still occupied by the cursor. Therefore, remember to close a cursor when you do not need to use it. If an exception occurs, the stored procedure exits but the cursor is not closed. Therefore, you are advised to include this interface in the exception handling of the stored procedure.

   The function prototype of **DBMS_SQL.CLOSE_CURSOR** is:

   ::

      DBMS_SQL.CLOSE_CURSOR (
      cursorid     IN INTEGER
      )
      RETURN INTEGER;

   .. table:: **Table 2** DBMS_SQL.CLOSE_CURSOR interface parameters

      ============== =============================
      Parameter Name Description
      ============== =============================
      cursorid       ID of the cursor to be closed
      ============== =============================

-  .. _en-us_topic_0000001460882592__li2228125392012:

   DBMS_SQL.PARSE

   This function parses the query statement of a given cursor. The input query statement is executed immediately. Currently, only the **SELECT** query statement can be parsed. The statement parameters can be transferred only through the TEXT type. The length cannot exceed 1 GB.

   The function prototype of **DBMS_SQL.PARSE** is:

   ::

      DBMS_SQL.PARSE (
      cursorid     IN INTEGER,
      query_string IN TEXT,
      label        IN INTEGER
      )
      RETURN BOOLEAN;

   .. table:: **Table 3** DBMS_SQL.PARSE interface parameters

      +----------------+--------------------------------------------------------------+
      | Parameter Name | Description                                                  |
      +================+==============================================================+
      | cursorid       | ID of the cursor whose query statement is parsed             |
      +----------------+--------------------------------------------------------------+
      | query_string   | Query statements to be parsed                                |
      +----------------+--------------------------------------------------------------+
      | language_flag  | Version language number. Currently, only **1** is supported. |
      +----------------+--------------------------------------------------------------+

-  .. _en-us_topic_0000001460882592__li55721712815:

   DBMS_SQL.EXECUTE

   This function executes a given cursor. This function receives a cursor ID. The obtained data after is used for subsequent operations. Currently, only the **SELECT** query statement can be executed.

   The function prototype of **DBMS_SQL.EXECUTE** is:

   ::

      DBMS_SQL.EXECUTE(
      cursorid     IN INTEGER,
      )
      RETURN INTEGER;

   .. table:: **Table 4** DBMS_SQL.EXECUTE interface parameters

      ============== ================================================
      Parameter Name Description
      ============== ================================================
      cursorid       ID of the cursor whose query statement is parsed
      ============== ================================================

-  .. _en-us_topic_0000001460882592__li126241517289:

   DBMS_SQL.FETCHE_ROWS

   This function returns the number of data rows that meet query conditions. Each time the interface is executed, the system obtains a set of new rows until all data is read.

   The function prototype of **DBMS_SQL.FETCHE_ROWS** is:

   ::

      DBMS_SQL.FETCHE_ROWS(
      cursorid     IN INTEGER,
      )
      RETURN INTEGER;

   .. table:: **Table 5** DBMS_SQL.FETCH_ROWS interface parameters

      ============== ===============================
      Parameter Name Description
      ============== ===============================
      curosorid      ID of the cursor to be executed
      ============== ===============================

-  .. _en-us_topic_0000001460882592__li93441630193318:

   DBMS_SQL.DEFINE_COLUMN

   This function defines columns returned from a given cursor and can be used only for the cursors defined by **SELECT**. The defined columns are identified by the relative positions in the query list. The data type of the input variable determines the column type.

   The function prototype of **DBMS_SQL.DEFINE_COLUMN** is:

   ::

      DBMS_SQL.DEFINE_COLUMN(
      cursorid     IN INTEGER,
      position     IN INTEGER,
      column_ref   IN ANYELEMENT,
      column_size     IN INTEGER default 1024
      )
      RETURN INTEGER;

   .. table:: **Table 6** DBMS_SQL.DEFINE_COLUMN interface parameters

      +----------------+----------------------------------------------------------------------------------------------------------------------+
      | Parameter Name | Description                                                                                                          |
      +================+======================================================================================================================+
      | cursorid       | ID of the cursor to be executed                                                                                      |
      +----------------+----------------------------------------------------------------------------------------------------------------------+
      | position       | Position of a dynamically defined column in the query                                                                |
      +----------------+----------------------------------------------------------------------------------------------------------------------+
      | column_ref     | Variable of any type. You can select an appropriate interface to dynamically define columns based on variable types. |
      +----------------+----------------------------------------------------------------------------------------------------------------------+
      | column_size    | Length of a defined column                                                                                           |
      +----------------+----------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001460882592__li15241105533618:

   DBMS_SQL.DEFINE_COLUMN_CHAR

   This function defines columns of the CHAR type returned from a given cursor and can be used only for the cursors defined by **SELECT**. The defined columns are identified by the relative positions in the query list. The data type of the input variable determines the column type.

   The function prototype of **DBMS_SQL.DEFINE_COLUMN_CHAR** is:

   ::

      DBMS_SQL.DEFINE_COLUMN_CHAR(
      cursorid     IN INTEGER,
      position     IN INTEGER,
      column       IN TEXT,
      column_size     IN INTEGER
      )
      RETURN INTEGER;

   .. table:: **Table 7** DBMS_SQL.DEFINE_COLUMN_CHAR interface parameters

      ============== =====================================================
      Parameter Name Description
      ============== =====================================================
      cursorid       ID of the cursor to be executed
      position       Position of a dynamically defined column in the query
      column         Parameter to be defined
      column_size    Length of a dynamically defined column
      ============== =====================================================

-  .. _en-us_topic_0000001460882592__li1061171414373:

   DBMS_SQL.DEFINE_COLUMN_INT

   This function defines columns of the INT type returned from a given cursor and can be used only for the cursors defined by **SELECT**. The defined columns are identified by the relative positions in the query list. The data type of the input variable determines the column type.

   The function prototype of **DBMS_SQL.DEFINE_COLUMN_INT** is:

   ::

      DBMS_SQL.DEFINE_COLUMN_INT(
      cursorid     IN INTEGER,
      position     IN INTEGER
      )
      RETURN INTEGER;

   .. table:: **Table 8** DBMS_SQL.DEFINE_COLUMN_INT interface parameters

      ============== =====================================================
      Parameter Name Description
      ============== =====================================================
      cursorid       ID of the cursor to be executed
      position       Position of a dynamically defined column in the query
      ============== =====================================================

-  .. _en-us_topic_0000001460882592__li3604521153710:

   DBMS_SQL.DEFINE_COLUMN_LONG

   This function defines columns of a long type (not LONG) returned from a given cursor and can be used only for the cursors defined by **SELECT**. The defined columns are identified by the relative positions in the query list. The data type of the input variable determines the column type. The maximum size of a long column is 1 GB.

   The function prototype of **DBMS_SQL.DEFINE_COLUMN_LONG** is:

   ::

      DBMS_SQL.DEFINE_COLUMN_LONG(
      cursorid     IN INTEGER,
      position     IN INTEGER
      )
      RETURN INTEGER;

   .. table:: **Table 9** DBMS_SQL.DEFINE_COLUMN_LONG interface parameters

      ============== =====================================================
      Parameter Name Description
      ============== =====================================================
      cursorid       ID of the cursor to be executed
      position       Position of a dynamically defined column in the query
      ============== =====================================================

-  .. _en-us_topic_0000001460882592__li17405103123711:

   DBMS_SQL.DEFINE_COLUMN_RAW

   This function defines columns of the RAW type returned from a given cursor and can be used only for the cursors defined by **SELECT**. The defined columns are identified by the relative positions in the query list. The data type of the input variable determines the column type.

   The function prototype of **DBMS_SQL.DEFINE_COLUMN_RAW** is:

   ::

      DBMS_SQL.DEFINE_COLUMN_RAW(
      cursorid     IN INTEGER,
      position     IN INTEGER,
      column       IN BYTEA,
      column_size     IN INTEGER
      )
      RETURN INTEGER;

   .. table:: **Table 10** DBMS_SQL.DEFINE_COLUMN_RAW interface parameters

      ============== =====================================================
      Parameter Name Description
      ============== =====================================================
      cursorid       ID of the cursor to be executed
      position       Position of a dynamically defined column in the query
      column         Parameter of the RAW type
      column_size    Column length
      ============== =====================================================

-  .. _en-us_topic_0000001460882592__li163691742193714:

   DBMS_SQL.DEFINE_COLUMN_TEXT

   This function defines columns of the TEXT type returned from a given cursor and can be used only for the cursors defined by **SELECT**. The defined columns are identified by the relative positions in the query list. The data type of the input variable determines the column type.

   The function prototype of **DBMS_SQL.DEFINE_COLUMN_TEXT** is:

   ::

      DBMS_SQL.DEFINE_COLUMN_CHAR(
      cursorid     IN INTEGER,
      position     IN INTEGER,
      max_size     IN INTEGER
      )
      RETURN INTEGER;

   .. table:: **Table 11** DBMS_SQL.DEFINE_COLUMN_TEXT interface parameters

      ============== =====================================================
      Parameter Name Description
      ============== =====================================================
      cursorid       ID of the cursor to be executed
      position       Position of a dynamically defined column in the query
      max_size       Maximum length of the defined TEXT type
      ============== =====================================================

-  .. _en-us_topic_0000001460882592__li8476651143718:

   DBMS_SQL.DEFINE_COLUMN_UNKNOWN

   This function processes columns of unknown data types returned from a given cursor and is used only for the system to report an error and exist when the type cannot be identified.

   The function prototype of **DBMS_SQL.DEFINE_COLUMN_UNKNOWN** is:

   ::

      DBMS_SQL.DEFINE_COLUMN_CHAR(
      cursorid     IN INTEGER,
      position     IN INTEGER,
      column       IN TEXT
      )
      RETURN INTEGER;

   .. table:: **Table 12** DBMS_SQL.DEFINE_COLUMN_UNKNOWN interface parameters

      ============== =====================================================
      Parameter Name Description
      ============== =====================================================
      cursorid       ID of the cursor to be executed
      position       Position of a dynamically defined column in the query
      column         Dynamically defined parameter
      ============== =====================================================

-  .. _en-us_topic_0000001460882592__li182631611152817:

   DBMS_SQL.COLUMN_VALUE

   This function returns the cursor element value specified by a cursor and accesses the data obtained by DBMS_SQL.FETCH_ROWS.

   The function prototype of **DBMS_SQL.COLUMN_VALUE** is:

   ::

      DBMS_SQL.COLUMN_VALUE(
      cursorid                 IN    INTEGER,
      position                 IN    INTEGER,
      column_value             INOUT ANYELEMENT
      )
      RETURN ANYELEMENT;

   .. table:: **Table 13** DBMS_SQL.COLUMN_VALUE interface parameters

      ============== =====================================================
      Parameter Name Description
      ============== =====================================================
      cursorid       ID of the cursor to be executed
      position       Position of a dynamically defined column in the query
      column_value   Return value of a defined column
      ============== =====================================================

-  .. _en-us_topic_0000001460882592__li55491765289:

   DBMS_SQL.COLUMN_VALUE_CHAR

   This function returns the value of the CHAR type in a specified position of a cursor and accesses the data obtained by DBMS_SQL.FETCH_ROWS.

   The function prototype of **DBMS_SQL.COLUMN_VALUE_CHAR** is:

   ::

      DBMS_SQL.COLUMN_VALUE_CHAR(
      cursorid                 IN    INTEGER,
      position                 IN    INTEGER,
      column_value             INOUT CHARACTER,
      err_num                  INOUT NUMERIC default 0,
      actual_length            INOUT INTEGER default 1024
      )
      RETURN RECORD;

   .. table:: **Table 14** DBMS_SQL.COLUMN_VALUE_CHAR interface parameters

      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter Name | Description                                                                                                                                |
      +================+============================================================================================================================================+
      | cursorid       | ID of the cursor to be executed                                                                                                            |
      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | position       | Position of a dynamically defined column in the query                                                                                      |
      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | column_value   | Return value                                                                                                                               |
      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | err_num        | Error No. It is an output parameter and the argument must be a variable. Currently, the output value is **-1** regardless of the argument. |
      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | actual_length  | Length of a return value                                                                                                                   |
      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001460882592__li169604123012:

   DBMS_SQL.COLUMN_VALUE_INT

   This function returns the value of the INT type in a specified position of a cursor and accesses the data obtained by DBMS_SQL.FETCH_ROWS. The function prototype of **DBMS_SQL.COLUMN_VALUE_INT** is:

   ::

      DBMS_SQL.COLUMN_VALUE_INT(
      cursorid                 IN    INTEGER,
      position                 IN    INTEGER
      )
      RETURN INTEGER;

   .. table:: **Table 15** DBMS_SQL.COLUMN_VALUE_INT interface parameters

      ============== =====================================================
      Parameter Name Description
      ============== =====================================================
      cursorid       ID of the cursor to be executed
      position       Position of a dynamically defined column in the query
      ============== =====================================================

-  .. _en-us_topic_0000001460882592__li9209325173117:

   DBMS_SQL.COLUMN_VALUE_LONG

   This function returns the value of a long type (not LONG or BIGINT) in a specified position of a cursor and accesses the data obtained by DBMS_SQL.FETCH_ROWS.

   The function prototype of **DBMS_SQL.COLUMN_VALUE_LONG** is:

   ::

      DBMS_SQL.COLUMN_VALUE_LONG(
      cursorid                 IN    INTEGER,
      position                 IN    INTEGER,
      length                   IN    INTEGER,
      off_set                  IN    INTEGER,
      column_value             INOUT TEXT,
      actual_length            INOUT INTEGER default 1024
      )
      RETURN RECORD;

   .. table:: **Table 16** DBMS_SQL.COLUMN_VALUE_LONG interface parameters

      ============== =====================================================
      Parameter Name Description
      ============== =====================================================
      cursorid       ID of the cursor to be executed
      position       Position of a dynamically defined column in the query
      length         Length of a return value
      off_set        Start position of a return value
      column_value   Return value
      actual_length  Length of a return value
      ============== =====================================================

-  .. _en-us_topic_0000001460882592__li1644815212328:

   DBMS_SQL.COLUMN_VALUE_RAW

   This function returns the value of the RAW type in a specified position of a cursor and accesses the data obtained by DBMS_SQL.FETCH_ROWS.

   The function prototype of **DBMS_SQL.COLUMN_VALUE_RAW** is:

   ::

      DBMS_SQL.COLUMN_VALUE_RAW(
      cursorid                 IN    INTEGER,
      position                 IN    INTEGER,
      column_value             INOUT BYTEA,
      err_num                  INOUT NUMERIC default 0,
      actual_length            INOUT INTEGER default 1024
      )
      RETURN RECORD;

   .. table:: **Table 17** DBMS_SQL.COLUMN_VALUE_RAW interface parameters

      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | Parameter Name | Description                                                                                                                                |
      +================+============================================================================================================================================+
      | cursorid       | ID of the cursor to be executed                                                                                                            |
      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | position       | Position of a dynamically defined column in the query                                                                                      |
      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | column_value   | Returned column value                                                                                                                      |
      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | err_num        | Error No. It is an output parameter and the argument must be a variable. Currently, the output value is **-1** regardless of the argument. |
      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+
      | actual_length  | Length of a return value. The value longer than this length will be truncated.                                                             |
      +----------------+--------------------------------------------------------------------------------------------------------------------------------------------+

-  .. _en-us_topic_0000001460882592__li5561542153219:

   DBMS_SQL.COLUMN_VALUE_TEXT

   This function returns the value of the TEXT type in a specified position of a cursor and accesses the data obtained by DBMS_SQL.FETCH_ROWS.

   The function prototype of **DBMS_SQL.COLUMN_VALUE_TEXT** is:

   ::

      DBMS_SQL.COLUMN_VALUE_TEXT(
      cursorid                 IN    INTEGER,
      position                 IN    INTEGER
      )
      RETURN TEXT;

   .. table:: **Table 18** DBMS_SQL.COLUMN_VALUE_TEXT interface parameters

      ============== =====================================================
      Parameter Name Description
      ============== =====================================================
      cursorid       ID of the cursor to be executed
      position       Position of a dynamically defined column in the query
      ============== =====================================================

-  .. _en-us_topic_0000001460882592__li13946783337:

   DBMS_SQL.COLUMN_VALUE_UNKNOWN

   This function returns the value of an unknown type in a specified position of a cursor. This is an error handling interface when the type is not unknown.

   The function prototype of **DBMS_SQL.COLUMN_VALUE_UNKNOWN** is:

   ::

      DBMS_SQL.COLUMN_VALUE_UNKNOWN(
      cursorid                 IN    INTEGER,
      position                 IN    INTEGER,
      COLUMN_TYPE              IN    TEXT
      )
      RETURN TEXT;

   .. table:: **Table 19** DBMS_SQL.COLUMN_VALUE_UNKNOWN interface parameters

      ============== =====================================================
      Parameter Name Description
      ============== =====================================================
      cursorid       ID of the cursor to be executed
      position       Position of a dynamically defined column in the query
      column_type    Returned parameter type
      ============== =====================================================

-  .. _en-us_topic_0000001460882592__li17449205852910:

   DBMS_SQL.IS_OPEN

This function returns the status of a cursor: **open**, **parse**, **execute**, or **define**. The value is **TRUE**. If the status is unknown, an error is reported. In other cases, the value is **FALSE**.

The function prototype of **DBMS_SQL.IS_OPEN** is:

::

   DBMS_SQL.IS_OPEN(
   cursorid                 IN    INTEGER
   )
   RETURN BOOLEAN;

.. table:: **Table 20** DBMS_SQL.IS_OPEN interface parameters

   ============== ==============================
   Parameter Name Description
   ============== ==============================
   cursorid       ID of the cursor to be queried
   ============== ==============================

Examples
--------

::

   -- Perform operations on RAW data in a stored procedure.
   create or replace procedure pro_dbms_sql_all_02(in_raw raw,v_in int,v_offset int)
   as
   cursorid int;
   v_id int;
   v_info bytea :=1;
   query varchar(2000);
   execute_ret int;
   define_column_ret_raw bytea :='1';
   define_column_ret int;
   begin
   drop table if exists pro_dbms_sql_all_tb1_02 ;
   create table pro_dbms_sql_all_tb1_02(a int ,b blob);
   insert into pro_dbms_sql_all_tb1_02 values(1,HEXTORAW('DEADBEEE'));
   insert into pro_dbms_sql_all_tb1_02 values(2,in_raw);
   query := 'select * from pro_dbms_sql_all_tb1_02 order by 1';
   -- Open a cursor.
   cursorid := dbms_sql.open_cursor();
   -- Compile the cursor.
   dbms_sql.parse(cursorid, query, 1);
   -- Define a column.
   define_column_ret:= dbms_sql.define_column(cursorid,1,v_id);
   define_column_ret_raw:= dbms_sql.define_column_raw(cursorid,2,v_info,10);
   -- Execute the cursor.
   execute_ret := dbms_sql.execute(cursorid);
   loop
   exit when (dbms_sql.fetch_rows(cursorid) <= 0);
   -- Obtain values.
   dbms_sql.column_value(cursorid,1,v_id);
   dbms_sql.column_value_raw(cursorid,2,v_info,v_in,v_offset);
   -- Output the result.
   dbms_output.put_line('id:'|| v_id || ' info:' || v_info);
   end loop;
   -- Close the cursor.
   dbms_sql.close_cursor(cursorid);
   end;
   /
   -- Invoke the stored procedure.
   call pro_dbms_sql_all_02(HEXTORAW('DEADBEEF'),0,1);

   -- Delete the stored procedure.
   DROP PROCEDURE pro_dbms_sql_all_02;
