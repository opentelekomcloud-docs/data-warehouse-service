:original_name: dws_04_0475.html

.. _dws_04_0475:

Case: Selecting an Appropriate Distribution Column
==================================================

Symptom
-------

Tables are defined as follows:

::

   CREATE TABLE t1 (a int, b int);
   CREATE TABLE t2 (a int, b int);

The following query is executed:

::

   SELECT * FROM t1, t2 WHERE t1.a = t2.b;

Optimization Analysis
---------------------

If **a** is the distribution column of **t1** and **t2**:

::

   CREATE TABLE t1 (a int, b int) DISTRIBUTE BY HASH (a);
   CREATE TABLE t2 (a int, b int) DISTRIBUTE BY HASH (a);

Then **Streaming** exists in the execution plan and the data volume is heavy among DNs, as shown in :ref:`Figure 1 <en-us_topic_0000001098654770__f5205d1152d7142eebcdc2eb0cee93030>`.

.. _en-us_topic_0000001098654770__f5205d1152d7142eebcdc2eb0cee93030:

.. figure:: /_static/images/en-us_image_0000001099135110.png
   :alt: **Figure 1** Selecting an appropriate distribution column (1)

   **Figure 1** Selecting an appropriate distribution column (1)

If **a** is the distribution column of **t1** and **b** is the distribution column of **t2**:

::

   CREATE TABLE t1 (a int, b int) DISTRIBUTE BY HASH (a);
   CREATE TABLE t2 (a int, b int) DISTRIBUTE BY HASH (b);

Then **Streaming** does not exist in the execution plan, and the data volume among DNs is decreasing and the query performance is increasing, as shown in :ref:`Figure 2 <en-us_topic_0000001098654770__ff0c933f80c044662b2ec539d8b3247b8>`.

.. _en-us_topic_0000001098654770__ff0c933f80c044662b2ec539d8b3247b8:

.. figure:: /_static/images/en-us_image_0000001098975124.png
   :alt: **Figure 2** Selecting an appropriate distribution column (2)

   **Figure 2** Selecting an appropriate distribution column (2)
