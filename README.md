# Assignment 05, Yosup Shin

The first part of this assignment focuses on reading and cleaning the Chicago 
crime dataset. When loading the dataset, the read_csv() function specifies 
the data types for the Longitude and Latitude columns as character variables 
using col_character() to avoid misinterpretation by R. This is an important 
step because longitudeand latitude values may include missing entries or 
inconsistencies that could otherwise cause parsing errors. 

After loading the data, all variable names are standardized — white spaces are 
replaced with underscores, and all letters are converted to lowercase — 
ensuring consistency and easier reference 
throughout the analysis.

The second question filters the dataset to retain only the relevant subset of 
observations. 
Specifically, it isolates rows where the primary_type variable equals “HOMICIDE,” 
where both longitude and latitude values are present, and where the date of 
the incident occurred within the past ten years relative to the current date. 
This rolling window approach, implemented using the lubridate package, 
keeps the dataset dynamically updated each time the analysis is rerun 
without the need to hard-code specific years.

In the third question, the filtered homicide dataset is converted into a 
spatial object. Using the sf package, the longitude and latitude columns are 
transformed into spatial geometries (points) with a coordinate reference system 
(CRS) with the value of 4326. 
This allows the dataset to be treated as geospatial data that can be visualized 
or spatially joined to shapefiles. The "remove = FALSE" make sure the data
keeps the original variables.
The geographic distribution of homicides is then visualized using ggplot2. 
Each point represents a homicide, color-coded according to whether 
an arrest was made: blue for TRUE and pink for FALSE. This map provides 
a clear spatial overview of where homicides and subsequent arrests have 
occurred across the city.

The fourth section integrates external geographic data by reading the 
Chicago census tract shapefile. 
From this shapefile, only the geoid10 and geometry columns are selcted using 
select function, as these contain the unique census tract identifiers and 
their spatial polygons. 

A spatial join is then performed using st_join(), linking each homicide point 
to the census tract polygon in which it occurred. 
After the join, the data are grouped by tract (geoid10), 
and summary statistics are calculated — including the 
total count of homicides and the arrest rate (the proportion of homicides 
that resulted in an arrest). 

These aggregated results are stored in a new dataset, chicago_merged_agg, 
which is then merged back with the original tract geometry to enable tract-level 
mapping. Finally, ggplot2 visualizations use color gradients to highlight 
spatial patterns in homicide counts and arrest rates across Chicago.

The fifth part extends the analysis by integrating socioeconomic data from the 
American Community Survey (ACS) through the tidycensus package. 
After setting and securely storing a Census API key, the code retrieves 
three key variables for all census tracts in Cook County, Illinois: 
median household income, population with a bachelor’s degree, and population 
below the poverty line. 

These variable codes are identified using the load_variables() function, 
which allows users to search through ACS metadata. 
To validate the accuracy of this retrieval, the same data are requested directly 
from the U.S. Census API using the httr and jsonlite packages. 
The API response is parsed into a dataframe following the same structure 
as the tidycensus output. A unit test, implemented with the testthat package, 
confirms that both datasets contain the same number of observations, ensuring 
consistency between the tidycensus and direct API methods.