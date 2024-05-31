:original_name: dws_16_0180.html

.. _dws_16_0180:

.. _en-us_topic_0000001819416269:

INTERVAL
========

In MySQL, the interval expression format is INTERVAL N, which is not supported by GaussDB(DWS) and needs to be converted to INTERVAL'N'.

**Input**

.. code-block::

   SELECT CURRENT_TIME() - INTERVAL 4 DAY;
   SELECT NOW() - INTERVAL 5 HOUR;
   SELECT CURRENT_TIME() - INTERVAL '4' DAY;
   SELECT NOW() - INTERVAL '5' HOUR;
   SELECT CURRENT_TIME() - INTERVAL "4" DAY;
   SELECT NOW() - INTERVAL "5" HOUR;

**Output**

.. code-block::

   SELECT  (CURRENT_TIME () - INTERVAL '4' DAY);
   SELECT  (NOW () - INTERVAL '5' HOUR);
   SELECT  (CURRENT_TIME () - INTERVAL '4' DAY);
   SELECT  (NOW () - INTERVAL '5' HOUR);
   SELECT  (CURRENT_TIME () - INTERVAL '4' DAY);
   SELECT  (NOW () - INTERVAL '5' HOUR);
