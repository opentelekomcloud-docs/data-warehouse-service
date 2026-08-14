:original_name: dws_06_0155.html

.. _dws_06_0155:

CREATE BARRIER
==============

Function
--------

Creates a barrier for cluster nodes. The barrier can be used for data restoration.

Precautions
-----------

Before creating a barrier, ensure that **gtm_backup_barrier** and **enable_cbm_tracking** are set to **on** for CNs and DNs in the cluster.

Syntax
------

::

   CREATE BARRIER [ barrier_name  ] ;

Parameter Description
---------------------

**barrier_name**

(Optional) Indicates the name of a barrier.

A string compliant with the :ref:`identifier naming rules <en-us_topic_0000001811634529__section1475018612353>`.

Examples
--------

Create a barrier without specifying its name.

::

   CREATE BARRIER;

Create a barrier named **barrier1**.

::

   CREATE BARRIER 'barrier1';
