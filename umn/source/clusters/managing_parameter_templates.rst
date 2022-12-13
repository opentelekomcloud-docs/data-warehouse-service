:original_name: dws_01_0126.html

.. _dws_01_0126:

Managing Parameter Templates
============================

To facilitate database parameter configuration, GaussDB(DWS) provides the parameter template function. A parameter template contains some common database parameters. You can manage parameter templates on the GaussDB(DWS) management console. After applying a parameter template to a cluster, you can modify parameters on the parameter modification page of the cluster.

The following parts are included in this section:

-  :ref:`Overview <en-us_topic_0000001134400734__section11563104515277>`
-  :ref:`Parameters <en-us_topic_0000001134400734__section1250020487150>`
-  :ref:`Creating a Parameter Template <en-us_topic_0000001134400734__section197345189212>`
-  :ref:`Modifying a Parameter Template <en-us_topic_0000001134400734__section940613324129>`
-  :ref:`Applying a Parameter Template to the Cluster <en-us_topic_0000001134400734__section17765171763118>`
-  :ref:`Deleting a Parameter Template <en-us_topic_0000001134400734__section18540123972615>`

.. _en-us_topic_0000001134400734__section11563104515277:

Overview
--------

A parameter template is a set of parameters applicable to data warehouses. All parameters in the template have default values. The parameters include the session timeout interval, date, and time format. For details, see :ref:`Parameters <en-us_topic_0000001134400734__section1250020487150>`. You can adjust parameter values to better adapt the database to actual services. When creating a cluster, you can specify a parameter template for it. Parameters in the template will be applied to all databases in the cluster. If you do not specify a parameter template, the system applies the default parameter template to the cluster. After a cluster is created, you can modify the parameters on the **Parameter Modifications** page. Alternatively, select an existing parameter template or create a parameter template on the **Parameter Template Management** page and apply it to the cluster.

GaussDB(DWS) presets a default parameter template to data warehouses of each version. The default parameter template cannot be deleted and modified. If you want to modify parameter values in the template, create a customized parameter template. The parameters in the customized template can be modified. After a customized parameter template is applied to a cluster, it is not associated with the cluster. If you modify the parameter values in the template, the modifications will not be synchronized to the cluster. You need to apply the template to the cluster again, and then the modified parameter values can be applied to the cluster. Similarly, if you modify parameters on the cluster details page, the modifications will not be synchronized to the parameter template.

.. note::

   The default values of the following parameters are for reference only. For more information, see "Setting GUC Parameters".

.. _en-us_topic_0000001134400734__section1250020487150:

Parameters
----------

The parameter template only contains three parameters. These parameters will take effect on a cluster during cluster installation. You can check more parameters on the cluster parameter modification page of the console. For details, see :ref:`Modifying Database Parameters <dws_01_0152>`.

.. table:: **Table 1** Parameters

   +--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | Parameter                | Description                                                                                                                                         | Default Value         |
   +==========================+=====================================================================================================================================================+=======================+
   | timezone                 | Sets the time zone displayed in the time stamps.                                                                                                    | UTC                   |
   +--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | log_timezone             | Sets the time zone for timestamps in the server log.                                                                                                | UTC                   |
   +--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+
   | password_encryption_type | Specifies the encryption type of user passwords.                                                                                                    | 1                     |
   |                          |                                                                                                                                                     |                       |
   |                          | -  **0** indicates that passwords are encrypted with MD5.                                                                                           |                       |
   |                          | -  **1** indicates that passwords are encrypted with SHA-256, which is compatible with the MD5 user authentication method of the PostgreSQL client. |                       |
   |                          | -  **2** indicates that passwords are encrypted with SHA-256. MD5 is not recommended because it is not a secure encryption algorithm.               |                       |
   +--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+-----------------------+

.. _en-us_topic_0000001134400734__section197345189212:

Creating a Parameter Template
-----------------------------

If parameters in the default parameter template cannot meet service requirements, you can customize a parameter template and change the parameter values to better adapt to services.

To create a parameter template, perform the following steps:

#. Log in to the GaussDB(DWS) management console.

#. In the navigation tree on the left, click **Parameter Templates**.

#. Click **Create Parameter Template** and set the following parameters:

   -  **Database Engine**: Select a database engine.

   -  **Database Version**: Select a database version.

   -  **Name**: Enter the name of the new parameter template.

      Enter 4 to 64 characters. Only letters, digits, hyphens (-), underscores (_), and periods (.) are allowed. The value must start with a letter. Letters are case-insensitive.

   -  **Description**: Enter the description of the new parameter template. This parameter is optional.

      The parameter template description contains 0 to 256 characters and does not support special characters ``!<>'=&".``

   .. note::

      The **Database Engine** and **Database Version** selected during parameter template creation must be the same as the cluster type and version of the parameter template to be applied.


   .. figure:: /_static/images/en-us_image_0000001180440267.png
      :alt: **Figure 1** Creating a parameter template

      **Figure 1** Creating a parameter template

#. Click **OK**.

.. _en-us_topic_0000001134400734__section940613324129:

Modifying a Parameter Template
------------------------------

You can modify the parameter values in a customized parameter template but cannot modify the parameter values in the default parameter template.

#. Log in to the GaussDB(DWS) management console.

#. In the navigation tree on the left, click **Parameter Templates**.

#. In the **Name** column, click the name of the target parameter template. Its parameter table is displayed.

#. Enter a new value in the **Value** column of the parameter to be modified. After the modification, click **Save**.


   .. figure:: /_static/images/en-us_image_0000001134400896.png
      :alt: **Figure 2** Modifying parameters

      **Figure 2** Modifying parameters

#. In the **Modification Preview** dialog box, confirm the settings and modifications and click **Save**.

.. _en-us_topic_0000001134400734__section17765171763118:

Applying a Parameter Template to the Cluster
--------------------------------------------

After a cluster is created, you can apply a new parameter template to the cluster so that the values of all parameters in the parameter template can take effect in the cluster.

To apply a parameter template, perform the following steps:

#. Log in to the GaussDB(DWS) management console.

#. In the navigation tree on the left, click **Parameter Templates**.

#. Select the target parameter template and click **Apply** in the **Operation** column.

#. In the **Parameter Template Application** dialog box that is displayed, select the target cluster.

   You can apply the selected parameter template to the cluster corresponding to the parameter template version.


   .. figure:: /_static/images/en-us_image_0000001134560688.png
      :alt: **Figure 3** Parameter template application

      **Figure 3** Parameter template application

#. Click **OK**.

   If some parameter values in the new parameter template are different from the original parameter values in the cluster, a window comparing the differences will be displayed.

.. _en-us_topic_0000001134400734__section18540123972615:

Deleting a Parameter Template
-----------------------------

You can delete an unnecessary parameter template or a parameter template that is no longer used. The default parameter template cannot be deleted. Deleted parameter templates cannot be restored. Exercise caution when performing this operation.

#. Log in to the GaussDB(DWS) management console.
#. In the navigation tree on the left, click **Parameter Templates**.
#. In the **Operation** column of the parameter template to be deleted, click **Delete**.
#. In the displayed dialog box, click **Yes**.
