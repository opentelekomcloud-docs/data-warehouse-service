:original_name: dws_04_0304.html

.. _dws_04_0304:

Using PostGIS
=============

.. note::

   -  The third-party software that the PostGIS Extension depends on needs to be installed separately. If you need to use PostGIS, submit a service ticket or contact technical support to submit an application.
   -  If the error message "ERROR: EXTENSION is not yet supported." is displayed, the PostGIS software package is not installed. Contact technical support.

Creating PostGIS Extension
--------------------------

Run the **CREATE EXTENSION** command to create PostGIS Extension.

::

   CREATE EXTENSION postgis;

Using PostGIS Extension
-----------------------

Use the following function to invoke a PostGIS Extension:

::

   SELECT GisFunction (Param1, Param2,......);

**GisFunction** is the function, and **Param1** and **Param2** are function parameters. The following SQL statements are a simple illustration for PostGIS use. For details about related functions, see `PostGIS 2.4.2 Manual <https://download.osgeo.org/postgis/docs/postgis-2.4.2.pdf>`__.

Example 1: Create a geometry table.

::

   CREATE TABLE cities ( id integer, city_name varchar(50) );
   SELECT AddGeometryColumn('cities', 'position', 4326, 'POINT', 2);

Example 2: Insert geometry data.

::

   INSERT INTO cities (id, position, city_name) VALUES (1,ST_GeomFromText('POINT(-9.5 23)',4326),'CityA');
   INSERT INTO cities (id, position, city_name) VALUES (2,ST_GeomFromText('POINT(-10.6 40.3)',4326),'CityB');
   INSERT INTO cities (id, position, city_name) VALUES (3,ST_GeomFromText('POINT(20.8 30.3)',4326), 'CityC');

Example 3: Calculate the distance between any two cities among three cities.

::

   SELECT p1.city_name,p2.city_name,ST_Distance(p1.position,p2.position) FROM cities AS p1, cities AS p2 WHERE p1.id > p2.id;

Deleting PostGIS Extension
--------------------------

Run the following command to delete PostGIS Extension from GaussDB(DWS):

::

   DROP EXTENSION postgis [CASCADE];

.. note::

   If PostGIS Extension is the dependee of other objects (for example, geometry tables), you need to add the **CASCADE** keyword to delete all these objects.
