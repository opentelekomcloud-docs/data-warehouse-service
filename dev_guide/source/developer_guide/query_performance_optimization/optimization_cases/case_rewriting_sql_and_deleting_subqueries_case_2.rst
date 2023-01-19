:original_name: dws_04_0487.html

.. _dws_04_0487:

Case: Rewriting SQL and Deleting Subqueries (Case 2)
====================================================

Symptom
-------

On a site, the customer gave the feedback saying that the execution time of the following SQL statements lasted over one day and did not end:

::

   UPDATE calc_empfyc_c_cusr1 t1
   SET ln_rec_count =
    (
       SELECT CASE WHEN current_date - ln_process_date + 1 <= 12 THEN 0 ELSE t2.ln_rec_count END
       FROM calc_empfyc_c1_policysend_tmp t2
       WHERE t1.ln_branch = t2.ln_branch AND t1.ls_policyno_cusr1 = t2.ls_policyno_cusr1
   )
   WHERE dsign = '1'
   AND flag = '1'
   AND EXISTS
       (SELECT 1
       FROM calc_empfyc_c1_policysend_tmp t2
       WHERE t1.ln_branch = t2.ln_branch AND t1.ls_policyno_cusr1 = t2.ls_policyno_cusr1
       );

The corresponding execution plan is as follows:

|image1|

Optimization
------------

SubPlan exists in the execution plan, and the calculation accounts for a large proportion in the SubPlan query. That is, SubPlan is a performance bottleneck.

Based on the SQL syntax, you can rewrite the SQL statements and delete SubPlan as follows:

::

   UPDATE calc_empfyc_c_cusr1 t1
   SET ln_rec_count = CASE WHEN current_date - ln_process_date + 1 <= 12 THEN 0 ELSE t2.ln_rec_count END
   FROM calc_empfyc_c1_policysend_tmp t2
   WHERE
   t1.dsign = '1' AND t1.flag = '1'
   AND t1.ln_branch = t2.ln_branch AND t1.ls_policyno_cusr1 = t2.ls_policyno_cusr1;

The modified SQL statement task is complete within 50s.

.. |image1| image:: /_static/images/en-us_image_0000001098655278.png
