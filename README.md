# Movies_ETL

## Project Overview
	Write a Python script that performs all three ETL (Extract, Transform, Load) steps on the Wikipedia and Kaggle data. 

## Resources
	Data Source: The Movie Data Base (TMDb), Wikipedia Movies JSON
	Software: Python 3.8.3, Jupyter Notebooks
	
	A sample of the TMDb ratings is hosted on this repo. The full CSV file can be downloaded from here:
		https://www.kaggle.com/rounakbanik/the-movies-dataset
## Challenge Overview
A Python function that takes in three inputs (Wikipedia Movies JSON, TMDb Movie Metadata, and TMDb Movie Ratings) and sanitizes the data has been created. At the end the produced Dataframe is inserted into a Postgres SQL database. The ETL process is robust based on the assumptions outlined. Those assumptions are:
1. The updated data received will be in the same format
	  * We assume that the data we receive as our inputs will always be the same. Wikipedia pages are open to the public to be edited, and there can be changes in the data structure or JSON that comes from scraping.
	
2. Only columns with <90% null values are the important columns
	  * The code 
	  ``` 
	  wiki_columns_to_keep = [column for column in wiki_movies_df.columns if wiki_movies_df[column].isnull().sum() < len(wiki_movies_df) * 0.9] 
	  ``` 
	will drop columns depending on their values. In the future its possible that columns in our data set change because of null value percentage being greater than or less than 90. If columns change, there will be problems with the rest of the ETL when data tries to get inserted into the Postgres SQL Database.
	 
3. Only columns with relevant data are included
		Related to above, there may be other columns that come into scope that are needed but are filtered out of the data. One assumption through the project was the column 'video' was arbitrarily dropped and set to be only 'FALSE' values. 
	
4. Merged TMDb Movie Metadata and Wikipedia Movie JSON  
	  * Based on scatter plots of the data its assumed that TMDb Movie Metadata has slightly higher quality data than Wikipedia Movie JSON. So for instances where TMDb had missing data the Wikipedia data was used. This assumption was made strictly only at visualizing the data between the two data sources, and not through spot check validation or other means.
	
5. Only Wikipedia entries that existed in the TMDb Movie Metadata were used
	  * The Wikipedia Movie data was inner joined on IMDb Movie ID to the TMDb Movie Metadata which gives us only movies where there is a relevant Wikipedia entry. By the end of the project mostly TMDb data was used over Wikipedia data, which brings into question if there is a need for the Wikipedia data to be inner joined to TMDb. 
	