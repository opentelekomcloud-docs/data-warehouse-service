:original_name: dws_mt_0109.html

.. _dws_mt_0109:

Temporary Tables
================

GaussDB(DWS) does not support **GLOBAL TEMPORARY TABLE**, and it will be migrated to **LOCAL TEMPORARY TABLE**.

**ON COMMIT DELETE ROWS** is also not supported, and will be migrated to **ON COMMIT PRESERVE ROWS**.


.. figure:: /_static/images/en-us_image_0000001706224653.png
   :alt: **Figure 1** Input: TEMPORARY TABLE

   **Figure 1** Input: TEMPORARY TABLE


.. figure:: /_static/images/en-us_image_0000001658025302.png
   :alt: **Figure 2** Output: TEMPORARY TABLE

   **Figure 2** Output: TEMPORARY TABLE
