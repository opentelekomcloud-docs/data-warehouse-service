:original_name: dws_04_0489.html

.. _dws_04_0489:

Case: Rewriting SQL Statements and Deleting in-clause
=====================================================

Symptom
-------

in-clause/any-clause is a common SQL statement constraint. Sometimes, the clause following **in** or **any** is a constant. For example:

::

   select
   count(1)
   from calc_empfyc_c1_result_tmp_t1
   where ls_pid_cusr1 in ('20120405', '20130405');

or

::

   select
   count(1)
   from calc_empfyc_c1_result_tmp_t1
   where ls_pid_cusr1 in any('20120405', '20130405');

Some special usages are as follows:

::

   SELECT
   ls_pid_cusr1,COALESCE(max(round((current_date-bthdate)/365)),0)
   FROM calc_empfyc_c1_result_tmp_t1 t1,p10_md_tmp_t2 t2
   WHERE t1.ls_pid_cusr1 = any(values(id),(id15))
   GROUP BY ls_pid_cusr1;

Where **id** and **id15** are columns of p10_md_tmp_t2. ls_pid_cusr1 = any(values(id),(id15)) equals t1. ls_pid_cusr1 = id or t1. ls_pid_cusr1 = id15.

Therefore, join-condition is essentially an inequality, and nestloop must be used for this join operation. The execution plan is as follows:

|image1|

Optimization
------------

The test result shows that both result sets are too large. As a result, nestloop is time-consuming with more than one hour to return results. Therefore, the key to performance optimization is to eliminate nestloop, using more efficient hashjoin. From the perspective of semantic equivalence, the SQL statements can be written as follows:

::

   select
   ls_pid_cusr1,COALESCE(max(round(ym/365)),0)
   from
   (
            (
                      SELECT
                               ls_pid_cusr1,(current_date-bthdate) as ym
                      FROM calc_empfyc_c1_result_tmp_t1 t1,p10_md_tmp_t2 t2
                      WHERE t1.ls_pid_cusr1 = t2.id and t1.ls_pid_cusr1 != t2.id15
            )
            union all
            (
                      SELECT
                               ls_pid_cusr1,(current_date-bthdate) as ym
                      FROM calc_empfyc_c1_result_tmp_t1 t1,p10_md_tmp_t2 t2
                      WHERE t1.ls_pid_cusr1 = id15
            )
   )
   GROUP BY ls_pid_cusr1;

The optimized SQL queries consist of two equivalent join subqueries, and each subquery can be used for hashjoin in this scenario. The optimized execution plan is as follows:

|image2|

Before the optimization, no result is returned for more than 1 hour. After the optimization, the result is returned within 7s.

.. |image1| image:: /_static/images/en-us_image_0000001145495111.png
.. |image2| image:: /_static/images/en-us_image_0000001099135088.jpg
