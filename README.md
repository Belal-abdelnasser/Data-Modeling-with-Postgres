# Data Modeling with Postgres

## Scope

* I want to analyze the data has been collected on songs and user activity on new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

* I'd like to create a Postgres database with tables designed to optimize queries on song play analysis, and bring you on the project. Your role is to create a database schema and ETL pipeline for this analysis. You'll be able to test your database and ETL pipeline by running queries given to you by the analytics team from Sparkify and compare your results with their expected results.

## Dependencies

1. Python 3.0
2. Psycopg2, Pandas
3. PosgreSQL

## How to use

Run these commands at the terminal

1. python create_tables.py
2. python etl.py

## Project files

### Directory: data/log_data

This directory contains a collection of JSON log files. These files are used to populate our Fact table - Song Plays - and to populate the Dimension tables for Users and Time.

### Directory: data/song_data

This directory contains a collection of Song JSON files. These files are used to populate Dimension tables for Songs and Artists.

### create_tables.py

This Python script recreates the database and tables used to store the data.

### etl.ipynb

A Python Jupyter Notebook that was used to initially explore the data and test the ETL process.

### etl.py

This Python script reads in the Log and Song data files, processes and inserts data into the database.

### sql_queries.py

A Python script that defines all the SQL statements used by this project.

### test.ipynb

A Python Jupyter Notebook that was used to test that data was loaded properly.

## Dataset

### 1. Song Dataset

The first dataset is a subset of real data from the [Million Song Dataset](http://millionsongdataset.com/). Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are file paths to two files in this dataset.

> song_data/A/B/C/TRABCEI128F424C983.json
> song_data/A/A/B/TRAABJL12903CDCF1A.json

And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like.

```json
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
```

### 2. Log Dataset

The second dataset consists of log files in JSON format generated by this [event simulator](https://github.com/Interana/eventsim) based on the songs in the dataset above. These simulate activity logs from a music streaming app based on specified configurations.

The log files in the dataset you'll be working with are partitioned by year and month. For example, here are filepaths to two files in this dataset.

> log_data/2018/11/2018-11-12-events.json
> log_data/2018/11/2018-11-13-events.json

And below is an example of what the data in a log file, 2018-11-12-events.json, looks like.
![Log-Data](./log-data.png "Log-Data")
This data contains information of which songs Users listened to at a specific time. Information is parsed to provide data for the Songplays Fact table and the Users and Time Dimension tables. The songplays.artist_id and songplays.song_id columns are populated by a lookup based on the Song Title, Artist Name and song Duration.

## Schema

Using the song and log datasets, I need to create a star schema optimized for queries on song play analysis.
After examining the Log and Song JSON files, I created a Star schema (shown below) that include one Fact table (songplays) and 4 Dimension tables.

![Schema](./Blank diagram.jpeg "Design")

### Fact Table

1. songplays - records in log data associated with song plays i.e. records with page `NextSong`
    * songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

### Dimension Table

1. users - users in the app
   * user_id, first_name, last_name, gender, level
2. songs - songs in music database
   * song_id, title, artist_id, year, duration
3. artists - artists in music database
   * artist_id, name, location, latitude, longitude
4. time - timestamps of records in songplays broken down into specific units
   * start_time, hour, day, week, month, year, weekday

## Project Steps

### Create Tables

1. Write `CREATE` statements in `sql_queries.py` to create each table
2. Write ``DROP` statements in `sql_queries.py` to drop each table if it exists.
3. Run `create_tables.py` to create your database and tables.
4. Run `test.ipynb` to confirm the creation of your tables with the correct columns.

### Build ETL Processes

In the `etl.ipynb` notebook,I developed ETL processes for each table. At the end of each table section, or at the end of the notebook, I run `test.ipynb` to confirm that records were successfully inserted into each table.

### Build ETL Pipeline

Use what has done in etl.ipynb to complete etl.py, where it'll process the entire datasets.
Remember to run create_tables.py before running etl.py to reset your tables. Run test.ipynb to confirm your records were successfully inserted into each table.
