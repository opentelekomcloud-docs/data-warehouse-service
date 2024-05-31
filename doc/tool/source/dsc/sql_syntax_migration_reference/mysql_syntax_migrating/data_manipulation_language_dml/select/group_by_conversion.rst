:original_name: dws_16_0182.html

.. _dws_16_0182:

.. _en-us_topic_0000001772696228:

GROUP BY Conversion
===================

During MySQL/ADB group query, non-group columns can be queried. During GaussDB(DWS) group query, only group columns and aggregate functions can be queried, and if non-group columns are queried, an error will be reported. Therefore, GROUP BY in GaussDB(DWS) is changed to allow querying on non-group columns.

**Input**

.. code-block::

   SELECT e.department_id, department_name, ROUND(AVG(salary), 0) avg_salary FROM employees e JOIN departments d on e.department_id = d.department_id GROUP BY department_name ORDER BY department_name;

**Output**

.. code-block::

   SELECT
     e.department_id,
     department_name,
     ROUND (AVG(salary), 0) AS "avg_salary"
   FROM
     employees "e"
     JOIN departments "d" ON e.department_id = d.department_id
   GROUP BY
     department_name,
     1
   ORDER BY
     department_name;
