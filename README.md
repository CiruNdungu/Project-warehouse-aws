INTRODUCTION

Sparkify, a music streaming startup has grown its userbase and song base and want to move their data to the cloud. The data resides in S3 directories as JSON logs for user activity on the app and JSON metadata on songs. 

REQUIREMENT/OBJECTIVE

1. The analytics team wants the data loaded on the cloud with tables designed to optimise queries on songplays data
2. The analytics team wants to gather more insights on what songs their users are listening to by analysing the tables

PROCESS

The task is to build an ETL pipeline that extracts data from S3, stages in Redshift and transforms the data into a set of dimensional tables for use by the analytics team

1. Create and launch a Redshift cluster on AWS
2. Define the fact and dimension tables then create a star schema and ETL pipeline to explore and load data from S3 to the Redshift staging tables
3. Connect to the Redshift cluster and run sample queries to validate the data

DATASETS

The original data is in JSON format and resides in the AWS S3 public available data. 

Song data: s3://udacity-dend/song_data
Log data: s3://udacity-dend/log_data
Log data json path: s3://udacity-dend/log_json_path.json

Song data is a subset of real data from the Million Song Dataset. Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID

The 'log data', dataset consists of log files in JSON format generated by this event simulator based on the songs in the dataset above. These simulate app activity logs from an imaginary music streaming app based on configuration settings. The log files in the dataset are partitioned by year and month. 

DATABASE SCHEMA

Using the song and event datasets a star schema optimized for queries on song play analysis was created. This includes the following tables.
Fact Table
songplays - records in event data associated with song plays i.e. records with page NextSong
•songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

Dimension Tables

users - users in the app
•user_id, first_name, last_name, gender, level
songs - songs in music database
•song_id, title, artist_id, year, duration
artists - artists in music database
•artist_id, name, location, lattitude, longitude
time - timestamps of records in songplays broken down into specific units
•start_time, hour, day, week, month, year, weekday

![schema text](https://user-images.githubusercontent.com/116004104/197519555-02aa2e5b-267d-4a66-9bdd-6e504d0d2828.jpeg)

ETL PROCESS

SETUP

1. Setup the IAM user in your AWS account and give it administrator access and attach policies. With this role, Redshift should access the S3 bucket as ReadOnly
2. Create clients for EC2, S3 IAM and Redshift with Access Key and Secret Key
2. Create the Redshift cluster, get the HOST and ARN and fill the dwh.cfg file with the details

ETL PIPELINE

1. Run create_tables.py to drop previous tables, if they exist, and create new tables.
2. Run etl.py to load the data from S3 into the staging tables then transfer into the target fact and dimension tables

HOW TO RUN

To run the project, 
1. Run create_tables.py on the terminal to drop and create the tables 
2. Run etl.py to execute the ETL process

SAMPLE QUERIES

Once the ETL process is complete, the following sample queries can be run;
1. Top 5 most played songs

SELECT sp.song_id, s.title, count(*) AS count 
    FROM songplays sp
    JOIN songs s
      ON sp.song_id = s.song_id
GROUP BY 1, 2
ORDER BY 3 DESC
   LIMIT 5;
   
   
   
   
2. Top 5 artists

SELECT sp.artist_id, a.name AS artist_name, count(*) AS count
    FROM songplays sp
    JOIN artists a
      ON sp.artist_id = a.artist_id
GROUP BY 1, 2
ORDER BY 3 DESC
   LIMIT 5;
   
   






