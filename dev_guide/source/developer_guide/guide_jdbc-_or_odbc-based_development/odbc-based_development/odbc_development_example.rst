:original_name: dws_04_0123.html

.. _dws_04_0123:

ODBC Development Example
========================

Code for Common Functions
-------------------------

::

   // The following example shows how to obtain data from GaussDB(DWS) through the ODBC interface.
   // DBtest.c (compile with: libodbc.so)
   #include <stdlib.h>
   #include <stdio.h>
   #include <sqlext.h>
   #ifdef WIN32
   #include <windows.h>
   #endif
   SQLHENV       V_OD_Env;        // Handle ODBC environment
   SQLHSTMT      V_OD_hstmt;      // Handle statement
   SQLHDBC       V_OD_hdbc;       // Handle connection
   char          typename[100];
   SQLINTEGER    value = 100;
   SQLINTEGER    V_OD_erg,V_OD_buffer,V_OD_err,V_OD_id;
   SQLLEN        V_StrLen_or_IndPtr;
   int main(int argc,char *argv[])
   {
         // 1. Apply for an environment handle.
         V_OD_erg = SQLAllocHandle(SQL_HANDLE_ENV,SQL_NULL_HANDLE,&V_OD_Env);
         if ((V_OD_erg != SQL_SUCCESS) && (V_OD_erg != SQL_SUCCESS_WITH_INFO))
         {
              printf("Error AllocHandle\n");
              exit(0);
         }
         // 2. Set environment attributes (version information)
         SQLSetEnvAttr(V_OD_Env, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, 0);
         // 3. Apply for a connection handle.
         V_OD_erg = SQLAllocHandle(SQL_HANDLE_DBC, V_OD_Env, &V_OD_hdbc);
         if ((V_OD_erg != SQL_SUCCESS) && (V_OD_erg != SQL_SUCCESS_WITH_INFO))
         {
              SQLFreeHandle(SQL_HANDLE_ENV, V_OD_Env);
              exit(0);
         }
         // 4. Set connection attributes.
         SQLSetConnectAttr(V_OD_hdbc, SQL_ATTR_AUTOCOMMIT, SQL_AUTOCOMMIT_ON, 0);
   // 5. Connect to the data source. userName and password indicate the username and password for connecting to the database. Set them as needed.
   // If the username and password have been set in the odbc.ini file, you do not need to set userName or password here, retaining "" for them. However, you are not advised to do so because the username and password will be disclosed if the permission for odbc.ini is abused.
         V_OD_erg = SQLConnect(V_OD_hdbc, (SQLCHAR*) "gaussdb", SQL_NTS,
                              (SQLCHAR*) "userName", SQL_NTS,  (SQLCHAR*) "password", SQL_NTS);
         if ((V_OD_erg != SQL_SUCCESS) && (V_OD_erg != SQL_SUCCESS_WITH_INFO))
         {
             printf("Error SQLConnect %d\n",V_OD_erg);
             SQLFreeHandle(SQL_HANDLE_ENV, V_OD_Env);
             exit(0);
         }
         printf("Connected !\n");
         // 6. Set statement attributes
         SQLSetStmtAttr(V_OD_hstmt,SQL_ATTR_QUERY_TIMEOUT,(SQLPOINTER *)3,0);
         // 7. Apply for a statement handle
         SQLAllocHandle(SQL_HANDLE_STMT, V_OD_hdbc, &V_OD_hstmt);
         // 8. Executes an SQL statement directly
         SQLExecDirect(V_OD_hstmt,"drop table IF EXISTS customer_t1",SQL_NTS);
         SQLExecDirect(V_OD_hstmt,"CREATE TABLE customer_t1(c_customer_sk INTEGER, c_customer_name VARCHAR(32));",SQL_NTS);
         SQLExecDirect(V_OD_hstmt,"insert into customer_t1 values(25,'li')",SQL_NTS);
         // 9. Prepare for execution
         SQLPrepare(V_OD_hstmt,"insert into customer_t1 values(?)",SQL_NTS);
         // 10. Bind parameters
         SQLBindParameter(V_OD_hstmt,1,SQL_PARAM_INPUT,SQL_C_SLONG,SQL_INTEGER,0,0,
                          &value,0,NULL);
         // 11. Execute the ready statement
         SQLExecute(V_OD_hstmt);
         SQLExecDirect(V_OD_hstmt,"select id from testtable",SQL_NTS);
         // 12. Obtain the attributes of a certain column in the result set
         SQLColAttribute(V_OD_hstmt,1,SQL_DESC_TYPE_NAME,typename,sizeof(typename),NULL,NULL);
         printf("SQLColAtrribute %s\n",typename);
         // 13. Bind the result set
         SQLBindCol(V_OD_hstmt,1,SQL_C_SLONG, (SQLPOINTER)&V_OD_buffer,150,
                   (SQLLEN *)&V_StrLen_or_IndPtr);
         // 14. Collect data using SQLFetch
         V_OD_erg=SQLFetch(V_OD_hstmt);
         // 15. Obtain and return data using SQLGetData
         while(V_OD_erg != SQL_NO_DATA)
         {
             SQLGetData(V_OD_hstmt,1,SQL_C_SLONG,(SQLPOINTER)&V_OD_id,0,NULL);
             printf("SQLGetData ----ID = %d\n",V_OD_id);
             V_OD_erg=SQLFetch(V_OD_hstmt);
         };
         printf("Done !\n");
         // 16. Disconnect from the data source and release handles
         SQLFreeHandle(SQL_HANDLE_STMT,V_OD_hstmt);
         SQLDisconnect(V_OD_hdbc);
         SQLFreeHandle(SQL_HANDLE_DBC,V_OD_hdbc);
         SQLFreeHandle(SQL_HANDLE_ENV, V_OD_Env);
         return(0);
    }

