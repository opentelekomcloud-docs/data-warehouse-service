:original_name: dws_04_0265.html

.. _dws_04_0265:

.. _en-us_topic_0000001764650532:

Creating a GDS Foreign Table
============================

Procedure
---------

#. Set the **location** parameter for the foreign table based on the path planned in :ref:`Planning Data Export <en-us_topic_0000001811609749>`.

   -  **Remote** mode

      Set the **location** parameter to the URL of the directory that stores the data files.

      -  You do not need to specify a file name in the URL.
      -  When the number of data sources exported is fewer than the available paths, files are created for the surplus paths without writing any data.

      For example:

      The IP address of the GDS data server is 192.168.0.90. The listening port number set during GDS startup is 5000. The directory for storing data files is **/output_data**.

      In this case, set the **location** parameter to **gsfs://192.168.0.90:5000/**.

      .. note::

         -  By setting **location** to a subdirectory, for example, **gsfs://192.168.0.90:5000/2019/11/**, you can export the same table to different directories based on the date.
         -  In the current version, the system checks if the **/output_data/2019/11** directory exists when an export task is executed. If it does not exist, the system creates it. During the export, files are written to this directory. In this way, you do not need to manually run the **mkdir -p /output_data/2019/11** command after creating or modifying a foreign table.

#. Set data format parameters in the foreign table based on the planned data file formats.
#. Create a GDS foreign table based on the parameter settings in the preceding steps.

Example
-------

-  **Example**: Create the GDS foreign table **foreign_tpcds_reasons** for the source data. Data is to be exported as CSV files.

   Data export mode settings are as follows:

   The data server resides on the same intranet as the cluster. The IP address of the data server is 192.168.0.90. Data is to be exported as CSV files. The **Remote** mode is selected for parallel data export.

   Assume that the directory for storing data files is **/output_data/** and the GDS listening port is 5000 when GDS is started. Therefore, the **location** parameter is set to **gsfs://192.168.0.90:5000/**.

   Data format parameter settings are as follows:

   -  **format** is set to **CSV**.
   -  **encoding** is set to **UTF-8**.
   -  **delimiter** is set to **E'\\x08'**.
   -  **quote** is set to **E'\\x1b'**.
   -  **null** is set to an empty string without quotation marks.
   -  **escape** is set to the same value as that of **quote** by default.
   -  **header** is set to **false**, indicating that the first row is identified as a data row in an exported file.
   -  **EOL** is set to **0X0A**.

   The foreign table is created using the following statement:

   ::

      CREATE FOREIGN TABLE foreign_tpcds_reasons
      (
        r_reason_sk    integer        not null,
        r_reason_id    char(16)       not null,
        r_reason_desc  char(100)
      )
      SERVER gsmpp_server
      OPTIONS (LOCATION 'gsfs://192.168.0.90:5000/',
      FORMAT 'CSV',
      DELIMITER E'\x08',
      QUOTE E'\x1b',
      NULL '',
      EOL '0x0a'
      )
      WRITE ONLY;
