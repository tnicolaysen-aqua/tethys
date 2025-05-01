*********
Data Prep
*********

**Last Updated:** July 2024

In this tutorial you will get you familiar with sample NextGen data and prepare it for use in your Tethys application.

0. Start From Previous Solution (Optional)
==========================================

If you wish to use the previous solution as a starting point:

.. parsed-literal::

    git clone https://github.com/tethysplatform/tethysapp-map_layout_tutorial.git
    cd tethysapp-map_layout_tutorial
    git checkout -b configure-map-layout-solution configure-map-layout-solution-|version|

1. Download the sample NextGen data
===================================

1. Download our sample NOAA-OWP NextGen water model data: `sample_nextgen_data.zip <https://drive.google.com/file/d/10Q960TiHNer-6cwjPYN_t4KsOX2917Hl/view?usp=share_link>`_
2. Save to :file:`$TETHYS_HOME/workspaces/map_layout_tutorial/app_workspace`
3. Unzip the contents to the same location
4. Delete the zip file

.. tip::
  
  The :file:`app_workspace` folder is the best place to store data that your Tethys app will need to reference.

2. Explore the Data
===================

1. Open the :file:`outputs` folder you just created.  You will see two types of file formats here: `.parquet` and `CSV` files.  The `CSV` files contain the time-series outputs of the NextGen model.  Files formatted :file:`<catID>.csv`` contain water catchment outputs.  Files formatted :file:`<nexID>_output.csv` contain nexus outputs.

2. Open the :file:`config` folder.  There are several file formats but the one relevant to this tutorial is :file: `.geojson`.  GeoJSON is a standardized spatial data format used to represent the locations and shapes of geographic features.  You will find GeoJSON files that correspond to different datasets: nexus points (`nexus.geojson`), flowpaths (`flowpaths.geojson`), and catchments (`catchments.geojson`).

3. Open the :file:`nexus.geojson` file.  The coordinate reference system (CRS) for the data is found at the end of the "name", ``...EPSG::5070`` (i.e. NAD83).  If we want to use it in our web-map we need to reproject it into either ``EPSG:4326`` (i.e. WGS84 geographic) or ``EPSG:3857`` (i.e. WGS84 projected).  In the next step well change the data to project in the ``EPSG:4326`` CRS.  


3. Reproject the Spatial Data
=============================

There are various packages and tools out there that can be used to reproject spatial datasets. The most straightforward option is to use ``geopandas``. To do this, we will create a small command-line python script that can reproject any provided GeoJSON file into the provided projection.

1. Create a subfolder in the :file:`~/tethysdev/tethysapp-map_layout_tutorial` folder called :file:`scripts`.

2. Enter the:file:`scripts` folder and create a file called :file:`reproject_environment.yml`.

This file will define the conda environment required to run the reproject script we will create.

3. Paste the following ``YAML`` code into the file:

.. code-block:: yaml

  name: reproject
  channels:
    - conda-forge
  dependencies:
    - python >=3.11
    - geopandas

4. Save and close this file

5. Create another file  in the :file:`scripts` folder called :file:`reproject.py`.

This file will contain the actual logic that performs the reprojection.

6. Paste the following ``python`` code into the file:

.. code-block:: python

  import argparse
  import geopandas as gp

  def main(args):
      gp.read_file(args.in_filename).to_crs(f'EPSG:{args.projection}').to_file(args.in_filename.replace('.geojson', f'_{args.projection}.geojson'))

  if __name__ == '__main__':
      parser = argparse.ArgumentParser(
          prog='reproject',
          description='Reproject GeoJSON files.'
      )

      parser.add_argument('in_filename', help='The source GeoJSON file to reproject.')
      parser.add_argument('projection', help='EPSG code of target projection.')

      args = parser.parse_args()
      main(args)

The ``argparse`` library is used to provide useful command-line management to your script, such as argument parsing, basic error handling and help messages. This logic actually takes up the bulk of the script, as can be seen.

The ``geopandas`` library is a powerful library for interacting with spatial data in various formats. Note that the main logic that performs the reprojection is contained on a single line. The GeoJSON file is read into memory, reprojected, and then written back out to a new GeoJSON file.

7. Save and close this file.

8. Open your computer's terminal and enter the following commands to create and activate the reproject environment.

.. code-block:: bash

  cd ~/tethysdev/tethysapp-map_layout_tutorial/scripts
  conda env create -f reproject_environment.yml
  conda activate reproject


9. Run the reprojection script on the catchment and nexus datasets

.. code-block:: bash

  python reproject.py $TETHYS_HOME/workspaces/map_layout_tutorial/app_workspace/sample_nextgen_data/config/nexus.geojson 4326
  python reproject.py $TETHYS_HOME/workspaces/map_layout_tutorial/app_workspace/sample_nextgen_data/config/catchments.geojson 4326

And that's it! These GeoJSON files have now been reprojected into ``EPSG:4326`` and are saved alongside the original versions with a ``_4326`` identifier. These are now ready for use in your Tethys web application!

4. Solution
===========

This concludes the Data Prep portion of the Map Layout Tutorial. You can view the solution on GitHub at `<https://github.com/tethysplatform/tethysapp-map_layout_tutorial/tree/data-prep-solution>`_ or clone it as follows:

.. parsed-literal::

    git clone https://github.com/tethysplatform/tethysapp-map_layout_tutorial.git
    cd tethysapp-map_layout_tutorial
    git checkout -b data-prep-solution data-prep-solution-|version|

You'll also need to do the following:

1. Download the solution version of the sample NextGen data used in this tutorial: `sample_nextgen_data_solution.zip <https://drive.google.com/file/d/1HA6fF_EdGtiE5ceKF0wH2H8GDElMA3zM/view?usp=share_link>`_.
2. Save to :file:`$TETHYS_HOME/workspaces/map_layout_tutorial/app_workspace`
3. Unzip the contents to the same location
4. Delete the zip file
5. Rename the :file:`sample_nextgen_data_solution` to :file:`sample_nextgen_data` (i.e. remove "_solution")
