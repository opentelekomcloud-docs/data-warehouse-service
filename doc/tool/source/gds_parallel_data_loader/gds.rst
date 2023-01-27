:original_name: gds_cmd_reference.html

.. _gds_cmd_reference:

gds
===

Context
-------

**gds** is used to import and export data of GaussDB(DWS).

Syntax
------

.. code-block::

   gds [ OPTION ] -d DIRECTORY

The **-d** and -**H** parameters are mandatory and **option** is optional. **gds** provides the file data from DIRECTORY for GaussDB(DWS) to access.

Before starting GDS, you need to ensure that your GDS version is consistent with the database version. Otherwise, the database will display an error message and terminate the import and export operations. You can view the specific version through the **-V** parameter.

Parameter Description
---------------------

-  -d dir

   Set the directory of the data file to be imported. If the GDS process permission is sufficient, the directory specified by **-**\ d is automatically created.

-  -p ip:port

   Set the IP address and port to be listened to of the GDS.

   Value range of the IP address: The IP address must be valid.

   Default value: **127.0.0.1**

   Value range of the listening port is a positive integer ranging from 1024 to 65535.

   Default value of **port**: **8098**

-  -l log_file

   Set the log file. This feature adds the function of automatical log splitting. After the **-R** parameter is set, GDS generates a new file based on the set value to prevent a single log file from being too large.

   Generation rule: By default, GDS identifies only files with the **.log** extension name and generates new log files.

   For example, if **-l** is set to **gds.log** and **-R** is set to 20 MB, a **gds-2020-01-17_115425.log** file will be created when the size of **gds.log** reaches 20 MB.

   If the log file name specified by **-l** does not end with **.log**, for example, **gds.log.txt**, the name of the new log file is **gds.log-2020-01-19_122739.txt**.

   When GDS is started, it checks whether the log file specified by **-l** exists. If the log file exists, a new log file is generated based on the current date and time, and the original log file is not overwritten.

-  -H address_string

   Set the hosts that can be connected to the GDS. This parameter must be the CIDR format and it supports the Linux system only. If multiple network segments need to be configured, use commas (,) to separate them. For example, **-H 10.10.0.0/24, 10.10.5.0/24**.

-  -e dir

   Set the saving path of error logs generated when data is imported.

   Default value: **data file directory**

-  -E size

   Set the upper thread of error logs generated when data is imported.

   Value range: 0 < size < 1 TB. The value must be a positive integer plus the unit. The unit can be KB, MB, or GB.

-  -S size

   Set the upper limit of the exported file size.

   Value range: 1 MB < size < 100 TB. The value must be a positive integer plus the unit. The unit can be KB, MB, or GB. If KB is used, the value must be greater than 1024 KB.

-  -R size

   Set the maximum size of a single GDS log file specified by **-l**.

   Value range: 1 MB < size < 100 TB. The value must be a positive integer plus the unit. The unit can be KB, MB, or GB. If KB is used, the value must be greater than 1024 KB.

   Default value: 16 MB

-  -t worker_num

   Set the number of concurrent imported and exported working threads.

   Value range: The value is a positive integer ranging between 0 and 200 (included).

   Default value: **8**

   Recommended value: 2 x CPU cores in the common file import and export scenario; in the pipe file import and export scenario, set the value to **64**.

   .. note::

      If a large number of pipe files are imported and exported concurrently, the value of this parameter must be greater than or equal to the number of concurrent services.

-  -s status_file

   Set the status file. This parameter supports the Linux system only.

-  -D

   The GDS is running on the backend and this parameter supports the Linux system only.

-  -r

   Traverse files in the recursion directory and this parameter supports the Linux system only.

-  -h

   Show help information.

-  --enable-ssl

   Use the SSL authentication mode to communicate with clusters.

-  --ssl-dir Cert_file

   Before using the SSL authentication mode, specify the path for storing the authentication certificates.

-  --debug-level

   Set the debug log level of the GDS to control the output of GDS debug logs.

   **Value range:** **0**, **1**, and **2**

   -  **0**: Only the file list related to log import and export is printed. If the log volume is small, set the parameter to this value only when the system is at normal state.
   -  **1**: All the log information is printed, including the connection information, session switch information, and statistics on each node.
   -  **2**: Detailed interaction logs and their status are printed to generate a huge number of debug logs to help identify the fault causes. You are advised to set the parameter to this value only during troubleshooting.

   **Default value**: **0**

-  --pipe-timeout

   Specify the timeout period for GDS to wait for operating a pipe.

   .. note::

      -  This parameter is used to prevent the following situation: One end of the pipe file is not read or written for a long time due to human or program problems. As a result, the read or write operation on the other end of the pipe is hung.
      -  This parameter does not indicate the maximum duration of a data import or export task. It indicates the maximum timeout duration of each read, open, or write operation on the pipe. If the timeout duration exceeds the value of **--pipe-timeout**, an error is reported to the frontend.

   **Value range**: greater than 1s Use a positive integer with the time unit, seconds (s), minutes (m), or hours (h). Example: **3600s**, **60m**, or **1h**, indicating one hour.

   Default value: **1h**/**60m**/**3600s**

Examples
--------

Data file is saved in the **/data** directory, the IP address is 192.168.0.90, and the listening port number is 5000.

.. code-block::

   gds -d /data/ -p 192.168.0.90:5000 -H 10.10.0.1/24

Data file is saved in the subdirectory of the **/data** directory, the IP address is 192.168.0.90, and the listening port number is 5000.

.. code-block::

   gds -d /data/ -p 192.168.0.90:5000 -H 10.10.0.1/24 -r

Data file is saved in the **/data** directory, the IP address is 192.168.0.90, and the listening port number is 5000 which is running on the backend. The log file is saved in the **/log/gds_log.txt** file, and the specified number of the concurrently imported working threads is 32.

.. code-block::

   gds -d /data/ -p 192.168.0.90:5000 -H 10.10.0.1/24 -l /log/gds_log.txt -D  -t 32

Data file is saved in the **/data** directory, the IP address is 192.168.0.90, and the listening port number is 5000. Only the IP address of **10.10.0.\*** can be connected.

.. code-block::

   gds -d /data/ -p 192.168.0.90:5000 -H 10.10.0.1/24

Data files are stored in the **/data/** directory, the IP address of the directory is **192.168.0.90**, and the listening port number is **5000**. Only the node whose IP address is **10.10.0.\*** can be connected to. The node communicates with the cluster using the SSL authentication mode, and the certificate files are stored in the **/certfiles/** directory.

.. code-block::

   gds -d /data/ -p 192.168.0.90:5000 -H 10.10.0.1/24 --enable-ssl --ssl-dir /certfiles/

.. note::

   -  One GDS provides the import and export services for one cluster only at a time.
   -  For security purpose, specify the IP address and the listening port through **-p**.
   -  The certificate file includes the root certificate **cacert.pem**, level-2 certificate file **client.crt**, and private key file **client.key**.
   -  The password protection files **client.key.rand** and **client.key.cipher** are used when the system loading certificates.
