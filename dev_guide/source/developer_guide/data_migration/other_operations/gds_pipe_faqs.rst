:original_name: dws_04_0279.html

.. _dws_04_0279:

GDS Pipe FAQs
=============

Precautions
-----------

-  GDS supports concurrent import and export. The **gds -t** parameter is used to set the size of the thread pool and control the maximum number of concurrent working threads. But it does not accelerate a single SQL task. The default value of **gds -t** is **8**, and the upper limit is **200**. When using the pipe function to import and export data, ensure that the value of **-t** is greater than or equal to the number of concurrent services. In the dual-cluster interconnection scenario, the value of **-t** must be greater than or equal to twice the number of concurrent services.

-  Data in pipes is deleted once read. Therefore, ensure that no other program except GDS reads data in the pipe during import or export. Otherwise, data may be lost, task errors may occur, or the exported files may be disordered.

-  Concurrent import and export of foreign tables with the same location are not supported. That is, multiple threads of GDS cannot read or write pipe files at the same time.

-  A single import or export task of GDS identifies only one pipe. Therefore, do not carry wildcard characters ({}[]?) in the location address set for the GDS foreign table. Example:

   .. code-block::

      CREATE FOREIGN TABLE foreign_test_pipe_tr( like test_pipe ) SERVER gsmpp_server OPTIONS (LOCATION 'gsfs://192.168.0.0.1:7789/foreign_test_*', FORMAT 'text', DELIMITER ',',  NULL '', EOL '0x0a' ,file_type 'pipe',auto_create_pipe 'false');

-  When the **-r** recursion parameter is enabled for GDS, only one pipe can be identified. That is, GDS identifies only one pipe in the current data directory and does not recursively search for it. Therefore, the **-r** parameter does not take effect in the pipe import and export scenarios.
-  CN retry is not supported during the import and export through a pipe, because GDS cannot control the operations performed by peer users and programs on pipes.
-  During the import, if the peer program does not write data into the pipe for more than one hour, the import task times out and an error is reported.
-  During the export, if the peer program does not read data from the pipe for more than one hour by default, the export task times out and an error is reported.
-  Ensure that the GDS version and kernel version support the function of importing and exporting data through pipes.
-  If the **auto_create_pipe parameter** of the foreign table is set to **true**, a delay may occur when GDS automatically creates a pipe. Before any operation on a pipe, check whether the automatically created pipe exists and whether it is a pipe file.
-  Once an import or export task through a GDS pipe is complete, the pipe is automatically deleted. However, the pipe deletion is delayed, if you manually terminate an import or export task. In this situation, the pipe is deleted after the timeout interval expires.

Common Troubleshooting Methods:
-------------------------------

-  Issue 1: **"/***/postgres_public_foreign_test_pipe_tr.pipe" must be named pipe.**

   Locating method: The type of the GDS foreign table **file_type** is pipe, but the operated file is a common file. Check whether the **postgres_public_foreign_test_pipe_tr.pipe** file is a pipe file.

-  Issue 2: **could not open pipe "/***/postgres_public_foreign_test_pipe_tw.pipe" cause by Permission denied**.

   Locating method: GDS does not have the permission to open the pipe file.

-  Issue 3: **could not open source file /*****/postgres_public_foreign_test_pipe_tw.pipe because timeout 300s for WRITING**.

   Locating method: Opening the pipe times out when GDS is used to export data. This is because the pipe is not created within 300 seconds after **auto_create_pipe** is set to **false**, or the pipe is created but is not read by any program within 300 seconds.

-  Issue 4: **could not open source file /*****/postgres_public_foreign_test_pipe_tw.pipe because timeout 300s for READING**.

   Locating method: Opening the pipe times out when GDS is used to export data. This is because the pipe is not created within 300 seconds after **auto_create_pipe** is set to **false**, or the pipe is created but is not written by any program within 300 seconds.

-  Issue 5: **could not poll writing source pipe file "/****/postgres_public_foreign_test_pipe_tw.pipe" timeout 300s.**

   Locating method: If the GDS does not receive any write event on the pipe within 300 seconds during data export, the pipe is not read for more than 300 seconds.

-  Issue 6: **could not poll reading source pipe file "/****/postgres_public_foreign_test_pipe_tw.pipe" timeout 300s.**

   Locating method: If the GDS does not receive any read event on the pipe within 300 seconds during data import, the pipe is not written for more than 300 seconds.

-  Issue 7: **could not open pipe file "/***/postgres_public_foreign_test_pipe_tw.pipe" for "WRITING" with error No such device or address**.

   Locating method: It indicates that the **/***/postgres_public_foreign_test_pipe_tw.pipe** file is not read by any program. As a result, GDS cannot open the pipe file by writing.
