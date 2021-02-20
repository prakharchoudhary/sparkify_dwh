# Project Datawarehouse

## Project description

Sparkify is a music streaming startup with a growing user base and song database.

Their user activity and songs metadata data resides in json files in S3. The goal of the current project is to build an ETL pipeline that extracts their data from S3, stages them in Redshift, and transforms data into a set of dimensional tables for their analytics team to continue finding insights in what songs their users are listening to. 

## How to run

1. To run this project fill the following information in a config file, *dwh.cfg*, present in the project's root folder.

```
[CLUSTER]
HOST='<CLUSTER-ENDPOINT-WITHOUT-PORT>'
DB_NAME='<DATABASE-NAME>'
DB_USER='<DATABASE-USER-NAME>'
DB_PASSWORD='<DATABASE-PASSWORD>'
DB_PORT=5439

[IAM_ROLE]
ARN='<IAM-ROLE-ARN>'

[S3]
LOG_DATA='s3://udacity-dend/log_data'
LOG_JSONPATH='s3://udacity-dend/log_json_path.json'
SONG_DATA='s3://udacity-dend/song_data'
```

2. Run the *create_tables.py* script to set up the staging and analytical tables in the database.

    `$ python create_tables.py`

3. Run the *etl,py* script to extract data from the log and song files present in S3, load it in staging tables in redshift, and lastly store it in the dimensional tables.

    `$ python etl.py`


## Project structure

This project includes:

- create_table.py is used to create the schema for staging, fact and dimension tables.
- etl.py is is used to load data from S3 into staging tables on Redshift and then processed into the star schema tables.
- sql_queries.py carries SQL statements which are used by *etl.py*, *create_table.py*.
- README.md is current file.

## Database schema design
State and justify your database schema design and ETL pipeline.

#### Staging Tables
- staging_events
- staging_songs

####  Fact Table
- songplays - records in event data associated with song plays i.e. records with page NextSong - 
*songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent*

#### Dimension Tables
- users - users in the app - 
*user_id, first_name, last_name, gender, level*
- songs - songs in music database - 
*song_id, title, artist_id, year, duration*
- artists - artists in music database - 
*artist_id, name, location, lattitude, longitude*
- time - timestamps of records in songplays - 
*start_time, hour, day, week, month, year, weekday*

### Steps followed in this project

1. Create Table Schemas
- Design schemas for your fact and dimension tables
- Write a SQL CREATE statement for each of these tables in sql_queries.py
- Complete the logic in create_tables.py to connect to the database and create these tables
- Write SQL DROP statements to drop tables in the beginning of - create_tables.py if the tables already exist. This way, you can run create_tables.py whenever you want to reset your database and test your ETL pipeline.
- Launch a redshift cluster and create an IAM role that has read access to S3.
- Add redshift database and IAM role info to dwh.cfg.
- Test by running create_tables.py and checking the table schemas in your redshift database. You can use Query Editor in the AWS Redshift console for this.

2. Build ETL Pipeline
- Implement the logic in etl.py to load data from S3 to staging tables on Redshift.
- Implement the logic in etl.py to load data from staging tables to analytics tables on Redshift.
- Test by running etl.py after running create_tables.py and running the analytic queries on your Redshift database to compare your results with the expected results.
- Delete your redshift cluster when finished.

3. Document Process
Do the following steps in your README.md file.

- Discuss the purpose of this database in context of the startup, Sparkify, and their analytical goals.
- State and justify your database schema design and ETL pipeline.
- [Optional] Provide example queries and results for song play analysis.