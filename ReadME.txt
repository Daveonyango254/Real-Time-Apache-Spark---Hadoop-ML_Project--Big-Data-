### Requirements
- Ubuntu [Tested on Ubuntu 22.04]
- Python 3.7
- Java 8 [Open JDK 1.8]
- Hadoop 3.3.6
- Spark 3.3.3
- Kafka 3.5.1
- Cassandra 4.1.3
- MySQL 8.0.34
- Superset

## Dataset Overview
The dataset used in this system consists of two CSV files:
- `Test_Churn.csv`: 
    - This is iterated over and fed to the topic of the Kafka producer
- `Train_Test.csv`:
    - This is loaded to the Hadoop DFS where it can be accessed by Spark

## System Architecture
This system seamlessly streams Telecom customers data from a local file to a Kafka topic. 
Spark Streaming ingests the stream and takes charge of processing this data, initially storing the raw stream in a Cassandra database. 
It further enhances the data by applying machine learning Algorithm to do a prediction of whether a customer is likely to churn or not and subsequently saves the predictions in a MySQL DB in a table.
HDFS stores  the annotated version of customer data that is used to train the Machine Learning Model used for the Classification task.
MySQL database powers the creation of a live dashboard on Superset, offering real-time insights. 

Here's an overview of the system's architecture:

####################Link to be placed###############################

### Kafka Producer
The `test_churn.csv` file from local storage is used to simulate a continuous stream of data representing customer data that is continuously updated in the Telecom platform. 
This setup allows for testing and development of stream processing pipelines in a controlled environment before deploying them to handle live data streams.

### Spark Structured Streaming
Spark Streaming is used to consume data from the Kafka topic and execute transformation (ML) and write operations to both a Cassandra and MySQL database. Initially, the raw stream is stored in a Cassandra database for archival purposes. Subsequently, the test_churn data is enriched by applying predictions from the ML model trained by the train_churn data retrieved from HDFS. This process creates a new data structure that facilitates visualization and tracking of churn predictions for Analysis and Strategy & planning.

### Real-time Dashboard
Superset is employed for visualizing the aggregated data, allowing users to monitor and track desired metrics and key performance indicators (KPIs). 
This is accomplished by establishing a connection to the MySQL database and fetching data from the dedicated table where it is stored. 
To maintain real-time data updates, the data state is periodically refreshed, ensuring that the dashboard reflects the most current information.

