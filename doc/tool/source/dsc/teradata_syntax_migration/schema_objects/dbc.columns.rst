:original_name: dws_mt_0068.html

.. _dws_mt_0068:

DBC.COLUMNS
===========

**DBC.COLUMNS** view is a table containing information about table and view columns, stored procedures, or macro parameters. The view includes the following column names: **DatabaseName**, **TableName**, **ColumnName**, **ColumnFormat**, **ColumnTitle**, **ColumnType**, and **DefaultValue**. In GaussDB(DWS), this table is equivalent to the **information_schema.columns** table.

.. note::

   This feature requires one time execution of the custom script file *DSC/scripts/teradata/db_scripts/*\ **mig_fn_get_datatype_short_name.sql**.

   For more information about the steps to execute the file, refer :ref:`System Requirements <dws_mt_0014>` and :ref:`Prerequisites <dws_mt_0207>` sections respectively.

The DSC migrates the following dbc.columns to their corresponding information_schema columns.

.. table:: **Table 1** Migration of dbc.columns to information_schema columns

   ======================= ==========================================
   dbc.columns             information_schema.columns
   ======================= ==========================================
   ColumnName              Column_Name
   ColumnType              mig_fn_get_datatype_short_name (data_Type)
   ColumnLength            character_maximum_length
   DecimalTotalDigits      numeric_precision
   DecimalFractionalDigits numeric_scale
   databasename            table_schema
   tablename               table_name
   ColumnId                ordinal_position
   ======================= ==========================================

The following assumptions are made when migrating dbc.columns:

-  The FROM clause will contain only the dbc.columns TABLE NAME
-  COLUMN NAME can be in the form of *column_name* or *schema_name.table_name.column_name*.

Migration of dbc.columns is not supported for the following cases:

-  If the FROM clause has an ALIAS for dbc.columns table name (dbc.columns alias).
-  If dbc.columns is combined with other tables (FROM dbc.columns alias1, table1 alias2 OR dbc.columns alias1 join table1 alias2).

.. note::

   -  If the input SELECT statement includes dbc.column COLUMN NAMES directly, then the tool will migrate the input column names as an ALIAS. For example, the input column name **DecimalFractionalDigits** is migrated to *numeric_scale* with an ALIAS **DecimalFractionalDigits**.

      Example:

      Input:

      ::

         SEL
                columnid
                ,DecimalFractionalDigits
              FROM
                dbc.columns
         ;

      Output:

      ::

         SELECT
                ordinal_position columnid
                ,numeric_scale DecimalFractionalDigits
              FROM
                information_schema.columns
         ;

   -  For table names and schema names, the DSC will convert all string values to lowercase. To maintain case-sensitivity, the table/schema names should be within double quotes. In the following input example, "Test" will not be converted to lower case.

      ::

         SELECT
                   TableName
              FROM
                   dbc . columns
              WHERE
                   dbc.columns.databasename = '"Test"';

**Input:** **dbc.columns table with all supported columns**

::

   SELECT
   '$AUTO_DB_IP'
   ,objectdatabasename
   ,objecttablename
   ,'$TX_DATE_10'
   ,''
   ,'0'
   ,FirstStepTime
   ,FirstRespTime
   ,RowCount
   ,cast(RowCount*sum(case when T2.ColumnType ='CV' then T2.ColumnLength/3 else  T2.ColumnLength end) as decimal(38,0))
   ,'3'
   ,''
   ,'BAK_CLR_DATA'
   ,'2'
   ,''
   FROM TMP_clr_information T1
   inner join dbc.columns T2
   on T1.objectdatabasename =T2.DatabaseName
   and T1.objecttablename =T2.TableName
   where  T2.DatabaseName not in (
   sel child from dbc.children
   where parent='$FCRM_DB'
   )
   group by 1,2,3,4,5,6,7,8,9,11,12,13,14,15;

**Output**

::

   SELECT
             '$AUTO_DB_IP'
             ,objectdatabasename
             ,objecttablename
             ,'$TX_DATE_10'
             ,''
             ,'0'
             ,FirstStepTime
             ,FirstRespTime
             ,RowCount
             ,CAST( RowCount * SUM ( CASE WHEN mig_fn_get_datatype_short_name ( T2.data_Type ) = 'CV' THEN T2.character_maximum_length / 3 ELSE T2.character_maximum_length END ) AS DECIMAL( 38 ,0 ) )
             ,'3'
             ,''
             ,'BAK_CLR_DATA'
             ,'2'
             ,''
        FROM
             TMP_clr_information T1 INNER JOIN information_schema.columns T2
                  ON T1.objectdatabasename = T2.table_schema
             AND T1.objecttablename = T2.table_name
        WHERE
             NOT EXISTS (
                  SELECT
                            child
                       FROM
                            dbc.children
                       WHERE
                            child = T2.table_schema
                            AND( parent = '$FCRM_DB' )
             )
        GROUP BY
             1 ,2 ,3 ,4 ,5 ,6 ,7 ,8 ,9 ,11 ,12 ,13 ,14 ,15
   ;

**Input:** **dbc.columns table with TABLE NAME**

::

   SELECT
             TRIM( ColumnName )
             ,UPPER( dbc.columns.ColumnType )
        FROM
             dbc . columns
        WHERE
             dbc.columns.databasename = '"Test"'
        ORDER BY
             dbc.columns.ColumnId
   ;

**Output**

::

   SELECT
             TRIM( Column_Name )
             ,UPPER( mig_fn_get_datatype_short_name ( information_schema.columns.data_Type ) )
        FROM
             information_schema.columns
        WHERE
             information_schema.columns.table_schema = CASE
                  WHEN TRIM( '"Test"' ) LIKE '"%'
                  THEN REPLACE( SUBSTR( '"Test"' ,2 ,LENGTH( '"Test"' ) - 2 ) ,'""' ,'"' )
                  ELSE LOWER( '"Test"' )
             END
        ORDER BY
             information_schema.columns.ordinal_position
   ;
