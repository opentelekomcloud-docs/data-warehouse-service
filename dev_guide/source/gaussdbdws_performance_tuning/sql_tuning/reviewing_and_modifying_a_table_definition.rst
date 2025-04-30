:original_name: dws_04_0437.html

.. _dws_04_0437:

.. _en-us_topic_0000002052813806:

Reviewing and Modifying a Table Definition
==========================================

In a distributed framework, data is distributed on DNs. Data on one or more DNs is stored on a physical storage device. To properly define a table, you must:

#. **Evenly distribute data on each DN** to avoid the available capacity decrease of a cluster caused by insufficient storage space of the storage device associated with a DN. Specifically, select a proper distribution key to avoid data skew.
#. **Evenly assign table scanning tasks on each DN** to avoid that a DN is overloaded by the table scanning tasks. Specifically, do not select columns in the equivalent filter of a base table as the distribution key.
#. **Reduce the data volume scanned** by using the partition pruning mechanism.
#. **Avoid the use of random I/O** by using clustering or partial clustering.
#. **Avoid data shuffle** to reduce the network pressure by selecting the **join-condition** column or **group by** column as the distribution column.

The distribution column is the core for defining a table. :ref:`Figure 1 <en-us_topic_0000002052813806__en-us_topic_0000001233681745_fig10935341194919>` shows the procedure of defining a table. The table definition is created during the database design and is reviewed and modified during SQL tuning.

.. _en-us_topic_0000002052813806__en-us_topic_0000001233681745_fig10935341194919:

.. figure:: /_static/images/en-us_image_0000002054191302.png
   :alt: **Figure 1** Defining a table

   **Figure 1** Defining a table
