:original_name: dws_mt_0015.html

.. _dws_mt_0015:

Supported Keywords and Features
===============================

This section provides the system context of DSC. DSC is a CLI tool that helps data migration engineers migrate scripts of source databases, such as Teradata, Oracle, Netezza, MySQL, and DB2, to GaussDB(DWS) databases.

DSC reads the scripts from the input folder. After the migration is complete, the migrated scripts are saved in the output folder. DSC also logs operations and errors to the log folder.

The rules of migrating keywords in SQL scripts are essential to the syntax migration using DSC. This section describes the Teradata, Oracle, Netezza, and MySQL keywords and features supported by DSC, and the parameters for keyword migration rules. Configure these parameters based on source databases, target databases, and migration scenarios.

Keywords that cannot be migrated are recorded in error logs after migration. Error logs also contain details about the files to which the error keywords belong, such as file names and locations. For details, see :ref:`Log Reference <dws_mt_0187>` and :ref:`Troubleshooting <dws_mt_0191>`.


.. figure:: /_static/images/en-us_image_0000001188681162.png
   :alt: **Figure 1** System context of DSC

   **Figure 1** System context of DSC
