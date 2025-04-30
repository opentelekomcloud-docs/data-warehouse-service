:original_name: dws_04_1400.html

.. _dws_04_1400:

Accessing Hive Metastore Across Clusters
========================================

To access MRS Hive data from storage-compute decoupled data warehouse, including when Hive is connected to HDFS or OBS, create an external schema. This function is supported only by 9.1.0 and later versions.

Preparing the Environment
-------------------------

-  Create a storage-compute decoupled and an MRS analysis cluster. Ensure that the MRS and GaussDB(DWS) clusters are in the same region, AZ, and VPC subnet and that the clusters can communicate with each other.

Constraints and Limitations
---------------------------

-  Currently, only the SELECT, INSERT, and INSERT OVERWRITE operations can be performed on tables in the Hive database through external schemas.

-  MRS supports two types of data sources. For details, see :ref:`Table 1 <en-us_topic_0000001838498170__en-us_topic_0000001473887582_table751712227268>`.

   .. _en-us_topic_0000001838498170__en-us_topic_0000001473887582_table751712227268:

   .. table:: **Table 1** Operations supported by MRS data sources

      +-------------+-----------------------+-------------------------+------+-----+---------+-----+
      | Data Source | Table Type            | Operation               | TEXT | CSV | PARQUET | ORC |
      +=============+=======================+=========================+======+=====+=========+=====+
      | HDFS        | Non-partitioned table | SELECT                  | Y    | Y   | Y       | Y   |
      +-------------+-----------------------+-------------------------+------+-----+---------+-----+
      |             |                       | INSERT/INSERT OVERWRITE | x    | x   | Y       | Y   |
      +-------------+-----------------------+-------------------------+------+-----+---------+-----+
      |             | Partitioned table     | SELECT                  | Y    | Y   | Y       | Y   |
      +-------------+-----------------------+-------------------------+------+-----+---------+-----+
      |             |                       | INSERT/INSERT OVERWRITE | x    | x   | Y       | Y   |
      +-------------+-----------------------+-------------------------+------+-----+---------+-----+
      | OBS         | Non-partitioned table | SELECT                  | Y    | Y   | Y       | Y   |
      +-------------+-----------------------+-------------------------+------+-----+---------+-----+
      |             |                       | INSERT/INSERT OVERWRITE | x    | x   | Y       | Y   |
      +-------------+-----------------------+-------------------------+------+-----+---------+-----+
      |             | Partitioned table     | SELECT                  | x    | x   | Y       | Y   |
      +-------------+-----------------------+-------------------------+------+-----+---------+-----+
      |             |                       | INSERT/INSERT OVERWRITE | x    | x   | Y       | Y   |
      +-------------+-----------------------+-------------------------+------+-----+---------+-----+

-  Transaction atomicity is no longer ensured. If a transaction fails, data consistency cannot be ensured. Rollback is not supported.
-  GRANT and REVOKE operations cannot be performed on tables created on Hive using external schemas.
-  Concurrency support: Concurrent read and write operations on GaussDB(DWS), Hive, and Spark may cause dirty reads. Concurrent operations including INSERT OVERWRITE on the same non-partitioned table or the same partition of the same partitioned table may not ensure the expected result. Therefore, do not perform such operations.
-  The Hive Metastore features do not support the federation mechanism.

Procedure
---------

#. Create a table in Hive.
#. Insert data on Hive, or upload a local TXT data file to an OBS bucket then import the file to Hive through the OBS bucket, and import the file from the TXT storage table to the ORC storage table.
#. Create an MRS data source connection.
#. Create a foreign server.
#. Create an external schema.
#. Use the external schema to import data to or read data from Hive tables.

Preparing the ORC Table Data Source of MRS
------------------------------------------

