:original_name: dws_04_0114.html

.. _dws_04_0114:

VIEW Object Design
==================

.. _en-us_topic_0000002100586266__en-us_topic_0000002135806933_section0606333194410:

Suggestion 2.16: Limiting View Nesting to Three Layers
------------------------------------------------------

.. note::

   **Impact of rule violation:**

   -  Too many nested views can lead to unstable execution plans and unpredictable time consumption.
   -  The risk of rebuilding objects on which views depend is high and the probability of lock conflicts increases.

   **Solution:**

   -  Create views based on physical tables.
