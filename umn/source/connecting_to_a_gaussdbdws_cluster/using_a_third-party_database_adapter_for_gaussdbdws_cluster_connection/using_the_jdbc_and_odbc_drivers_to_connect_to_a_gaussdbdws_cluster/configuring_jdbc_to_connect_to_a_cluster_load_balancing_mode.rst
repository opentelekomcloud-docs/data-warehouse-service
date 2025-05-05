:original_name: dws_01_0169.html

.. _dws_01_0169:

Configuring JDBC to Connect to a Cluster (Load Balancing Mode)
==============================================================

Context
-------

If you use JDBC to connect to only one CN in the cluster, this CN may be overloaded and other CN resources wasted. It also incurs single-node failure risks.

To avoid these problems, you can use JDBC to connect to multiple CNs. The following three methods are available:

-  Connection using ELB: An ELB distributes access traffic to multiple ECSs for traffic control based on forwarding policies. It improves the fault tolerance capability of application programs.
-  To connect to a cluster using JDBC load balancing, include at least one internal IP address of the CN in the URL. The system will scan all CN IP addresses automatically. JDBC load balancing functions like ELB, randomly connecting to a CN.
-  Connection in multi-host mode: Use JDBC to configure multiple nodes, which is similar to ELB.

Method 1: Using ELB to Connect to a Cluster
-------------------------------------------

#. Obtain the Elastic Load Balance address. On the console, go to the details page of a cluster and obtain the ELB IP address.

#. Configure the driver. For details, see :ref:`Downloading the JDBC or ODBC Driver <dws_01_0032>`.

#. Obtain the database connection.

   ::

      private static final String USER_NAME = "dbadmin";
      private static final String PASSWORD = "password";
      // jdbc:postgresql://ELB_IP:PORT/dbName"
      private static final String URL = "jdbc:postgresql://100.95.153.169:8000/gaussdb";
      private static Properties properties = new Properties();
      static {
          properties.setProperty("user", USER_NAME);
          properties.setProperty("password", PASSWORD);
      }
      /**
       * Obtain the database connection.
       */
      public static Connection getConnection() {
          Connection connection = null;
          try {
              connection = DriverManager.getConnection(URL, properties);
          } catch (SQLException e) {
              e.printStackTrace();
          }
          return connection;
      }

Method 2: Using JDBC Load Balancing to Connect to a Cluster (Recommended)
-------------------------------------------------------------------------

#. Obtain the private IP address. Open the specified cluster topology page on the console and obtain the private IP address of the CN. For details, see :ref:`Obtaining the Connection Address of a GaussDB(DWS) Cluster <dws_01_0033>`.

#. Configure the driver. For details, see :ref:`Downloading the JDBC or ODBC Driver <dws_01_0032>`.

   .. note::

      Starting from version 8.3.1.200, the JDBC load balancing mode now allows for the driver to connect to the cluster. If you intend to utilize this mode, ensure that your JDBC driver version is 8.3.1.200 or later.

#. Obtain the database connection. For how to set URL parameters, see :ref:`Using JDBC to Connect to a Cluster <dws_01_0077>`.

   ::

      private static final String USER_NAME = "dbadmin";
      private static final String PASSWORD = "password";
      // jdbc:postgresql://host1:port1,host2:port2/dbName"
      private static final String URL = "jdbc:postgresql://100.95.146.194:8000,100.95.148.220:8000,100.93.0.221:8000/gaussdb?loadBalanceHosts=true&cnListRefreshSwitch=on&cnListRefreshDelay=100000&cnListRefreshPeriod=5000
      ;
      private static Properties properties = new Properties();
      static {
          properties.setProperty("user", USER_NAME);
          properties.setProperty("password", PASSWORD);
      }
      /**
       * Obtain the database connection.
       */
      public static Connection getConnection() {
          Connection connection = null;
          try {
              connection = DriverManager.getConnection(URL, properties);
          } catch (SQLException e) {
              e.printStackTrace();
          }
          return connection;
      }

Method 3: Connecting to the Cluster in Multi-host Mode
------------------------------------------------------

#. Obtain the EIP. Go to the details page of a cluster on the console and obtain the EIP.

#. Configure the driver. For details, see :ref:`Downloading the JDBC or ODBC Driver <dws_01_0032>`.

#. Obtain the database connection.

   ::

      private static final String USER_NAME = "dbadmin";
      private static final String PASSWORD = "password";
      // jdbc:postgresql://host1:port1,host2:port2/dbName"
      private static final String URL = "jdbc:postgresql://100.95.146.194:8000,100.95.148.220:8000,100.93.0.221:8000/gaussdb?loadBalanceHosts=true";
      private static Properties properties = new Properties();
      static {
          properties.setProperty("user", USER_NAME);
          properties.setProperty("password", PASSWORD);
      }
      /**
       * Obtain the database connection.
       */
      public static Connection getConnection() {
          Connection connection = null;
          try {
              connection = DriverManager.getConnection(URL, properties);
          } catch (SQLException e) {
              e.printStackTrace();
          }
          return connection;
      }
