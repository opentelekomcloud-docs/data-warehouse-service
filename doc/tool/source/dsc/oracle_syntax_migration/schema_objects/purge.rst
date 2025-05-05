:original_name: dws_mt_0113.html

.. _dws_mt_0113:

PURGE
=====

In Oracle, the **DROP TABLE** statement moves a table to the recycle bin, allowing for potential recovery. In contrast, the **PURGE** statement permanently deletes a table or index from the recycle bin, releasing all associated space. It can also empty the entire recycle bin or permanently remove specific contents from a deleted tablespace.

After migration using **Oracle** syntax, the query does not include **PURGE**.

The following examples illustrate the syntax changes for **PURGE** before and after migration.

Pre-migration
-------------


.. figure:: /_static/images/en-us_image_0000001706105309.jpg
   :alt: **Figure 1** Input: statement containing PURGE

   **Figure 1** Input: statement containing PURGE

Post-migration
--------------


.. figure:: /_static/images/en-us_image_0000001657865874.jpg
   :alt: **Figure 2** Output: statement without PURGE

   **Figure 2** Output: statement without PURGE
