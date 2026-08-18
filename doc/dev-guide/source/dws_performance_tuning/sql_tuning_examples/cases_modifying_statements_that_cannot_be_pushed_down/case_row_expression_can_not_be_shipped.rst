:original_name: dws_04_0140.html

.. _dws_04_0140:

Case: "Row () Expression" Can Not Be Shipped
============================================

Possible Cause
--------------

The statement includes a row expression, typically shown as "(...)"in the targetlist( between SELECT and FROM).

**Solution**: Rewrite the SQL statement to eliminate the row expression.

Case 1: Using Row Expression (k4.businessID, arriveDate) in the targetlist
--------------------------------------------------------------------------

Original statement

::

   SELECT
       date_format(arriveDate, '%Y-%m-%d') AS "date",
       sales,
       COUNT(DISTINCT (k4.businessID, arriveDate)/*row expression*/)
   FROM (
       SELECT k1.businessID, k2.arriveDate, k1.sales
       FROM potential_customers k1
       LEFT JOIN flow_reception k2
           ON k1.businessID = k2.businessID
       WHERE k2.arriveDate BETWEEN '2024-08-31' AND '2024-09-02'
   ) k4
   GROUP BY arriveDate, sales;

|image1|

**Solution**: Do not output the row expression directly. Instead, rewrite the row expression as a character string.

::

   SELECT
       date_format(arriveDate, '%Y-%m-%d') AS "date",
       sales,
       COUNT(DISTINCT k4.businessID ||'-' ||  arriveDate/*Eliminate row expressions*/)
   FROM (
       SELECT k1.businessID, k2.arriveDate, k1.sales
       FROM potential_customers k1
       LEFT JOIN flow_reception k2
           ON k1.businessID = k2.businessID
       WHERE k2.arriveDate BETWEEN '2024-08-31' AND '2024-09-02'
   ) k4
   GROUP BY arriveDate, sales;

|image2|

Modification comparison

|image3|

.. |image1| image:: /_static/images/en-us_image_0000002526265757.png
.. |image2| image:: /_static/images/en-us_image_0000002526306787.png
.. |image3| image:: /_static/images/en-us_image_0000002526307817.png
