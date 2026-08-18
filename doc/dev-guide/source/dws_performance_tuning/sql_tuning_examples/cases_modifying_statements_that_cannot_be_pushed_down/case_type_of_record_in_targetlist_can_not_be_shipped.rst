:original_name: dws_04_0134.html

.. _dws_04_0134:

Case: Type of Record in TargetList Can Not Be Shipped
=====================================================

Possible Cause
--------------

The output column contains the record type, which is not displayed in the outermost output column. This type of error is reported in the following scenarios:

#. The SQL writing logic is incorrect. As a result, the output column contains (...).
#. The SQL service logic is correct. First, grasp what the service does. Then, change the record field to text format and adjust it as needed.

Case 1: Using the Output Column in the (…) Format
-------------------------------------------------

Original statement

::

   SELECT
       d.id,
       coalesce(d.period, 'snull') AS period,
       (d.plan_unit_code, 'snull') AS plan_unit_code,
       coalesce(d.product_type_model, 'snull') AS product_type_model,
       coalesce(d.revision, 'snull') AS revision,
       d.start_date
   FROM (SELECT *
       FROM cdcscm.cdc_mp_d_forecast_t_6120 t
       WHERE t.cdc_timestamp > to_date('2023-07-06 00:00:00', 'yyyy-mm-dd hh24:mi:ss') - 7
       AND t.cdc_timestamp < to_date('2023-08-08 00:00:00', 'yyyy-mm-dd hh24:mi:ss')
   ) t1,
   sdiscm.mp_d_forecast_t_6120 d
   WHERE (t1.audit_op_type = 'delete' AND t1.audit_op_option = 'before')
   AND d.id = t1.id

Rewritten statement

::

   SELECT
       d.id,
       coalesce(d.period, 'snull') AS period,
       coalesce(d.plan_unit_code, 'snull') AS plan_unit_code,
       coalesce(d.product_type_model, 'snull') AS product_type_model,
       coalesce(d.revision, 'snull') AS revision,
       d.start_date
   FROM (SELECT *
       FROM cdcscm.cdc_mp_d_forecast_t_6120 t
       WHERE t.cdc_timestamp > to_date('2023-07-06 00:00:00', 'yyyy-mm-dd hh24:mi:ss') - 7
       AND t.cdc_timestamp < to_date('2023-08-08 00:00:00', 'yyyy-mm-dd hh24:mi:ss')
   ) t1,
   sdiscm.mp_d_forecast_t_6120 d
   WHERE (t1.audit_op_type = 'delete' AND t1.audit_op_option = 'before')
   AND d.id = t1.id

.. note::

   The statements before and after rewriting are not equivalent because the original SQL statement is incorrect. The correct format is "coalesce(d.plan_unit_code, 'snull') AS plan_unit_code".

Modification comparison

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002493930042.png
