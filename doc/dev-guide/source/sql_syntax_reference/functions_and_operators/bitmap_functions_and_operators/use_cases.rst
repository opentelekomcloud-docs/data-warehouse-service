:original_name: dws_06_0325.html

.. _dws_06_0325:

Use Cases
=========

Background
----------

Currently, real-time precision marketing is required in the Internet, education, and gaming industries. User profiling enables user search based on combined criteria. Example:

-  Before launching a sales promotion, e-commerce companies need to send the promotion message to a selected batch of users with specific features.
-  In education, exercise questions need to be pushed based on students' weaknesses.
-  On search, video, and portal websites, different contents are pushed to users based on their interests.

These use cases have the following characteristics in common:

-  The data volume is huge and the calculation workload is huge.
-  There are a large number of users with a lot of labels and fields, occupying a large amount of storage space.
-  The feature conditions for selection are diversified, and it is difficult to find a fixed index. If each field has an index, that will occupy too much storage space.
-  High performance is required because real-time marketing requires response in seconds.
-  Data update has high requirements on timeliness, and user profiles need to be updated in real time.

Roaring bitmaps in GaussDB(DWS) can efficiently generate, compress, and parse bitmap data, and supports the most common bitmap aggregation operations (AND, OR, NOT, and XOR). This feature meets the requirements of real-time precision marketing and quick user selection in the case of a large amount of data with hundreds of millions of users and tens of millions of labels.

**Example of Using roaringbitmap**
----------------------------------

Assume that there is a web page browsing information table **userinfo**. The fields in the table are as follows:

::

   CREATE TABLE userinfo
   (userid int,
   age int,
   gender text,
   salary int,
   hobby  text
   )WITH (orientation=column);

The data in the **userinfo** table increases with the change of user information. For example, if a user has multiple hobby attributes, there will be multiple records in the **userinfo** table.

If a user wants to filter out males with income greater than CNY10,000, age greater than 30, and a hobby of phishing, and then push specific messages to these target groups.

The traditional method is to directly query the original table. The statement is as follows:

::

   SELECT distinct userid FROM userinfo WHERE salary > 10000 AND age > 30 AND gender ='m' AND hobby ='fishing';

If the **userinfo** table contains a small amount of data, indexes are created in the **salary**, **age**, **gender**, and **hobby** columns to meet the query requirements. However, if the **userinfo** table contains a large amount of data and a large number of labels, the preceding statement cannot meet the requirements. The reasons are as follows:

-  A large number of indexes need to be created.
-  The count (distinct) performance is poor.

**Roaring bitmaps perform better in this case.**

#. Create a RoaringBitmap table.

   ::

      CREATE TABLE userinfoset
      ( age int,
      gender text,
      salary int,
      hobby  text,
      userset roaringbitmap,
      PRIMARY KEY(age,gender,salary,hobby)
      )WITH (orientation=column);

#. All data in the **userinfo** table must be aggregated to the **userinfoset** table through the aggregation of the label column. You can run the following command to aggregate all data. Or you can aggregate only incremental data. To aggregate only incremental data, a set of users with the same label are put in a table record. This can be implemented by using the UPSERT function. Frequent update operations may generate a large amount of dirty data. Therefore, you are advised to create the **userinfoset** table as a row-store table to aggregate incremental data.

   ::

      INSERT INTO userinfoset
      SELECT age, gender, salary, hobby, rb_build_agg(userid)
      FROM
      userinfo
      GROUP BY age, gender, salary, hobby;

#. Query the **userinfoset** table for the selected user information.

   ::

      SELECT rb_iterate(rb_or_agg(userset)) FROM userinfoset WHERE salary > 10000 AND age > 30 AND gender ='m' AND hobby ='fishing';

After data aggregation, the data volume of the table **userinfoset** is much smaller than that of the source table, so the scanning performance of the base table is much faster. In addition, based on the advantages of Roaring bitmaps, the performance of calculating **rb_or_agg** and **rb_iterate** is better. Compared with the traditional method, the performance is significantly improved.
