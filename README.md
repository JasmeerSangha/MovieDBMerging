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
The wikipedia, kaggle and ratings datasets are joined on their imdb ids, which were exracted using regular expressions. 
### Loading
Loading the data into pgAdgmin to create an SQL file is straight forward and assumes that the user has a database named "movie_data" and has imported their pgAdmin password from a config file. Currently the code has been designed to upload movies_with_ratings_df which has combined the wikipedia kaggle and ratings data into a single table, as described in the transform section. Due to the size of ratings.csv, loading it into SQL has currently been disabled. If needed the data loop can be enabled to create an SQL table. In order to prevent overloading, the function has been chunked into blocks of size one million. The function has been designed to overwrite any previous data populating these tables. If desired, this can be easily changed by directing the data to a new database.
