# snowemergency-etl
ETL for the Snow Emergency Data Sets

# Project Description
The City of Minneapolis has made data available for tows and tags resulting from snow emergencies.  This repository includes Jupyter notebooks that perform data preparation for these data sets.

# Methods Used
## Towing Data
For two of the snow emergencies, Grant 2015 and Polk 2016, the `.csv` files do not have latitude, longitude, ward, community or neighborhood information available.  We are able to use the GeoJSON files to extract the latitude and longitude information.  Using the latitude and longitude, we are then able to find the corresponding ward, community and neighborhood of each point.  In addition, for these emergencies, there are towing incidents that do not have latitude and longitude information but do have address information.  We geocode the latitude and longitude using the Google geocoding API.

The steps used for the Grand and Polk Towing Data are as follows:

1.  Fill missing coordinates by using Google Places to geocode the given address information.
2.  Fill missing Ward, Community, and Neighborhood information in using shapely's `polygon.contains(point)` functionality. We have GeoJSONs with the boundaries for Minneapolis Wards, Communities, and Neighborhoods. For each point, we can write a function that returns the corresponding Ward, Community, and Neighborhood.
3.  Extract the GeoDataFrame to an ordinary Pandas DataFrame by dropping the geometry column to a pair of columns: latitude and longitude.
4.  Split the datetime string into a date and time field.
5.  Write the final data frame to a .csv file to be combined with the other cleaned files.

For the remaining snow emergencies, the `.csv` files for the Snow Emergency data sets differ between snow emergencies. Columns have differing names, different columns are included in different files, community and neighborhood names are not standard, dates and times are in different formats, etc. For each file, we will transform each file to have the following columns:

    Date, Time, Location, Latitude, Longitude, Day, Ward, Community, Neighborhood, Emergency.

To achieve this, we:

1.  Rename columns to have uniform names.
2.  Drop extra columns.
3.  Pick a uniform date and time format and split the datetime string into a date and time field.
4.  Pick uniform neighborhood and community names. The easiest way to do this is to use the City of Minneapolis OpenData GeoJSON files of communities and neighborhoods for uniform names.
5.  Write the final data frame to a `.csv` file to be combined with the other cleaned files.

The final step for the towing data sets is then to read all sixteen files back in and concatenate the data frames producing a single data frame that is then written back out to a `.csv` file.
