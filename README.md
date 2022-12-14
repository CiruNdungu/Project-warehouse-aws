**INTRODUCTION**

Sparkify, a music streaming startup has grown its userbase and song base and want to move their data to the cloud. The data resides in S3 directories as JSON logs for user activity on the app and JSON metadata on songs. 


**REQUIREMENT/OBJECTIVE**

1. The analytics team wants the data loaded on the cloud with tables designed to optimise queries on songplays data
2. The analytics team wants to gather more insights on what songs their users are listening to by analysing the tables


**PROCESS**

The task is to build an ETL pipeline that extracts data from S3, stages in Redshift and transforms the data into a set of dimensional tables for use by the analytics team

1. Create and launch a Redshift cluster on AWS
2. Define the fact and dimension tables then create a star schema and ETL pipeline to explore and load data from S3 to the Redshift staging tables
3. Connect to the Redshift cluster and run sample queries to validate the data


**DATASETS**

The original data is in JSON format and resides in the AWS S3 public available data. 

**Song data**: s3://udacity-dend/song_data

**Log data**: s3://udacity-dend/log_data

**Log data json path**: s3://udacity-dend/log_json_path.json


Song data is a subset of real data from the Million Song Dataset. Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID

The 'log data', dataset consists of log files in JSON format generated by this event simulator based on the songs in the dataset above. These simulate app activity logs from an imaginary music streaming app based on configuration settings. The log files in the dataset are partitioned by year and month. 


**DATABASE SCHEMA**

Using the song and event datasets a star schema optimized for queries on song play analysis was created. This includes the following tables.


**Fact Table**

**songplays** - records in event data associated with song plays i.e. records with page NextSong



**Dimension Tables**

**users** - users in the app

**songs** - songs in music database

**artists** - artists in music database

**time** - timestamps of records in songplays broken down into specific units




![schema diag](https://user-images.githubusercontent.com/116004104/207310748-829e2a9a-8748-47cb-910d-861b3c8086ed.JPG)


**ETL PROCESS**


**SETUP**

1. Setup the IAM user in your AWS account and give it administrator access and attach policies. With this role, Redshift should access the S3 bucket as ReadOnly
2. Create clients for EC2, S3 IAM and Redshift with Access Key and Secret Key
2. Create the Redshift cluster, get the HOST and ARN and fill the dwh.cfg file with the details


**ETL PIPELINE**

1. Run create_tables.py to drop previous tables, if they exist, and create new tables.
2. Run etl.py to load the data from S3 into the staging tables then transfer into the target fact and dimension tables


**HOW TO RUN**

To run the project, 
1. Run create_tables.py on the terminal to drop and create the tables 
2. Run etl.py to execute the ETL process


**SAMPLE QUERIES**

Once the ETL process is complete, the following sample queries can be run;

**1. Top 5 most played songs**

![top 5 songs query](https://user-images.githubusercontent.com/116004104/207312275-06aa30ab-e297-4038-be6b-c05a03847873.JPG)

   
   
![top 5 songs](https://user-images.githubusercontent.com/116004104/207310864-1090053e-5639-4a2d-84b6-452572ea410a.JPG)

   

**2. Top 5 artists**


![top 5 artists query](https://user-images.githubusercontent.com/116004104/207312355-f8fac4b1-067f-49ac-8304-bab758154569.JPG)

![Top 5 artists](https://user-images.githubusercontent.com/116004104/207310896-303958fc-01a7-40c8-bebc-49e252900d95.JPG)