#. Create a **product_info.txt** file on the local PC, copy the following data to the file, and save the file to the local PC.

   ::

      100,XHDK-A-1293-#fJ3,2017-09-01,A,2017 Autumn New Shirt Women,red,M,328,2017-09-04,715,good
      205,KDKE-B-9947-#kL5,2017-09-01,A,2017 Autumn New Knitwear Women,pink,L,584,2017-09-05,406,very good!
      300,JODL-X-1937-#pV7,2017-09-01,A,2017 autumn new T-shirt men,red,XL,1245,2017-09-03,502,Bad.
      310,QQPX-R-3956-#aD8,2017-09-02,B,2017 autumn new jacket women,red,L,411,2017-09-05,436,It's really super nice
      150,ABEF-C-1820-#mC6,2017-09-03,B,2017 Autumn New Jeans Women,blue,M,1223,2017-09-06,1200,The seller's packaging is exquisite
      200,BCQP-E-2365-#qE4,2017-09-04,B,2017 autumn new casual pants men,black,L,997,2017-09-10,301,The clothes are of good quality.
      250,EABE-D-1476-#oB1,2017-09-10,A,2017 autumn new dress women,black,S,841,2017-09-15,299,Follow the store for a long time.
      108,CDXK-F-1527-#pL2,2017-09-11,A,2017 autumn new dress women,red,M,85,2017-09-14,22,It's really amazing to buy
      450,MMCE-H-4728-#nP9,2017-09-11,A,2017 autumn new jacket women,white,M,114,2017-09-14,22,Open the package and the clothes have no odor
      260,OCDA-G-2817-#bD3,2017-09-12,B,2017 autumn new woolen coat women,red,L,2004,2017-09-15,826,Very favorite clothes
      980,ZKDS-J-5490-#cW4,2017-09-13,B,2017 Autumn New Women's Cotton Clothing,red,M,112,2017-09-16,219,The clothes are small
      98,FKQB-I-2564-#dA5,2017-09-15,B,2017 autumn new shoes men,green,M,4345,2017-09-18,5473,The clothes are thick and it's better this winter.
      150,DMQY-K-6579-#eS6,2017-09-21,A,2017 autumn new underwear men,yellow,37,2840,2017-09-25,5831,This price is very cost effective
      200,GKLW-l-2897-#wQ7,2017-09-22,A,2017 Autumn New Jeans Men,blue,39,5879,2017-09-25,7200,The clothes are very comfortable to wear
      300,HWEC-L-2531-#xP8,2017-09-23,A,2017 autumn new shoes women,brown,M,403,2017-09-26,607,good
      100,IQPD-M-3214-#yQ1,2017-09-24,B,2017 Autumn New Wide Leg Pants Women,black,M,3045,2017-09-27,5021,very good.
      350,LPEC-N-4572-#zX2,2017-09-25,B,2017 Autumn New Underwear Women,red,M,239,2017-09-28,407,The seller's service is very good
      110,NQAB-O-3768-#sM3,2017-09-26,B,2017 autumn new underwear women,red,S,6089,2017-09-29,7021,The color is very good
      210,HWNB-P-7879-#tN4,2017-09-27,B,2017 autumn new underwear women,red,L,3201,2017-09-30,4059,I like it very much and the quality is good.
      230,JKHU-Q-8865-#uO5,2017-09-29,C,2017 Autumn New Clothes with Chiffon Shirt,black,M,2056,2017-10-02,3842,very good

#. Log in to the OBS console, click **Create Parallel File System**, configure the following parameters, and click **Create Now**.

   .. table:: **Table 2** Bucket parameters

      ====================== ==========================
      Parameter              Value
      ====================== ==========================
      Region                 Based on site requirements
      Data Redundancy Policy Single-AZ storage
      Bucket Name            mrs-datasource
      Default Storage Class  Standard
      Bucket Policy          Private
      Default Encryption     Disable
      Direct Reading         Disable
      Enterprise Project     default
      Tags                   N/A
      ====================== ==========================

#. Switch back to the MRS console and click the name of the created MRS cluster. On the **Dashboard** page, click the Synchronize button next to **IAM User Sync**. The synchronization takes about 5 minutes.

