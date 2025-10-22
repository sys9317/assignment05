# Assignment 05, Yosup Shin

The first exercise reads in our full crime dataset, manipulating the longitude
and latitude variables in as character columns. We then make the variables 
names lower case.

In the second exercise, we manipulated the dataset to only look at the last 10
years (from today's date) of homicide data, so since 2015.

Exercise 3, we started off by converting the longitude and latitude columns in
the crimes data (which is limited to the past 10 years of homicide data) into
spatial geometries (points) before we plotted them. We then used ggplot to 
replicate the map provided, coding arrests in blue and non-arrests in red. 

In exercise 4, we read in the chicago shape file, specifically the geoid10 and
geometry variables. Using spatial join, we matched each point in the crimes 
dataset (our original dataset from exercised 2 and 3) with a census tract 
polygon (from the chicago dataset). We then calculated the number of homicides,
as well as the proportion of homicides where arrests occur for each census 
tract (or arrest location) and saved that in a new dataframe called 
chicago_merged_agg. We then joined that with the chicago dataset and recreated
the maps provided, using scale_fill_gradient to illustrate the high versus low
crime counts and arrest rates.

In exercise 5, we downloaded and saved our API key. We used codes from the 
tidycensus package to retrieve the median house-hold income, population with a
bachelor’s degree, and population below the poverty line for each census tract
in Cook County, Illinois. We retrieved them by filtering within the label and 
concept variables. To test that we had done this correctly, we created a URL 
that retrieves the same data. We used the httr and jsonlite packages to write a
GET request and pull the data down. We used the parse code from Aaron's
textbook to help us convert the data into text, a matrix, and ultimately a 
dataframe. After creating this new API dataframe, we  began the process of
confirming it was the same as the one made by tidycensus. We did this by using 
the test_that command, within which we compared our API data with our tidycensus
data, specifically checking that they were equal. The test passed, telling us 
that it was. 