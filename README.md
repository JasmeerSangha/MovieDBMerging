# Movies-ETL
## Overview
Asked to retreive and merge Wikipedia and Kaggle data focussed on movies. The resulting database will be used for future hackathon participants to experiment with. For convenince, challenge.ipynb has been created to automate any merging of future data assuming similar make-up.

## Resources
- Data Sources: wikipedia.movies.json, ratings.csv, movies_metadata.csv
- Software/Tools: pgAdmin, Jupyter Lab(pandas)
- Languages: SQL, Python

## Results
### Extraction
The code depends on the wikipedia data being in json format and the kaggle data being in csv format. The code is limited by to data that contains the same column names referenced in the cleaning functions.
### Transformations
The wikipedia, kaggle and ratings datasets are joined on their imdb ids, which were extracted using regular expressions. Significant effort was directed towaards combining columns representing similar attributes ( such as "Director" and "Directed by") in order to clean the data. The final table named "movies_with_ratings_df" has 42 columns whos values were preferentially filled with kaggle data as it was trusted and more consistant compared to the wikipedia data. Wikipedia data was used for columns that did not exist in the kaggle data or used to fill null values of those columns which were represented in both frames such as run-time, budget, and revenue. The ratings data was significantly compressed by creating new columns within the dataframe. Each showing the number of x star ratings for each movie, where x varies from 0.5 to 5 stars.

#### Notables ####
The code relies on all column names to stay consistent for future datasets, disregarding those that are filtered out early in the data cleaning process. To increase the robustness of our code a few changes were made:
- when changing kaggle data types to numeric the key word coerce is passed, this ensures any non-numerica values are returned as NaN values instead of errors
- when comparing release dates of movies the code drops all movies which the dates are mismatched by 1000 days (approximately 3 years)
- one of the regular expressions to extract dates has been updated to `date_form_two = r'\\d{4}.[01]\\d.[0123]\\d'` this can now also recognise dates which are released on the 1st-9th of every month, previous versions erroneously filtered these dates out
### Loading
Loading the data into pgAdgmin to create an SQL file is straight forward and assumes that the user has a database named "movie_data" and has imported their pgAdmin password from a config file. Currently the code has been designed to upload movies_with_ratings_df which has combined the wikipedia kaggle and ratings data into a single table, as described in the transform section. Due to the size of ratings.csv, loading it into SQL has currently been disabled. If needed the data loop can be enabled to create an SQL table. In order to prevent overloading, the function has been chunked into blocks of size one million. The function has been designed to overwrite any previous data populating these tables. If desired, this can be easily changed by directing the data to a new database.