#. Click **Nodes** and click a master node. On the displayed page, switch to the **EIPs** tab, click **Bind EIP**, select an existing EIP, and click **OK**. If no EIP is available, create one. Record the EIP.

#. (Optional) Connect Hive to OBS.

   .. note::

      Perform this step when Hive interconnects with OBS. Skip this step when Hive interconnects with HDFS.

   a. Go back to the MRS cluster page. Click the cluster name. On the **Dashboard** tab page of the cluster details page, click **Access Manager**. If a message is displayed indicating that EIP needs to be bound, bind an EIP first.
   b. In the **Access MRS Manager** dialog box, click **OK**. You will be redirected to the MRS Manager login page. Enter the username **admin** and its password for logging in to MRS Manager. The password is the one you entered when creating the MRS cluster.
   c. Interconnect Hive with OBS by referring to .

#. Download the client.

   a. Go back to the MRS cluster page. Click the cluster name. On the **Dashboard** tab page of the cluster details page, click **Access Manager**. If a message is displayed indicating that EIP needs to be bound, bind an EIP first.

   b. In the **Access MRS Manager** dialog box, click **OK**. You will be redirected to the MRS Manager login page. Enter the username **admin** and its password for logging in to MRS Manager. The password is the one you entered when creating the MRS cluster.

   c. Choose **Services** > **Download Client**. Set **Client Type** to **Only configuration files** and set **Download To** to **Server**. Click **OK**.

      |image1|

#. Log in to the active master node as user **root** and update the client configuration of the active management node.

   **cd /opt/client**

   **sh refreshConfig.sh /opt/client** *Full_path_of_client_configuration_file_package*

   In this tutorial, run the following command:

   **sh refreshConfig.sh /opt/client** /tmp/MRS-client/MRS_Services_Client.tar

#. Switch to user **omm** and go to the directory where the Hive client is located.

   **su - omm**

   **cd /opt/client**

#. Create the **product_info** table whose storage format is TEXTFILE on Hive.

   a. Import environment variables to the **/opt/client** directory.

      **source bigdata_env**

      .. note::

         If **find: 'opt/client/Hudi': Permission denied** is displayed, ignore it. This does not affect subsequent operations.

   b. Log in to the Hive client.

      #. If Kerberos authentication is enabled for the current cluster, run the command below to authenticate the current user. The current user must have the permission to create Hive tables. For details, see "Creating a Role" in *MapReduce Service User Guide*.

      #. Configure roles with corresponding permissions. For details, see "Creating a Role" in *MapReduce Service User Guide*.. Bind a role to the user. If Kerberos authentication is not enabled for the current cluster, you do not need to run the following command:

         **kinit** *MRS cluster user*

      #. Run the following command to start the Hive client:

         **beeline**

   c. Run the following SQL commands in sequence to create a demo database and the **product_info** table:

      ::

         CREATE DATABASE demo;

      ::

         USE demo;

      ::

         DROP TABLE product_info;

         CREATE TABLE product_info
         (
             product_price                int            ,
             product_id                   char(30)       ,
             product_time                 date           ,
             product_level                char(10)       ,
             product_name                 varchar(200)   ,
             product_type1                varchar(20)    ,
             product_type2                char(10)       ,
             product_monthly_sales_cnt    int            ,
             product_comment_time         date           ,
             product_comment_num          int        ,
             product_comment_content      varchar(200)
         )
         row format delimited fields terminated by ','
         stored as TEXTFILE;

