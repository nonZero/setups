# Installing GeoDjango Stuff: PostGIS, GDAL, GEOS

## Ubuntu Linux 16.04
* Run:

        sudo apt-get install postgis postgresql-9.5-postgis-2.2

## Ubuntu Linux 17.10
* Run:

        sudo apt-get install postgis postgresql-9.6-postgis-2.3

## Windows
### PostGIS
* Install Postgis 2.4.2 **64 bit** from [here](https://winnie.postgis.net/download/windows/pg10/buildbot/postgis-bundle-pg10x64-setup-2.4.2-1.exe)
### GDAL + GEOS
* Run `python` to check if your **python** is 32 or 64 bit.
* Install corresponding [OSGeo4W](https://trac.osgeo.org/osgeo4w/) (32 or 64 bit) into `C:\OSGeo4W` or `C:\OSGeo4W64`:
    * **Note:** Select Express Web-GIS Install and click next.
    * In the ‘Select Packages’ list, ensure that GDAL is selected; MapServer and Apache are also enabled by default, may be unchecked safely.
* Make sure the following is included in your `settings.py`:

        import os
        if os.name == 'nt':
            import platform
            OSGEO4W = r"C:\OSGeo4W"
            if '64' in platform.architecture()[0]:
                OSGEO4W += "64"
            assert os.path.isdir(OSGEO4W), "Directory does not exist: " + OSGEO4W
            os.environ['OSGEO4W_ROOT'] = OSGEO4W
            os.environ['GDAL_DATA'] = OSGEO4W + r"\share\gdal"
            os.environ['PROJ_LIB'] = OSGEO4W + r"\share\proj"
            os.environ['PATH'] = OSGEO4W + r"\bin;" + os.environ['PATH']

* Run `python manage.py check` to verify geodjango is working correctly.

