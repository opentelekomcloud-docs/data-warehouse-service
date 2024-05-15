:original_name: dws_04_0263.html

.. _dws_04_0263:

Planning Data Export
====================

Scenarios
---------

Before you use GDS to export data from a cluster, prepare data to be exported and plan the export path.

Planning an Export Path
-----------------------

-  **Remote** mode

#. Log in to the GDS data server as user **root** and create the **/output_data** directory for storing data files.

   ::

      mkdir -p /output_data

#. (Optional) Create a user and the user group to which it belongs. This user is used to start GDS and must have the write permission on the directory for storing data files.

   ::

      groupadd gdsgrp
      useradd -g gdsgrp gdsuser

   If the following information is displayed, the user and user group already exist. Skip this step.

   ::

      useradd: Account 'gdsuser' already exists.
      groupadd: Group 'gdsgrp' already exists.

#. Change the directory owner to **gdsuser**.

   ::

      chown -R gdsuser:gdsgrp /output_data
