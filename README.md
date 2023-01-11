# Data Warehouse Project Using AWS Redshift

## Summary

This project requires us to create a data pipeline for Sparkify, a music listening service looking for analytics on song and event data. 

The project combines song listen log files and song metadata to facilitate analytics. An AWS Redshift cluster is created using infrastructure as code through the Python SDK. Following cluster creation a data pipeline built within Python and SQL prepares the data schema for analytics. JSON data is copied from an S3 bucket to Redshift staging tables before being inserted into a star schema with fact and dimension tables. A star schema is perfect for this particular application since denormalization is easy, queries can be kept simple, and aggregations are fast.


## Files
CluserLaunchAndQueries.ipynb -- Contains cells of code to create IAM role, Redshift cluster, allow TCP connection, run the create_tables.py and etl.py scripts, and query tables in SQL

create_tables.py -- Drop and recreate tables

dwh.cfg -- Configure Redshift cluster and data import

etl.py -- Copy data to staging tables and insert into star schema fact and dimension tables

sql_queries.py -- Creating and dropping staging and star schema tables, copying JSON data from S3 to Redshift staging tables, inserting data from staging tables to star schema fact and dimension tables


## Running the scripts

-Set AWS variables in dwh.cfg (AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY)

-Choose a DB_NAME and DB_PASSWORD in dhw.cfg

-Run the cells in CluserLaunchAndQueries.ipynb to Create IAM role, Redshift cluster, and configure TCP connectivity

-Complete HOST & ARN in dwh.cfg with outputs from CluserLaunchAndQueries.ipynb

-Drop and recreate tables by running the %run 'create_tables.py' script in the notebook

-Run ETL pipeline by running the %run 'etl.py' script in the notebook

-Delete IAM role and Redshift cluster with the appropriate cells in the notebook

## Query Examples

These queries and their outputs can be found in the notebook

A quick test query:
SELECT * FROM songplays ORDER BY songplay_id LIMIT 10;

A query to find the most played songs:
SELECT sp.song_id, sp.artist_id, a.name as artist_name, s.title as song_title, count(*) AS plays 
FROM songplays sp 
JOIN songs s 
ON sp.song_id = s.song_id 
JOIN artists a ON sp.artist_id = a.artist_id 
GROUP BY 1, 2, 3, 4 
ORDER BY 5 DESC 
LIMIT 10;

A query to find the most popular artists:
SELECT sp.artist_id, a.name AS artist_name, count(*) AS plays 
FROM songplays sp 
JOIN artists a 
ON sp.artist_id = a.artist_id 
GROUP BY 1, 2 
ORDER BY 3 DESC 
LIMIT 10;

A query to find what areas most users live in:
SELECT location, count(*) AS plays 
FROM songplays 
GROUP BY 1 
ORDER BY 2 DESC 
LIMIT 10;

## Sources

1. PostgreSQL documentation
- https://www.postgresql.org/docs/15/index.html

2. Udacity Lessons

3. Redshift Python SDK documentation
- https://boto3.amazonaws.com/v1/documentation/api/latest/index.html