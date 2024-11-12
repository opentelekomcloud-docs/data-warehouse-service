:original_name: dws_03_2149.html

.. _dws_03_2149:

What Should I Do If the Scale-In Button of GaussDB(DWS) Is Unavailable?
=======================================================================

Symptom
-------

When a user performs a scale-in operation, the **Scale In** button is unavailable and the user cannot proceed to the next scale-in operation.

Possible Causes
---------------

The system verifies the cluster's eligibility for scaling in before each operation. The **Scale In** button is dimmed if the cluster does not qualify.

Solution
--------

Check the cluster configuration information and check whether the scale-in meets the following conditions:

-  The cluster consists of rings of four or five hosts each, with primary, standby, and secondary DNs deployed on them. A cluster ring is the smallest unit for scaling in, which requires at least two rings. The system removes nodes from the last ring to the first when scaling in.
-  The removed nodes cannot contain the GTM, CM Server, or CN component.
-  The cluster status is **Normal**, and no other task information is displayed.
-  The cluster tenant account cannot be in the read-only, frozen, or restricted state.
-  The cluster is not in logical cluster mode.
-  The cluster cannot have idle nodes.
