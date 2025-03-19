******************
Start and Register
******************

**Last Updated:** July 2024


1. Scaffold New App
===================

Open the terminal and enter the Tethys environment.  Create a new app by entering the following code into the terminal and enter the optional metadata:

.. code-block:: bash

    conda activate tethys
    tethys scaffold geoserver_app

2. Create Spatial Dataset Service Setting
=========================================

Open the ``app.py`` and add the ``SpatialDatasetServiceSetting`` dependency to the top of the file and method to the end of the ``App`` class:

.. code-block:: python
    :emphasize-lines: 1, 9-22

    from tethys_sdk.app_settings import SpatialDatasetServiceSetting

    class App(TethysAppBase):
        """
        Tethys app class for Geoserver App.
        """
        ...

        def spatial_dataset_service_settings(self):
            """
            Example spatial_dataset_service_settings method.
            """
            sds_settings = (
                SpatialDatasetServiceSetting(
                    name='main_geoserver',
                    description='spatial dataset service for app to use',
                    engine=SpatialDatasetServiceSetting.GEOSERVER,
                    required=True,
                ),
            )

            return sds_settings

3. Install GeoServer and Start Tethys Development Server
========================================================

Enter the following commands into the terminal to install and run the app.

.. code-block:: bash

    cd tethysapp-geoserver_app
    tethys install -d
    tethys manage start


4. Start GeoServer
==================

If you are using the Docker containers, create and start your :doc:`../../software_suite/geoserver` container:

.. code-block:: bash

	tethys docker init -c geoserver

.. tip::
    
    After executing this command the terminal will prompt you for input about several settings.  It is OK to accept the default values on all the settings.

.. code-block:: bash

	tethys docker start -c geoserver

Otherwise ensure that you have GeoServer installed and running. Refer to the `GeoServer Installation Guide <http://docs.geoserver.org/stable/en/user/installation/>`_ for system specific instructions.

5. Create GeoServer Spatial Dataset Service
===========================================

Register the GeoServer with Tethys Portal admin page by creating a Spatial Dataset Service:

1. Select the "Site Admin" link from the drop down menu by your username.
2. Scroll to the "Tethys Services" section and select the "Spatial Dataset Services" link.
3. Click on the "Add Spatial Dataset Service" button.
4. Create a new Spatial Dataset Service named "primary_geoserver" of engine type GeoServer.
5. Enter the endpoint and public endpoint as the same (e.g.: http://localhost:8181/geoserver/rest/ if using Docker or http://localhost:8080/geoserver/rest/ if using a default installation of GeoServer).
6. No API Key is required.
7. Fill out the username and password (default username and password is "admin" and "geoserver", respectively).
8. Press "Save".

.. Note:: 
    
    For security reasons, after the password is saved the field will always show four placeholder pips no matter the lenghth of the password itself.

.. important::

    In a production deployment of Tethys, the public endpoint should point to the publicly accessible host and port of the geoserver (e.g.: http://www.example.com:8181/geoserver/rest/)

6. Assign Spatial Dataset Service to App Setting
================================================

Assign the "primary_geoserver" Spatial Dataset Service to the "main_geoserver" setting for the app.

1. Select the "Site Admin" link from the drop down menu by your username.
2. Scroll to the "Tethys Apps" section and select the "Installed Apps" link.
3. Select the "Geoserver App" link.
4. Scroll down to the "Spatial Dataset Service Settings" section and assign the "primary_geoserver" to the Spatial Dataset Service property of the "main_geoserver" setting for the app.
5. Press "Save".

.. tip::

	If you don't see the "main_geoserver" setting in the "Spatial Dataset Service Settings" section try restarting the Tethys development server. If it still doesn't show up, then stop the Tethys development server, uninstall the app, reinstall it, and start the Tethys server again:

    .. code-block:: bash

        tethys uninstall geoserver_app
        cd tethysapp-geoserver_app
        tethys install -d
        tethys manage start


7. Download Test Files
======================

Download the sample shapefiles that you will use to test your app:

:download:`geoserver_app_data.zip`

The archive contains several shapefiles organized into folders. Unzip the archive to your preferred location and inspect the files.

8. GeoServer Web Admin Interface
================================

Explore the GeoServer web admin interface by visiting link: `<http://localhost:8181/geoserver/web/>`_.

9. Solution
===========

This concludes the this part of the GeoServer tutorial. You can view the solution on GitHub at `<https://github.com/tethysplatform/tethysapp-geoserver_app>`_ or clone it as follows:

.. parsed-literal::

    git clone https://github.com/tethysplatform/tethysapp-geoserver_app.git
    cd tethysapp-geoserver_app
    git checkout -b start-and-register-solution start-and-register-solution-|version|
