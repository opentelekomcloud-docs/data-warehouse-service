:original_name: dws_04_0184.html

.. _dws_04_0184:

Uploading Data to OBS
=====================

Scenarios
---------

Before importing data from OBS to a cluster, prepare source data files and upload these files to OBS. If the data files have been stored on OBS, you only need to complete :ref:`2 <en-us_topic_0000001233681749__en-us_topic_0000001082926859_en-us_topic_0117407656_l348f74692e3b4bfca287b6be74cfbed3>` to :ref:`3 <en-us_topic_0000001233681749__en-us_topic_0000001082926859_en-us_topic_0117407656_l04595af7f12b4893b44195f2f3bc227a>` in :ref:`Uploading Data to OBS <en-us_topic_0000001233681749__en-us_topic_0000001082926859_en-us_topic_0117407656_s17a8a2c2b90743c18e649d3c777065df>`.

Preparing Data Files
--------------------

Prepare source data files to be uploaded to OBS. GaussDB(DWS) supports only source data files in CSV, TXT, ORC, or CarbonData format.

If user data cannot be saved in CSV format, store the data as any text file.

.. note::

   According to :ref:`How Data Is Imported <en-us_topic_0000001233883255__en-us_topic_0000001082926905_en-us_topic_0117407705_sefc365e1804e4606aafdeb3398080e73>`, when the source data file contains a large volume of data, evenly split the file into multiple files before storing it to OBS. The import performance is better when the number of files is an integer multiple of the DN quantity.

Assume that you have stored the following three CSV files in OBS:

-  Data file **product_info.0**

   The file contains the following data:

   ::

      100,XHDK-A-1293-#fJ3,2017-09-01,A,2017 Autumn New Shirt Women,red,M,328,2017-09-04,715,good!
      205,KDKE-B-9947-#kL5,2017-09-01,A,2017 Autumn New Knitwear Women,pink,L,584,2017-09-05,406,very good!
      300,JODL-X-1937-#pV7,2017-09-01,A,2017 autumn new T-shirt men,red,XL,1245,2017-09-03,502,Bad.
      310,QQPX-R-3956-#aD8,2017-09-02,B,2017 autumn new jacket women,red,L,411,2017-09-05,436,It's really super nice.
      150,ABEF-C-1820-#mC6,2017-09-03,B,2017 Autumn New Jeans Women,blue,M,1223,2017-09-06,1200,The seller's packaging is exquisite.

-  Data file **product_info.1**

   The file contains the following data:

   ::

      200,BCQP-E-2365-#qE4,2017-09-04,B,2017 autumn new casual pants men,black,L,997,2017-09-10,301,The clothes are of good quality.
      250,EABE-D-1476-#oB1,2017-09-10,A,2017 autumn new dress women,black,S,841,2017-09-15,299,Follow the store for a long time.
      108,CDXK-F-1527-#pL2,2017-09-11,A,2017 autumn new dress women,red,M,85,2017-09-14,22,It's really amazing to buy.
      450,MMCE-H-4728-#nP9,2017-09-11,A,2017 autumn new jacket women,white,M,114,2017-09-14,22,Open the package and the clothes have no odor.
      260,OCDA-G-2817-#bD3,2017-09-12,B,2017 autumn new woolen coat women,red,L,2004,2017-09-15,826,Very favorite clothes.

-  Data file **product_info.2**

   The file contains the following data:

   ::

      980,"ZKDS-J",2017-09-13,"B","2017 Women's Cotton Clothing","red","M",112,,,
      98,"FKQB-I",2017-09-15,"B","2017 new shoes men","red","M",4345,2017-09-18,5473
      50,"DMQY-K",2017-09-21,"A","2017 pants men","red","37",28,2017-09-25,58,"good","good","good"
      80,"GKLW-l",2017-09-22,"A","2017 Jeans Men","red","39",58,2017-09-25,72,"Very comfortable."
      30,"HWEC-L",2017-09-23,"A","2017 shoes women","red","M",403,2017-09-26,607,"good!"
      40,"IQPD-M",2017-09-24,"B","2017 new pants Women","red","M",35,2017-09-27,52,"very good."
      50,"LPEC-N",2017-09-25,"B","2017 dress Women","red","M",29,2017-09-28,47,"not good at all."
      60,"NQAB-O",2017-09-26,"B","2017 jacket women","red","S",69,2017-09-29,70,"It's beautiful."
      70,"HWNB-P",2017-09-27,"B","2017 jacket women","red","L",30,2017-09-30,55,"I like it so much"
      80,"JKHU-Q",2017-09-29,"C","2017 T-shirt","red","M",90,2017-10-02,82,"very good."

.. _en-us_topic_0000001233681749__en-us_topic_0000001082926859_en-us_topic_0117407656_s17a8a2c2b90743c18e649d3c777065df:


Uploading Data to OBS
---------------------

#. Upload data to OBS.

   Store the source data files to be imported in the OBS bucket in advance.

   a. Log in to the OBS management console.

      Click **Service List** and choose **Object Storage Service** to open the OBS management console.

   b. Create a bucket.

      For details about how to create an OBS bucket, see "OBS Console Operation Guide > Managing Buckets > Creating a Bucket" in the *Object Storage Service User Guide*.

      For example, create two buckets named **mybucket** and **mybucket02**.

   c. Create a folder.

      For details about how to create an OBS bucket, see "OBS Console Operation Guide > Managing Objects > Creating a Folder" in the *Object Storage Service User Guide*.

      For example:

      -  Create a folder named **input_data** in the **mybucket** OBS bucket.
      -  Create a folder named **input_data** in the **mybucket02** OBS bucket.

   d. Upload the files.

      For details about how to create an OBS bucket, see "OBS Console Operation Guide > Managing Objects > Uploading a File" in the *Object Storage Service User Guide*.

      For example:

      -  Upload the following data files to the **input_data** folder in the **mybucket** OBS bucket:

         ::

            product_info.0
            product_info.1

      -  Upload the following data file to the **input_data** folder in the **mybucket02** OBS bucket:

         ::

            product_info.2

#. .. _en-us_topic_0000001233681749__en-us_topic_0000001082926859_en-us_topic_0117407656_l348f74692e3b4bfca287b6be74cfbed3:

   Obtain the OBS path for storing source data files.

   After the source data files are uploaded to an OBS bucket, a globally unique access path is generated. The OBS path of the source data files is the value of the **location** parameter used for creating a foreign table.

   The OBS path in the **location** parameter is in the format of **obs://**\ *bucket_name*/*file_path*/

   For example, the OBS paths are as follows:

   ::

      obs://mybucket/input_data/product_info.0
      obs://mybucket/input_data/product_info.1
      obs://mybucket02/input_data/product_info.2

#. .. _en-us_topic_0000001233681749__en-us_topic_0000001082926859_en-us_topic_0117407656_l04595af7f12b4893b44195f2f3bc227a:

   Grant the OBS bucket read permission for the user who will import data.

   When importing data from OBS to a cluster, the user must have the read permission for the OBS buckets where the source data files are located. You can configure the ACL for the OBS buckets to grant the read permission to a specific user.

   For details, see "Console Operation Guide > Permission Control > Configuring a Bucket ACL" in *Object Storage Service User Guide*.