Code for Batch Processing
-------------------------

::

   /**********************************************************************
   * Set UseBatchProtocol to 1 in the data source and set the database parameter support_batch_bind
   * to on.
   * The CHECK_ERROR command is used to check and print error information.
   * This example is used to interactively obtain the DSN, data volume to be processed, and volume of ignored data from users, and insert required data into the test_odbc_batch_insert table.
   ***********************************************************************/
   #include <stdio.h>
   #include <stdlib.h>
   #include <sql.h>
   #include <sqlext.h>
   #include <string.h>

   #include "util.c"

   void Exec(SQLHDBC hdbc, SQLCHAR* sql)
   {
       SQLRETURN retcode;                  // Return status
       SQLHSTMT hstmt = SQL_NULL_HSTMT;    // Statement handle
       SQLCHAR     loginfo[2048];

       // Allocate Statement Handle
       retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);
       CHECK_ERROR(retcode, "SQLAllocHandle(SQL_HANDLE_STMT)",
                   hstmt, SQL_HANDLE_STMT);

       // Prepare Statement
       retcode = SQLPrepare(hstmt, (SQLCHAR*) sql, SQL_NTS);
       sprintf((char*)loginfo, "SQLPrepare log: %s", (char*)sql);
       CHECK_ERROR(retcode, loginfo, hstmt, SQL_HANDLE_STMT);

       retcode = SQLExecute(hstmt);
       sprintf((char*)loginfo, "SQLExecute stmt log: %s", (char*)sql);
       CHECK_ERROR(retcode, loginfo, hstmt, SQL_HANDLE_STMT);

       retcode = SQLFreeHandle(SQL_HANDLE_STMT, hstmt);
       sprintf((char*)loginfo, "SQLFreeHandle stmt log: %s", (char*)sql);
       CHECK_ERROR(retcode, loginfo, hstmt, SQL_HANDLE_STMT);
   }

   int main ()
   {
       SQLHENV  henv  = SQL_NULL_HENV;
       SQLHDBC  hdbc  = SQL_NULL_HDBC;
       int      batchCount = 1000;
       SQLLEN   rowsCount = 0;
       int      ignoreCount = 0;

       SQLRETURN   retcode;
       SQLCHAR     dsn[1024] = {'\0'};
       SQLCHAR     loginfo[2048];

   // Interactively obtain data source names.
       getStr("Please input your DSN", (char*)dsn, sizeof(dsn), 'N');
   // Interactively obtain the amount of data to be batch processed.
       getInt("batchCount", &batchCount, 'N', 1);
       do
       {
   // Interactively obtain the amount of batch processing data that is not inserted into the database.
           getInt("ignoreCount", &ignoreCount, 'N', 1);
           if (ignoreCount > batchCount)
           {
               printf("ignoreCount(%d) should be less than batchCount(%d)\n", ignoreCount, batchCount);
           }
       }while(ignoreCount > batchCount);

       retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);
       CHECK_ERROR(retcode, "SQLAllocHandle(SQL_HANDLE_ENV)",
                   henv, SQL_HANDLE_ENV);

       // Set ODBC Verion
       retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION,
                                           (SQLPOINTER*)SQL_OV_ODBC3, 0);
       CHECK_ERROR(retcode, "SQLSetEnvAttr(SQL_ATTR_ODBC_VERSION)",
                   henv, SQL_HANDLE_ENV);

       // Allocate Connection
       retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);
       CHECK_ERROR(retcode, "SQLAllocHandle(SQL_HANDLE_DBC)",
                   henv, SQL_HANDLE_DBC);

       // Set Login Timeout
       retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);
       CHECK_ERROR(retcode, "SQLSetConnectAttr(SQL_LOGIN_TIMEOUT)",
                   hdbc, SQL_HANDLE_DBC);

       // Set Auto Commit
       retcode = SQLSetConnectAttr(hdbc, SQL_ATTR_AUTOCOMMIT,
                                           (SQLPOINTER)(1), 0);
       CHECK_ERROR(retcode, "SQLSetConnectAttr(SQL_ATTR_AUTOCOMMIT)",
                   hdbc, SQL_HANDLE_DBC);

       // Connect to DSN
       sprintf(loginfo, "SQLConnect(DSN:%s)", dsn);
       retcode = SQLConnect(hdbc, (SQLCHAR*) dsn, SQL_NTS,
                                  (SQLCHAR*) NULL, 0, NULL, 0);
       CHECK_ERROR(retcode, loginfo, hdbc, SQL_HANDLE_DBC);

       // init table info.
       Exec(hdbc, "drop table if exists test_odbc_batch_insert");
       Exec(hdbc, "create table test_odbc_batch_insert(id int primary key, col varchar2(50))");

   // The following code constructs the data to be inserted based on the data volume entered by users:
       {
           SQLRETURN retcode;
           SQLHSTMT hstmtinesrt = SQL_NULL_HSTMT;
           int          i;
           SQLCHAR      *sql = NULL;
           SQLINTEGER   *ids  = NULL;
           SQLCHAR      *cols = NULL;
           SQLLEN       *bufLenIds = NULL;
           SQLLEN       *bufLenCols = NULL;
           SQLUSMALLINT *operptr = NULL;
           SQLUSMALLINT *statusptr = NULL;
           SQLULEN      process = 0;

   // Data is constructed by column. Each column is stored continuously.
           ids = (SQLINTEGER*)malloc(sizeof(ids[0]) * batchCount);
           cols = (SQLCHAR*)malloc(sizeof(cols[0]) * batchCount * 50);
   // Data size in each row for a column
           bufLenIds = (SQLLEN*)malloc(sizeof(bufLenIds[0]) * batchCount);
           bufLenCols = (SQLLEN*)malloc(sizeof(bufLenCols[0]) * batchCount);
   // Whether this row needs to be processed. The value is SQL_PARAM_IGNORE or SQL_PARAM_PROCEED.
           operptr = (SQLUSMALLINT*)malloc(sizeof(operptr[0]) * batchCount);
           memset(operptr, 0, sizeof(operptr[0]) * batchCount);
   // Processing result of the row
   // Note: In the database, a statement belongs to one transaction. Therefore, data is processed as a unit. That is, either all data is inserted successfully or all data fails to be inserted.
           statusptr = (SQLUSMALLINT*)malloc(sizeof(statusptr[0]) * batchCount);
           memset(statusptr, 88, sizeof(statusptr[0]) * batchCount);

           if (NULL == ids || NULL == cols || NULL == bufLenCols || NULL == bufLenIds)
           {
               fprintf(stderr, "FAILED:\tmalloc data memory failed\n");
               goto exit;
           }

           for (int i = 0; i < batchCount; i++)
           {
               ids[i] = i;
               sprintf(cols + 50 * i, "column test value %d", i);
               bufLenIds[i] = sizeof(ids[i]);
               bufLenCols[i] = strlen(cols + 50 * i);
               operptr[i] = (i < ignoreCount) ? SQL_PARAM_IGNORE : SQL_PARAM_PROCEED;
           }

           // Allocate Statement Handle
           retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmtinesrt);
           CHECK_ERROR(retcode, "SQLAllocHandle(SQL_HANDLE_STMT)",
                       hstmtinesrt, SQL_HANDLE_STMT);

           // Prepare Statement
           sql = (SQLCHAR*)"insert into test_odbc_batch_insert values(?, ?)";
           retcode = SQLPrepare(hstmtinesrt, (SQLCHAR*) sql, SQL_NTS);
           sprintf((char*)loginfo, "SQLPrepare log: %s", (char*)sql);
           CHECK_ERROR(retcode, loginfo, hstmtinesrt, SQL_HANDLE_STMT);

           retcode = SQLSetStmtAttr(hstmtinesrt, SQL_ATTR_PARAMSET_SIZE, (SQLPOINTER)batchCount, sizeof(batchCount));
           CHECK_ERROR(retcode, "SQLSetStmtAttr", hstmtinesrt, SQL_HANDLE_STMT);

           retcode = SQLBindParameter(hstmtinesrt, 1, SQL_PARAM_INPUT, SQL_C_SLONG, SQL_INTEGER, sizeof(ids[0]), 0,&(ids[0]), 0, bufLenIds);
           CHECK_ERROR(retcode, "SQLBindParameter for id", hstmtinesrt, SQL_HANDLE_STMT);

           retcode = SQLBindParameter(hstmtinesrt, 2, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 50, 50, cols, 50, bufLenCols);
           CHECK_ERROR(retcode, "SQLBindParameter for cols", hstmtinesrt, SQL_HANDLE_STMT);

           retcode = SQLSetStmtAttr(hstmtinesrt, SQL_ATTR_PARAMS_PROCESSED_PTR, (SQLPOINTER)&process, sizeof(process));
           CHECK_ERROR(retcode, "SQLSetStmtAttr for SQL_ATTR_PARAMS_PROCESSED_PTR", hstmtinesrt, SQL_HANDLE_STMT);

           retcode = SQLSetStmtAttr(hstmtinesrt, SQL_ATTR_PARAM_STATUS_PTR, (SQLPOINTER)statusptr, sizeof(statusptr[0]) * batchCount);
           CHECK_ERROR(retcode, "SQLSetStmtAttr for SQL_ATTR_PARAM_STATUS_PTR", hstmtinesrt, SQL_HANDLE_STMT);

           retcode = SQLSetStmtAttr(hstmtinesrt, SQL_ATTR_PARAM_OPERATION_PTR, (SQLPOINTER)operptr, sizeof(operptr[0]) * batchCount);
           CHECK_ERROR(retcode, "SQLSetStmtAttr for SQL_ATTR_PARAM_OPERATION_PTR", hstmtinesrt, SQL_HANDLE_STMT);

           retcode = SQLExecute(hstmtinesrt);
           sprintf((char*)loginfo, "SQLExecute stmt log: %s", (char*)sql);
           CHECK_ERROR(retcode, loginfo, hstmtinesrt, SQL_HANDLE_STMT);

           retcode = SQLRowCount(hstmtinesrt, &rowsCount);
           CHECK_ERROR(retcode, "SQLRowCount execution", hstmtinesrt, SQL_HANDLE_STMT);

           if (rowsCount != (batchCount - ignoreCount))
           {
               sprintf(loginfo, "(batchCount - ignoreCount)(%d) != rowsCount(%d)", (batchCount - ignoreCount), rowsCount);
               CHECK_ERROR(SQL_ERROR, loginfo, NULL, SQL_HANDLE_STMT);
           }
           else
           {
               sprintf(loginfo, "(batchCount - ignoreCount)(%d) == rowsCount(%d)", (batchCount - ignoreCount), rowsCount);
               CHECK_ERROR(SQL_SUCCESS, loginfo, NULL, SQL_HANDLE_STMT);
           }

           if (rowsCount != process)
           {
               sprintf(loginfo, "process(%d) != rowsCount(%d)", process, rowsCount);
               CHECK_ERROR(SQL_ERROR, loginfo, NULL, SQL_HANDLE_STMT);
           }
           else
           {
               sprintf(loginfo, "process(%d) == rowsCount(%d)", process, rowsCount);
               CHECK_ERROR(SQL_SUCCESS, loginfo, NULL, SQL_HANDLE_STMT);
           }

           for (int i = 0; i < batchCount; i++)
           {
               if (i < ignoreCount)
               {
                   if (statusptr[i] != SQL_PARAM_UNUSED)
                   {
                       sprintf(loginfo, "statusptr[%d](%d) != SQL_PARAM_UNUSED", i, statusptr[i]);
                       CHECK_ERROR(SQL_ERROR, loginfo, NULL, SQL_HANDLE_STMT);
                   }
               }
               else if (statusptr[i] != SQL_PARAM_SUCCESS)
               {
                   sprintf(loginfo, "statusptr[%d](%d) != SQL_PARAM_SUCCESS", i, statusptr[i]);
                   CHECK_ERROR(SQL_ERROR, loginfo, NULL, SQL_HANDLE_STMT);
               }
           }

           retcode = SQLFreeHandle(SQL_HANDLE_STMT, hstmtinesrt);
           sprintf((char*)loginfo, "SQLFreeHandle hstmtinesrt");
           CHECK_ERROR(retcode, loginfo, hstmtinesrt, SQL_HANDLE_STMT);
       }


   exit:
       printf ("\nComplete.\n");

       // Connection
       if (hdbc != SQL_NULL_HDBC) {
           SQLDisconnect(hdbc);
           SQLFreeHandle(SQL_HANDLE_DBC, hdbc);
       }

       // Environment
       if (henv != SQL_NULL_HENV)
           SQLFreeHandle(SQL_HANDLE_ENV, henv);

       return 0;
   }
