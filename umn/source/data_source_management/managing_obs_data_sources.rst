:original_name: dws_01_1602.html

.. _dws_01_1602:

Managing OBS Data Sources
=========================

GaussDB(DWS) allows you to access data on OBS by using an agency. You can create a GaussDB(DWS) agency, grant the OBS OperateAccess or OBS Administrator permission to the agency, and bind the agency to an OBS data source you created. In this way, you can access data on OBS by using OBS foreign tables.

.. note::

   -  This feature is supported only in 8.2.0 or later.
   -  For the OBS data source of a cluster, only one of the creation, modification, and deletion operations can be performed at a time.

Creating an OBS Agency
----------------------

**Scenario**

Before creating an OBS data source, create an agency that grants GaussDB(DWS) the OBS OperateAccess or OBS Administrator permission.

**Procedure**

#. Click your account in the upper right corner of the page and choose **Identity and Access Management**.
#. In the navigation pane on the left, choose **Agency**. In the upper right corner, click **Create Agency**.
#. Select **Cloud Service** and set **Cloud Service** to **DWS**.
#. Click **Next** to grant the OBS OperateAccess or OBS Administrator permission to the agency.
#. Click **Next**. Select **All resources** or specific resources, confirm the information, and click **Submit**.

Creating an OBS Data Source
---------------------------

**Prerequisites**

An agency has been created to grant GaussDB(DWS) the OBS OperateAccess permission.

**Procedure**

#. On the GaussDB(DWS) console, choose **Clusters** > **Dedicated Clusters**.

#. In the cluster list, click the name of a cluster. On the page that is displayed, choose **Data Sources** > **OBS Data Source**.

#. Click **Create OBS Cluster Connection** and configure parameters.

   |image1|

   .. table:: **Table 1** OBS data source connection parameters

      +-----------------------------------+---------------------------------------------------------------------------------------------------------------+
      | Parameter                         | Description                                                                                                   |
      +===================================+===============================================================================================================+
      | Data Source Name                  | Name of the OBS data source connection to be created. You can assign a personalized value to this parameter.  |
      |                                   |                                                                                                               |
      |                                   | The data source name is used as the server name specified in the statement for creating an OBS foreign table. |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------+
      | OBS Agency                        | Agency with the OBS OperateAccess permission to be granted to GaussDB(DWS)                                    |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------+
      | Database                          | Database where the OBS data source connection is to be created                                                |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------+
      | Description                       | Description about the OBS data source connection                                                              |
      +-----------------------------------+---------------------------------------------------------------------------------------------------------------+

#. Confirm the settings and click **OK**. The creation takes about 10 seconds.

Updating the OBS Data Source Configuration
------------------------------------------

**Scenario**

After an OBS data source connection is created, GaussDB(DWS) periodically updates the temporary agency information used by the data source. If the automatic update fails for 24 hours, the data source connection will be unavailable. To solve this problem, manually update the information on the console.

**Procedure**

#. On the GaussDB(DWS) console, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of a cluster. On the page that is displayed, choose **Data Sources** > **OBS Data Source**.
#. In the **Operation** column of an OBS data source, click **Update Configuration**.
#. Confirm the settings and click **OK**. The update takes about 10 seconds.

Changing the OBS Data Source Agency
-----------------------------------

**Scenario**

You can change the agency bound to the OBS data source.

**Procedure**

#. On the GaussDB(DWS) console, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of a cluster. On the page that is displayed, choose **Data Sources** > **OBS Data Source**.
#. In the **Operation** column of a data source, click **Manage Agency**. In the dialog box that is displayed, select a new agency.
#. Confirm the settings and click **OK**. The change takes about 10 seconds.

Deleting an OBS Data Source
---------------------------

#. On the GaussDB(DWS) console, choose **Clusters** > **Dedicated Clusters**.
#. In the cluster list, click the name of a cluster. On the page that is displayed, choose **Data Sources** > **OBS Data Source**.
#. In the **Operation** column of an OBS data source, click **Delete**.
#. Confirm the settings and click **OK**. The deletion takes about 10 seconds.

Using an OBS Data Source
------------------------

GaussDB(DWS) uses foreign tables to access data on OBS. The **SERVER** parameters specified for accesses with and without an agency are different.

If you access OBS without an agency, the **SERVER** provided on the console contains parameters **access_key** and **secret_access_key**, which are the AK and SK of the OBS access protocol, respectively.

If you access OBS with an agency, the **SERVER** provided on the console contains the **access_key**, **secret_access_key**, and **security_token** parameters, which are the temporary AK, temporary SK, and the **SecurityToken** value of the temporary security credential in IAM, respectively.

After the OBS agency and OBS data source are created, you can obtain the **SERVER** information, for example, **obs_server**, on the console. The way users create and use foreign tables with an agency is the same as the way they do without an agency. For how to use the OBS data source, see "Data Migration" > "Importing Data from OBS" in *Data Warehouse Service (DWS) Developer Guide*.

The following example shows how common user **jim** reads data from OBS through a foreign table.

#. Repeat the preceding steps to create an OBS data source named **obs_server**.

#. Connect to the database as system administrator dbadmin, create a common user, and grant the common user the permission to use OBS servers and OBS foreign tables. Replace **{Password}** with the actual password and **obs_server** with the actual OBS data source name.

   ::

      CREATE USER jim PASSWORD '{Password}';
      ALTER USER jim USEFT;
      GRANT USAGE ON FOREIGN SERVER obs_server TO jim;

#. Connect to the database as common user **jim** and create an OBS foreign table **customer_address** that does not contain partition columns.

   In the following command, replace **obs_server** with the name of the created OBS data source. Replace **/user/obs/region_orc11_64stripe1/** with the actual OBS directory for storing data files. **user** indicates the OBS bucket name.

   ::

      CREATE FOREIGN TABLE customer_address
      (
          ca_address_sk             integer               not null,
          ca_address_id             char(16)              not null,
          ca_street_number          char(10)                      ,
          ca_street_name            varchar(60)                   ,
          ca_street_type            char(15)                      ,
          ca_suite_number           char(10)                      ,
          ca_city                   varchar(60)                   ,
          ca_county                 varchar(30)                   ,
          ca_state                  char(2)                       ,
          ca_zip                    char(10)                      ,
          ca_country                varchar(20)                   ,
          ca_gmt_offset             decimal(36,33)                  ,
          ca_location_type          char(20)
      )
      SERVER obs_server OPTIONS (
          FOLDERNAME '/user/obs/region_orc11_64stripe1/',
          FORMAT 'ORC',
          ENCODING 'utf8',
          TOTALROWS  '20'
      )
      DISTRIBUTE BY roundrobin;

#. Query data stored in OBS by using a foreign table.

   ::

      SELECT COUNT(*) FROM customer_address;
      count
      -------
      20
      (1row)

.. |image1| image:: /_static/images/en-us_image_0000001951848781.png
