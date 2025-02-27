# CMPD Crime Dashboard

![image](https://github.com/user-attachments/assets/874ea665-026d-49f4-8bd8-a986e88a9c9f)
![image](https://github.com/user-attachments/assets/ab46af7c-a1c3-4fef-904f-5089998d2455)
![image](https://github.com/user-attachments/assets/ed2a7393-43d8-4fd0-a117-c04c4ef47327)




## The Dashboard

This multi-page dashboard was primarily created using the JavaScript and CSS based Dash python library. The data used in the dashboard was obtained from Charlotte.gov’s open data portal. The data required a fair amount of cleaning and manipulation which I will touch on later. The first page uses aggregate data of crimes to create four graphs and 2 “cards”. The content on the first page can be filtered by either clicking on a zip code in choropleth map, hovering over a data point on the line graph, or selecting a crime in the dropdown menu. The second page is a day-by-day breakdown of crimes in Charlotte and displays exactly where crimes happened and their corresponding information. The second page also displays what crime was the most commonly committed and which CMPD district had the most incidents. The graphs on the second page can be filtered using the date-picker or the crime dropdown menu. The third page shows an animated choropleth map of the zip codes in Charlotte, colored by the amount of incidents in each zip code. The choropleth map is animated by displaying the choropleth map every month from earliest to latest.


## How to Run 

The dashboard was originally intended to be deployed on Google Cloud Run, but due to the intense computing needed to run some of the functions to filter datasets along with inefficiencies in Dash callbacks, the dashboard is now only available to run locally. To run, first clone the repository to your local device. Make sure to download all the requirements listed within the requirements.txt folder (I recommend using a virtual env). When running the dashboard, you can either use the cleaned dataset already in the repository titled “CMPD_Cleaned.csv” (last updated in April of 2024) or you can download the most recent version of “CMPD Incidents” from the charlotte.gov open data portal (https://data.charlottenc.gov/datasets/d22200cd879248fcb2258e6840bd6726_0/explore ). If you chose to download a more recent version, save it as a CSV file named “CMPD_Incidents.csv” and place it in the directory of the cloned repository. Then, execute all the cells of CmpdDataCleansing.ipynb and you should have a new version of the cleaned data set called “CMPD_Cleaned.csv”. Once you have the copy of “CMPD_Cleaned.csv” that you want in the directory, you can now open CmpdDashboard.ipynb and execute the cells. A dash window should then open up in the browser of the notebook displaying the graph!

## Link to Video Demoing Dashboard

If you do not wan't to download all the required files to run the dashboard locally, I have provided a link to a youtuve video showcasing the dashboard. https://www.youtube.com/watch?v=2-bToSJnEtA

## The Purpose

For most of my life, I thought I wanted to work in law enforcement. I was a part of CMPD’s youth “explorers” program as well as their “Envision Academy”. Since learning to program, I have wanted to combine my prior interest with my current ones to make this project idea a reality. Additionally, I made the dashboard in hopes that people that use it will be better informed about the areas of Charlotte and the incidents going on around them. Since the news doesn’t report on every crime in the area, when people hear a lot of police cars, they can simply check the second page on the dashboard to find out what took place.

## The Data Cleaning and Manipulation

As mentioned earlier, the dataset came from Charlotte.gov’s open data portal in a file called “CMPD Incidents”. The dataset has about 550,000 rows with relevant information about the incident reported. My primary data cleanse came from removing data points that were of non-criminal crimes or crimes committed in zip codes outside of Charlotte. This brought the total entries down ~3,000 (which is not statistically significant). The most difficult part of the data cleansing process was filling missing null zip code values in order to make an accurate choropleth map. Out of all the rows, +168,000 of the records did not have zip code values– a big issue when trying to make a choropleth map of crimes grouped on zip codes. I filled zip codes in two different ways. My first approach was to use the data points that had a non-null address value and used the address to find its corresponding zip code in Charlotte.gov’s “Master Address” dataset from the open data portal. This brought the amount of null zip code values to just over 91,000. My second solution was to use the x and y coordinates from the data points, a list of the x and y coordinates of the edges of zip code boundaries in NC (obtained from: https://github.com/OpenDataDE/State-zip-code-GeoJSON/blob/master/nc_north_carolina_zip_codes_geo.min.json ), and the Python library Shapely which has a function allowing you to check if a point lies within a polygon (a collection of xy coordinates– like a zip code). So for each null zip code data point, I iterated through a list made of sublists that contained the coordinates of zip code boundaries and checked if the data point’s coordinate lies within the zip code. This led me to 0 null zip code values! I also added a weekday field using the “date reported” field in the dataset and the datetime library. 
