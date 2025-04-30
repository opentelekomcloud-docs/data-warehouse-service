:original_name: dws_04_0243.html

.. _dws_04_0243:

Preparing Data on OBS
=====================

Scenarios
---------

Before you use the SQL on OBS feature to query OBS data:

#. You have stored the ORC data on OBS.

   For example, the ORC table has been created when you use the Hive or Spark component, and the ORC data has been stored on OBS.

   Assume that there are two ORC data files, named **product_info.0** and **product_info.1**, whose original data is stored in the **demo.db/product_info_orc/** directory of the **mybucket** OBS bucket. You can view their original data in :ref:`Original Data <en-us_topic_0000001811609589__en-us_topic_0000001188482188_en-us_topic_0000001145410931_en-us_topic_0102810712_en-us_topic_0093837401_section1888720568453>`.

#. .. _en-us_topic_0000001811609589__en-us_topic_0000001188482188_en-us_topic_0000001145410931_en-us_topic_0102810712_li12771154711:

   If your data files are already on OBS, perform steps in :ref:`Obtaining the OBS Path of Original Data and Setting Read Permission <en-us_topic_0000001811609589__en-us_topic_0000001188482188_en-us_topic_0000001145410931_en-us_topic_0102810712_section112221416173415>`.

   .. note::

      This section uses the ORC format as an example to describe how to import data. The methods for importing CarbonData, Parquet, and JSON data are similar.

.. _en-us_topic_0000001811609589__en-us_topic_0000001188482188_en-us_topic_0000001145410931_en-us_topic_0102810712_en-us_topic_0093837401_section1888720568453:

Original Data
-------------

Assume that you have stored the two ORC data files on OBS and their original data is as follows:

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

.. _en-us_topic_0000001811609589__en-us_topic_0000001188482188_en-us_topic_0000001145410931_en-us_topic_0102810712_section112221416173415:

Obtaining the OBS Path of Original Data and Setting Read Permission
-------------------------------------------------------------------

#. Log in to the OBS management console.

   Click **Service List** and choose **Object Storage Service** to open the OBS management console.

#. .. _en-us_topic_0000001811609589__en-us_topic_0000001188482188_en-us_topic_0000001145410931_en-us_topic_0102810712_li123314509351:

   Obtain the OBS path for storing source data files.

   After the source data files are uploaded to an OBS bucket, a globally unique access path is generated. You need to specify the OBS paths of source data files when creating a foreign table.

   For details about how to view the OBS path for storing the data source files, see "OBS Console Operation Guide > Managing Objects > Accessing an Object Using Its URL" in the *Object Storage Service User Guide*.

   For example, the OBS paths are as follows:

   ::

      https://obs.example.com/mybucket/demo.db/product_info_orc/product_info.0
      https://obs.example.com/mybucket/demo.db/product_info_orc/product_info.1

#. Grant the OBS bucket read permission for the user.

   The user who executes the SQL on OBS function needs to obtain the read permission on the OBS bucket where the source data file is located. You can configure the ACL for the OBS buckets to grant the read permission to a specific user.

   For details, see "Console Operation Guide > Permission Control > Configuring a Bucket ACL" in *Object Storage Service User Guide*.
