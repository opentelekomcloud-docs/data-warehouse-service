:original_name: dws_03_2142.html

.. _dws_03_2142:

What Are the Differences Between Hot Data Storage and Cold Data Storage?
========================================================================

The biggest difference between hot data storage and cold data storage lies in the storage media.

-  Hot data is frequently queried or updated and has high requirements on access response time. It is stored on **DN data disks**.
-  Cold data is not updated and is occasionally queried, and does not have high requirements on access response time. It is stored in **OBS**.

Different storage media determine the cost, performance, and application scenarios of the two storage mode, as shown in :ref:`Table 1 <en-us_topic_0000001384775385__table550418217359>`.

.. _en-us_topic_0000001384775385__table550418217359:

.. table:: **Table 1** Differences between hot and cold data storage

   +--------------+----------------+------+----------------------+--------------------------------------------------------------------------------------------------------------------+
   | Storage      | Read and Write | Cost | Capacity             | Application Scenario                                                                                               |
   +==============+================+======+======================+====================================================================================================================+
   | Hot storage  | Fast           | High | Fixed and restricted | This mode is applicable to scenarios where the data volume is limited and needs to be frequently read and updated. |
   +--------------+----------------+------+----------------------+--------------------------------------------------------------------------------------------------------------------+
   | Cold storage | Slow           | Low  | Large and unlimited  | This mode is applicable to scenarios such as data archiving. It features low cost and large capacity.              |
   +--------------+----------------+------+----------------------+--------------------------------------------------------------------------------------------------------------------+
