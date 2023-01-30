:original_name: dws_07_0129.html

.. _dws_07_0129:

gds_ctl.py
==========

Context
-------

**gds_ctl.py** can be used to start and stop **gds** if **gds.conf** has been configured.

**Prerequisites**
-----------------

Run the following commands on Linux OS: You need to ensure that the directory structure is as follows before the execution:

\|----gds

\|----gds_ctl.py

\|----config

\|-------gds.conf

\|-------gds.conf.sample

or

\|----gds

\|----gds_ctl.py

\|-------gds.conf

\|-------gds.conf.sample

Content of **gds.conf**:

.. code-block::

   <?xml version="1.0"?>
   <config>
   <gds name="gds1" ip="127.0.0.1" port="8098" data_dir="/data" err_dir="/err" data_seg="100MB" err_seg="1000MB" log_file="./gds.log" host="10.10.0.1/24" daemon='true' recursive="true" parallel="32"></gds>
   </config>

Configuration description of **gds.conf**:

-  name: tag name

-  ip: IP addresses to be listened to

-  port: Port number to be listened to

   Value range: an integer ranging from 1024 to 65535

   Default value: **8098**

-  **data_dir**: data file directory

-  **err_dir**: error log file directory

-  **log_file**: log file path

-  **host**: hosts that can be connected to the GDS.

-  **recursive**: whether the data file directory is recursive

   **Value range**:

   -  **true**: indicates the recursion data file directory.
   -  **false**: indicates the data file directory is not recursive.

-  **daemon**: specifies whether the service is running in DAEMON mode.

   **Value range**:

   -  **true** indicates the server is running in the DAEMON mode.
   -  **false** indicates the server is not running in the DAEMON mode.

-  **parallel**: indicates the number of concurrently imported and exported working threads.

   The default number of concurrencies is **8** and the maximum number is **200**.

Syntax
------

.. code-block::

   gds_ctl.py [ start | stop all | stop [ ip: ] port | stop | status ]

Description
-----------

**gds_ctl.py** can be used to start or stop GDS if **gds.conf** is configured.

Parameter Description
---------------------

-  start

   Enable the GDS configured in **gds.conf**.

-  stop

   Stop the running instance started by the configuration file in the GDS that can be disabled by the current users.

-  stop all

   Stop all the running instances in the GDS that can be disabled by the current users.

-  stop [ ip: ] port

   Stop the specific running GDS instance that can be closed by the current user. If **ip:port** is specified when the GDS is started, stop the corresponding **ip:port** to be specified. If the IP address is not specified when the GDS is started, you need to stop the specified port only. The stop fails if different information is specified when the GDS is started or stopped.

-  status

   Query the running status of the GDS instance started by the **gds.conf**.

Examples
--------

Start the GDS.

.. code-block::

   python3 gds_ctl.py start

Stop the GDS started by the configuration file.

.. code-block::

   python3 gds_ctl.py stop

Stop all the GDS instances that can be stopped by the current user.

.. code-block::

   python3 gds_ctl.py stop all

Stop the GDS instance specified by **[ip:]port** that can be stopped by the current user.

.. code-block::

   python3 gds_ctl.py stop 127.0.0.1:8098

Query the GDS status.

.. code-block::

   python3 gds_ctl.py status
