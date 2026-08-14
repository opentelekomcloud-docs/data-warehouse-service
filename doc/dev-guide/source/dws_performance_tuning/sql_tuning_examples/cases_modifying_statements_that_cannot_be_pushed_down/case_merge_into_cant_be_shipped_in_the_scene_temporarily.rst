:original_name: dws_04_0141.html

.. _dws_04_0141:

Case: MERGE INTO Can't Be Shipped in the Scene Temporarily
==========================================================

Possible Cause
--------------

The USING clause of the MERGE INTO statement contains non-list expressions.

**Solution**: Rewrite the SQL statement to remove non-list expressions.

Case 1: Using AND 1 = 2 in the WHERE condition of Subquery d
------------------------------------------------------------

Original statement

::

   MERGE INTO dwifin.dwi_bill_pay_interface_line l
   USING (SELECT to_number(ss_id || invoice_id) as ap_invoice_id,
                 to_number(ss_id || invoice_line_id) as credit_invoice_line_id,
                 line_number,
                 nvl(deleted_flag, 'N') AS src_sys_del_flag
            FROM sdifin.apwf_pay_interface_line_t_6600 t
           WHERE t.adjust_type = 'Y' AND 1 = 2) d
   ON (l.ap_invoice_id = d.ap_invoice_id AND l.credit_invoice_line_id = d.credit_invoice_line_id AND l.line_number = d.line_number AND l.src_sys_del_flag = d.src_sys_del_flag AND l.ss_id = 6600
   )
   WHEN MATCHED THEN UPDATE SET l.del_flag = 'Y', last_upd_cycle_id = 20250301000000;

**Solution**: Remove AND 1 = 2.

::

   MERGE INTO dwifin.dwi_bill_pay_interface_line l
   USING (SELECT to_number(ss_id || invoice_id) as ap_invoice_id,
                 to_number(ss_id || invoice_line_id) as credit_invoice_line_id,
                 line_number,
                 nvl(deleted_flag, 'N') AS src_sys_del_flag
            FROM sdifin.apwf_pay_interface_line_t_6600 t
           WHERE t.adjust_type = 'Y' ) d
   ON (l.ap_invoice_id = d.ap_invoice_id AND l.credit_invoice_line_id = d.credit_invoice_line_id AND l.line_number = d.line_number AND l.src_sys_del_flag = d.src_sys_del_flag AND l.ss_id = 6600
   )
   WHEN MATCHED THEN UPDATE SET l.del_flag = 'Y', last_upd_cycle_id = 20250301000000;

Modification comparison

|image1|

.. |image1| image:: /_static/images/en-us_image_0000002526310177.png
