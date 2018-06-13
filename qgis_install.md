From website:
http://usabilityetc.com/2016/06/how-to-install-qgis-with-homebrew/

brew install gdal
brew install python
pip install matplotlib
pip install psycopg2
brew tap osgeo/osgeo4mac
brew install qgis-28 --without-grass
brew linkapps qgis-28