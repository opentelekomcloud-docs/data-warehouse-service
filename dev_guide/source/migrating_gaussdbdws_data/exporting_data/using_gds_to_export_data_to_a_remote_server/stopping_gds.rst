:original_name: dws_04_0267.html

.. _dws_04_0267:

.. _en-us_topic_0000001764650296:

Stopping GDS
============

GDS is a data service tool provided by GaussDB(DWS) that enables high-speed data export through coordination with external mechanisms.

If GDS is no longer used, perform the following steps to stop it:

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

   -  If GDS is started using the **gds_ctl.py** command, run the following commands to stop GDS:

      .. code-block::

         cd /opt/bin/dws/gds/bin
         python3 gds_ctl.py stop
