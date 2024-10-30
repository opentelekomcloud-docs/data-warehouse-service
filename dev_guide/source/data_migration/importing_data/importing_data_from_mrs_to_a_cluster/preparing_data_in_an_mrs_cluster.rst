:original_name: dws_04_0212.html

.. _dws_04_0212:

.. _en-us_topic_0000001717097320:

Preparing Data in an MRS Cluster
================================

Before importing data from MRS to a GaussDB(DWS) cluster, you must have:

#. Created an MRS cluster.
#. Created a Hive/Spark ORC table in the MRS cluster and stored the table data to the HDFS path corresponding to the table.

If you have completed the preparations, skip this section.

In this tutorial, the Hive ORC table will be created in the MRS cluster as an example to complete the preparation work. The process and the SQL syntax for creating a Spark ORC table in the MRS cluster are similar to those in Hive.

.. _en-us_topic_0000001717097320__en-us_topic_0000001188642198_en-us_topic_0000001082830951_en-us_topic_0109259515_en-us_topic_0101477888_section55166005141018:

Data File
---------

The sample data of the **product_info.txt** data file is as follows:

.. code-block::

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

Creating a Hive ORC Table in an MRS Cluster
-------------------------------------------

#. Create an MRS cluster.

   For details, see "Creating a Cluster > Creating a User-Defined Cluster" in the *MapReduce Service User Guide*.

#. Download the client.

   a. Go back to the MRS cluster page. Click the cluster name. On the **Dashboard** tab page of the cluster details page, click **Access Manager**. If a message is displayed indicating that EIP needs to be bound, bind an EIP first.

   b. Enter the username **admin** and its password for logging in to MRS Manager. The password is the one you entered when creating the MRS cluster.

   c. Choose **Cluster** > *Name of the desired cluster* > **Dashboard**. On the page that is displayed, choose **More** > **Download Client**. The **Download Cluster Client** dialog box is displayed.

      |image1|

      .. note::

         To obtain the client of an earlier version, choose **Services** > **Download Client** and set **Select Client Type** to **Configuration Files Only**.

#. .. _en-us_topic_0000001717097320__en-us_topic_0000001188642198_en-us_topic_0000001082830951_en-us_topic_0109259515_en-us_topic_0101477888_li14725131112614:

   Log in to the Hive client of the MRS cluster.

   a. Log in to a Master node.

      For details, see "Logging in to a cluster > Logging In to an ECS" in the MapReduce Service User Guide.

   b. Switch the user.

      .. code-block::

         sudo su - omm

   c. Run the following command to go to the client directory:

      .. code-block::

         cd /opt/client

   d. Run the following command to configure the environment variables:

      .. code-block::

         source bigdata_env

   e. If Kerberos authentication is enabled for the current cluster, run the following command to authenticate the current user. The current user must have the permission to create Hive tables.

      For details, see section "Creating a Role" in the *MapReduce Service User Guide*.

   f. Configure roles with the corresponding permissions.

      For details, see section "Creating a User" in the *MapReduce Service User Guide*.

   g. Bind roles to users. If the Kerberos authentication is disabled for the current cluster, skip this step.

      .. code-block::

         kinit MRS cluster user

      Example: **kinit hiveuser**

   h. Run the following command to start the Hive client:

      .. code-block::

         beeline

#. Create a database demo on Hive.

   Run the following command to create the database demo:

   .. code-block::

      CREATE DATABASE demo;

#. Create table **product_info** of the **Hive TEXTFILE** type in the database demo and import the :ref:`Data File <en-us_topic_0000001717097320__en-us_topic_0000001188642198_en-us_topic_0000001082830951_en-us_topic_0109259515_en-us_topic_0101477888_section55166005141018>` (**product_info.txt**) to the HDFS path corresponding to the table.

   Run the following command to switch to the database demo:

   .. code-block::

      USE demo;

   Run the following command to create table **product_info** and define the table fields based on data in the :ref:`Data File <en-us_topic_0000001717097320__en-us_topic_0000001188642198_en-us_topic_0000001082830951_en-us_topic_0109259515_en-us_topic_0101477888_section55166005141018>`.

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

   For details about how to import data to an MRS cluster, see "Cluster Operation Guide > Managing Active Clusters > Managing Data Files" in the *MapReduce Service User Guide*.

#. Create a Hive ORC table named **product_info_orc** in the database demo.

   Run the following command to create the Hive ORC table **product_info_orc**. The table fields are the same as those of the **product_info** table created in the previous step.

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

#. Insert data in the **product_info** table to the Hive ORC table **product_info_orc**.

   ::

      INSERT INTO product_info_orc SELECT * FROM product_info;

   Query table **product_info_orc**.

   ::

      SELECT * FROM product_info_orc;

   If data displayed in the :ref:`Data File <en-us_topic_0000001717097320__en-us_topic_0000001188642198_en-us_topic_0000001082830951_en-us_topic_0109259515_en-us_topic_0101477888_section55166005141018>` can be queried, the data has been successfully inserted to the ORC table.

.. |image1| image:: /_static/images/en-us_image_0000001636122557.png
