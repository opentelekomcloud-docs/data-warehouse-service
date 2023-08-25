:original_name: dws_07_0128.html

.. _dws_07_0128:

Stopping GDS
============

Scenarios
---------

Stop GDS after data is imported successfully.

Procedure
---------

#. Log in as user **gds_user** to the data server where GDS is installed.
#. Select the mode of stopping GDS based on the mode of starting it.

   -  If GDS is started using the **gds** command, perform the following operations to stop GDS:

      a. Query the GDS process ID:

         .. code-block::

            ps -ef|grep gds

         For example, the GDS process ID is 128954.

         .. code-block::

            ps -ef|grep gds
            gds_user 128954      1  0 15:03 ?        00:00:00 gds -d /input_data/ -p 192.168.0.90:5000 -l /log/gds_log.txt -D
            gds_user 129003 118723  0 15:04 pts/0    00:00:00 grep gds

      b. Run the **kill** command to stop GDS. **128954** in the command is the GDS process ID.

         .. code-block::

            kill -9 128954
