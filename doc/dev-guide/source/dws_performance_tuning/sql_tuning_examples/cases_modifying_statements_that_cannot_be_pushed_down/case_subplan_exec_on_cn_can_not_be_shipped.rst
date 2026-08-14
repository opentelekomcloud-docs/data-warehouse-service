:original_name: dws_04_0133.html

.. _dws_04_0133:

Case: Subplan Exec on CN Can Not Be Shipped
===========================================

Possible Cause
--------------

A sub-query run exclusively on a CN. Generally, any sub-query cannot be pushed down if it invokes system catalogs or views, or some functions cannot be pushed down.

Case 1: Using System Catalogs or Views Queried During a Subquery
----------------------------------------------------------------

Original statement

::

   WITH error_log  AS NOT MATERIALIZED (
       SELECT
           upper(log_column_name) AS log_column_name,
           log_error_code,
           s.char_length AS data_length,
           s.data_type,s.nullable
       FROM (SELECT
               distinct
               unnest(string_to_array(bad_log_column_name,',')) AS log_column_name,
               unnest(string_to_array(bad_log_error_code,',')) AS log_error_code
           FROM stgltc.BAD_cfs_inv_invoice_ad_2500
       ) T,
       (SELECT * FROM user_tab_columns WHERE table_name=lower('dlt_cfs_inv_invoice_ad_2500')) S
       WHERE  upper(T.log_column_name)=upper(S.column_name)
   )

   SELECT
       CASE WHEN upper('ACTIVITY_NAME') IN (SELECT log_column_name FROM error_log WHERE data_type IN ('varchar','char','character','nchar','character varying','varchar2','nvarchar2','clob','text') AND log_error_code='22001'/*The string is too long*/)
           THEN SUBSTRB(ACTIVITY_NAME,0,(SELECT distinct DATA_LENGTH FROM error_log WHERE upper(log_column_name)=upper('ACTIVITY_NAME')))
           ELSE ACTIVITY_NAME
       END AS ACTIVITY_NAME,
       CASE WHEN upper('ADJUSTMENT_ID') IN (SELECT log_column_name FROM error_log WHERE data_type IN ('varchar','char','character','nchar','character varying','varchar2','nvarchar2','clob','text') AND log_error_code='22001'/* The string is too long.*/
           THEN SUBSTRB(ADJUSTMENT_ID,0,(SELECT distinct DATA_LENGTH FROM error_log WHERE upper(log_column_name)=upper('ADJUSTMENT_ID')))
           ELSE ADJUSTMENT_ID
       END AS ADJUSTMENT_ID
   FROM stgltc.BAD_cfs_inv_invoice_ad_2500

Rewritten statement

::

   -- Identify the subquery that cannot be pushed down as the WITH error_log clause.
   -- SELECT * FROM user_tab_columns WHERE table_name=lower('dlt_cfs_inv_invoice_ad_2500')
   --
   -- This part is a system catalog query, so it can't be pushed down. The result is saved in an intermediate table.
   -- Create the intermediate table as a row-store table.
   CREATE TEMP TABLE s WITH(orientation=row) DISTRIBUTE BY ROUNDROBIN AS
   SELECT * FROM user_tab_columns WHERE table_name=lower('dlt_cfs_inv_invoice_ad_2500')

   -- All the tables involved in the query are column-store tables, and the temporary table s created earlier is a row-store table.
   -- Add a hint to use the vectorized executor.
   WITH error_log  AS NOT MATERIALIZED (
       SELECT
           upper(log_column_name) AS log_column_name,
           log_error_code,
           s.char_length AS data_length,
           s.data_type,s.nullable
       FROM (SELECT
               distinct
               unnest(string_to_array(bad_log_column_name,',')) AS log_column_name,
               unnest(string_to_array(bad_log_error_code,',')) AS log_error_code
           FROM stgltc.bad_cfs_inv_invoice_ad_2500
       ) T,
       pg_temp.S
       WHERE  upper(T.log_column_name)=upper(S.column_name)
   )

   SELECT /*+ set global(enable_force_vector_engine on)*/
       CASE WHEN upper('ACTIVITY_NAME') IN (SELECT log_column_name FROM error_log WHERE data_type IN ('varchar','char','character','nchar','character varying','varchar2','nvarchar2','clob','text') AND log_error_code='22001'/*The string is too long*/)
           THEN SUBSTRB(ACTIVITY_NAME,0,(SELECT distinct DATA_LENGTH FROM error_log WHERE upper(log_column_name)=upper('ACTIVITY_NAME')))
           ELSE ACTIVITY_NAME
       END AS ACTIVITY_NAME,
       CASE WHEN upper('ADJUSTMENT_ID') IN (SELECT log_column_name FROM error_log WHERE data_type IN ('varchar','char','character','nchar','character varying','varchar2','nvarchar2','clob','text') AND log_error_code='22001'/* The string is too long.*/
           THEN SUBSTRB(ADJUSTMENT_ID,0,(SELECT distinct DATA_LENGTH FROM error_log WHERE upper(log_column_name)=upper('ADJUSTMENT_ID')))
           ELSE ADJUSTMENT_ID
       END AS ADJUSTMENT_ID
   FROM stgltc.bad_cfs_inv_invoice_ad_2500

Modification comparison

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002528872995.png
