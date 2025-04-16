:original_name: dws_mt_0109.html

.. _dws_mt_0109:

Temporary Tables
================

GaussDB(DWS) does not support **GLOBAL TEMPORARY TABLE**, It migrates **GLOBAL TEMPORARY TABLE** to **LOCAL TEMPORARY TABLE**.

**ON COMMIT DELETE ROWS** is also not supported and will be migrated to **ON COMMIT PRESERVE ROWS**.

The following is an example of the syntax of a temporary table before and after migration.

Pre-migration
-------------


.. figure:: /_static/images/en-us_image_0000002049908224.png
   :alt: **Figure 1** GLOBAL TEMPORARY TABLE and ON COMMIT DELETE ROWS

   **Figure 1** GLOBAL TEMPORARY TABLE and ON COMMIT DELETE ROWS

Post-migration
--------------


.. figure:: /_static/images/en-us_image_0000002085828829.png
   :alt: **Figure 2** LOCAL TEMPORARY TABLE and ON COMMIT PRESERVE ROWS

   **Figure 2** LOCAL TEMPORARY TABLE and ON COMMIT PRESERVE ROWS
