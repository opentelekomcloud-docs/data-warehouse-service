:original_name: dws_04_0486.html

.. _dws_04_0486:

Case: Rewriting SQL and Deleting Subqueries (Case 1)
====================================================

Symptom
-------

::

   select
       1,
       (select count(*) from customer_address_001 a4 where a4.ca_address_sk = a.ca_address_sk) as GZCS
   from customer_address_001 a;

This SQL performance is poor. SubPlan exists in the execution plan as follows:

|image1|

Optimization
------------

The core of this optimization is to eliminate subqueries. Based on the service scenario analysis, *a\ *\ **.**\ *\ ca_address_sk* is not null. In terms of SQL syntax, you can rewrite the SQL statement as follows:

::

   select
   count(*)
   from customer_address_001 a4, customer_address_001 a
   where a4.ca_address_sk = a.ca_address_sk
   group by  a.ca_address_sk;

.. note::

   To ensure that the modified statements have the same functions, *not null* is added to *customer_address_001. ca_address_sk*.

.. |image1| image:: /_static/images/en-us_image_0000001145695103.jpg
