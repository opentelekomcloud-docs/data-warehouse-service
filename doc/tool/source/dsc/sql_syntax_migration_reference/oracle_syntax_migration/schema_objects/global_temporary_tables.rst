:original_name: dws_mt_0212.html

.. _dws_mt_0212:

Global Temporary Tables
=======================

Global temporary tables are converted to local temporary tables.

**Input - GLOBAL TEMPORARY TABLE**

.. code-block::

   CREATE GLOBAL TEMPORARY TABLE
   "Pack1"."GLOBAL_TEMP_TABLE"

   ( "ID" VARCHAR2(8)

   ) ON COMMIT DELETE ROWS ;

**Output**

.. code-block::

   CREATE
        LOCAL TEMPORARY TABLE

   "Pack1_GLOBAL_TEMP_TABLE" (

   "ID" VARCHAR2 (8)
             )
              ON COMMIT PRESERVE ROWS ;