#. Import the **product_info.txt** file to Hive.

   -  Hive is interconnected with OBS: Go back to OBS Console, click the name of the bucket, choose **Objects** > **Upload Object**, and upload the **product_info.txt** file to the path of the **product_info table** in the OBS bucket.
   -  Hive is interconnected with HDFS: Import the **product_info.txt** file to the HDFS path **/user/hive/warehouse/demo.db/product_info/**. For operations related to importing data to the MRS cluster, see section "Cluster Operation Guide" > "Managing Active Clusters" > "Managing Data Files" in the *MapReduce Service User Guide*..

#. Create an ORC table and import data to the table.

   a. Run the following SQL commands to create an ORC table:

      ::

         DROP TABLE product_info_orc;

         CREATE TABLE product_info_orc
         (
             product_price                int            ,
             product_id                   char(30)       ,
             product_time                 date           ,
             product_level                char(10)       ,
             product_name                 varchar(200)   ,
             product_type1                varchar(20)    ,
             product_type2                char(10)       ,
             product_monthly_sales_cnt    int            ,
             product_comment_time         date           ,
             product_comment_num          int            ,
             product_comment_content      varchar(200)
         )
         row format delimited fields terminated by ','
         stored as orc;

   b. Insert data in the **product_info** table into the Hive ORC table **product_info_orc**.

      ::

         insert into product_info_orc select * from product_info;

   c. Query whether the data import is successful.

      ::

         select * from product_info_orc;

Creating an MRS Data Source Connection
--------------------------------------

#. Log in to the GaussDB(DWS) console and click the created GaussDB(DWS) cluster. Ensure that the GaussDB(DWS) and MRS clusters are in the same region, AZ, and VPC subnet.

#. Click the **MRS Data Source** tab and click **Create MRS Cluster Connection**.

#. Set the following parameters and click **OK**.

   -  **Data Source**: mrs_server
   -  **Configuration Mode**: **MRS Account**
   -  **MRS Data Source**: Select the created **mrs_01** cluster.
   -  **MRS Account**: admin
   -  **Password**: Enter the password of the **admin** user created for the MRS data source.

   |image2|

Creating a Foreign Server
-------------------------

Perform this step only when Hive interconnects with OBS. Skip this step when Hive interconnects with HDFS.

#. Use Data Studio to connect to the created GaussDB(DWS) cluster.

2. Run the following statement to create a foreign server.

   .. important::

      Hard-coded or plaintext AK and SK are risky. For security purposes, encrypt your AK and SK and store them in the configuration file or environment variables.

   ::

      CREATE SERVER obs_server FOREIGN DATA WRAPPER DFS_FDW
      OPTIONS
      (
      address 'obs.example.com:5443', //Address for accessing OBS
      encrypt 'on',
      access_key '{AK value}',
      secret_access_key '{SK value}',
       type 'obs'
      );

3. Check the foreign server.

   ::

      SELECT * FROM pg_foreign_server WHERE srvname='obs_server';

   The server is successfully created if information similar to the following is displayed:

   ::

                           srvname                      | srvowner | srvfdw | srvtype | srvversion | srvacl |                                                     srvoptions
      --------------------------------------------------+----------+--------+---------+------------+--------+---------------------------------------------------------------------------------------------------------------------
       obs_server |    16476 |  14337 |         |            |        | {address=obs.example.com:5443,type=obs,encrypt=on,access_key=***,secret_access_key=***}
      (1 row)

Create an external schema.
--------------------------

#. Obtain the internal IP address and port number of the Hive metastore service and the name of the Hive database to be accessed.

   a. Log in to the MRS console.
   b. Choose **Cluster** > **Active Cluster** and click the name of the cluster to be queried to enter the page displaying the cluster's basic information.
   c. Click **Go to manager** on the O&M Management page and enter the username and password to log in to the management page.
   d. Click **Cluster**, **Hive**, **Configuration**, **All Configurations**, **MetaStore**, and **Port** in sequence, and record the value of **hive.metastore.port**.
   e. Click **Cluster**, **Hive**, and **Instance** in sequence, and record the MetaStore management IP address of the host whose name contains **master1**.

#. Create an external schema.

   ::

      //When interconnecting Hive with OBS: Set SERVER to the name of the external server created in , DATABASE to the database created on Hive, METAADDRESS to the IP address and port number of the Hive metastore service recorded in , and CONFIGURATION to the default configuration path of the MRS data source.
      DROP SCHEMA IF EXISTS ex1;

      CREATE EXTERNAL SCHEMA ex1
          WITH SOURCE hive
               DATABASE 'demo'
               SERVER obs_server
               METAADDRESS '***.***.***.***:***'
               CONFIGURATION '/MRS/gaussdb/mrs_server'

      //When interconnecting Hive with HDFS: Set SERVER to mrs_server (name of the data source created in ), METAADDRESS to the IP address and port number of the Hive metastore service recorded in , and CONFIGURATION to the default configuration path of the MRS data source.
      DROP SCHEMA IF EXISTS ex1;

      CREATE EXTERNAL SCHEMA ex1
          WITH SOURCE hive
               DATABASE 'demo'
               SERVER mrs_server
               METAADDRESS '***.***.***.***:***'
               CONFIGURATION '/MRS/gaussdb/mrs_server'

#. View the created external schema.

   ::

      SELECT * FROM pg_namespace WHERE nspname='ex1';
      SELECT * FROM pg_external_namespace WHERE nspid = (SELECT oid FROM pg_namespace WHERE nspname = 'ex1');
                           nspid                     | srvname | source | address | database | confpath |                                                     ensoptions   | catalog
      --------------------------------------------------+----------+--------+---------+------------+--------+---------------------------------------------------------------------------------------------------------------------
                        16393                        |    obs_server |  hive | ***.***.***.***:***        |  demo          | ***       |                         |
      (1 row)

Importing Data
--------------

#. Create a local table for data import.

   ::

      DROP TABLE IF EXISTS product_info;
      CREATE TABLE product_info
      (
          product_price                integer        ,
          product_id                   char(30)       ,
          product_time                 date           ,
          product_level                char(10)       ,
          product_name                 varchar(200)   ,
          product_type1                varchar(20)    ,
          product_type2                char(10)       ,
          product_monthly_sales_cnt    integer        ,
          product_comment_time         date           ,
          product_comment_num          integer        ,
          product_comment_content      varchar(200)
      ) ;

#. Import the target table from the Hive table.

   ::

      INSERT INTO product_info SELECT * FROM ex1.product_info_orc;

#. Query the import result.

   ::

      SELECT * FROM product_info;

Exporting Data
--------------

#. Create a local source table.

   ::

      DROP TABLE IF EXISTS product_info_export;
      CREATE TABLE product_info_export
      (
          product_price                integer        ,
          product_id                   char(30)       ,
          product_time                 date           ,
          product_level                char(10)       ,
          product_name                 varchar(200)   ,
          product_type1                varchar(20)    ,
          product_type2                char(10)       ,
          product_monthly_sales_cnt    integer        ,
          product_comment_time         date           ,
          product_comment_num          integer        ,
          product_comment_content      varchar(200)
      ) ;
      INSERT INTO product_info_export SELECT * FROM product_info;

#. Create a target table on Hive.

   ::

      DROP TABLE product_info_orc_export;

      CREATE TABLE product_info_orc_export
      (
          product_price                int            ,
          product_id                   char(30)       ,
          product_time                 date           ,
          product_level                char(10)       ,
          product_name                 varchar(200)   ,
          product_type1                varchar(20)    ,
          product_type2                char(10)       ,
          product_monthly_sales_cnt    int            ,
          product_comment_time         date           ,
          product_comment_num          int            ,
          product_comment_content      varchar(200)
      )
      row format delimited fields terminated by ','
      stored as orc;

#. Import the local source table to the Hive table.

   ::

      INSERT INTO ex1.product_info_orc_export SELECT * FROM product_info_export;

#. Query the import result on Hive

   ::

      SELECT * FROM product_info_orc_export;

.. |image1| image:: /_static/images/en-us_image_0000001884500297.png
.. |image2| image:: /_static/images/en-us_image_0000001884619813.png
