### Requirements
- Oracle_VM box
- Ubuntu 
- docker
- Python-container
- Java 8 [Open JDK 1.8]
- Hadoop-container 
- Jupyter-pyspark
- Kafka (3 node containers)
- connect
- Cassandra-container
- MySQL-container
- Superset-container

## Dataset Overview
The dataset used in this system consists of two CSV files:
- `Test_Churn.csv`: 
    - This is iterated over and fed to the topic of the Kafka producer
- `Train_Test.csv`:
    - This is loaded to the Hadoop DFS where it can be accessed by Spark

## System Architecture
This system seamlessly streams telecom customer data from a local file to a Kafka topic. Spark Streaming processes the data and stores the raw stream in a Cassandra database. It then uses a machine learning algorithm{ChurnPrediction.ipynb} to predict customer churn, saving these predictions in a MySQL database. HDFS stores the annotated customer data used to train the machine learning model. A live dashboard on Superset, powered by the MySQL database, provides real-time insights.

Here's an overview of the system's architecture:

***************** {project Architecture.png} ******************

### Kafka Producer
The `test_churn.csv` file from local storage is used to simulate a continuous stream of data representing customer data that is continuously updated in the Telecom platform. 
This setup allows for testing and development of stream processing pipelines in a controlled environment before deploying them to handle live data streams.

### Spark Structured Streaming
Spark Streaming is used to consume data from the Kafka topic and execute transformation (ML) and write operations to both a Cassandra/shared_volume and MySQL database. Initially, the raw stream is stored in a Cassandra database/shared_volume for archival purposes. Subsequently, the test_churn data is enriched by applying predictions from the ML model trained by the train_churn data retrieved from HDFS. This process creates a new data structure that facilitates visualization and tracking of churn predictions for Analysis and Strategy & planning.

### Real-time Dashboard
Superset is employed for visualizing the aggregated data, allowing users to monitor and track desired metrics and key performance indicators (KPIs). 
This is accomplished by establishing a connection to the MySQL database and fetching data from the dedicated table where it is stored. 
To maintain real-time data updates, the data state is periodically refreshed, ensuring that the dashboard reflects the most current information.

