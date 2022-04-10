#### This repository consists of two tasks
1) Data Preprocessing Pipeline
2) Translation Web Server 

![Alt text](resources/MTXArchitecture.jpg?raw=true "Title")

# Data Preprocessing Pipeline: 
A end to end pipeline that takes the given tmx file, extracts the data,transforms or filters the data and loads it into persistant layer.

The Data Preprocessing Pipeline is divided into two stages
- Stage 1|Data Extraction: The data is extracted in parallel and pushed into the kafka topic
- Stage 2|Data Cleaning: The streaming data from the kafka topic is transformed (filtered) in parallel and persisted on to the elastic search.
## Running the Project
### Techincal Documentation
The entire landscape is dockerized and connected on a netwrok bridge. Each module runs on a separate docker container (this gives us the feasibility and ease for scaling up the project)


For setting up the data preprocessing pipeline the
docker-compose is available in d repo that holds the containers for kafka brokers, data_extraction and data_cleaning. For elasticsearch the docker-compose is at database repo
- all the containers runs on lengoo_mtx network
- attach to individual containers in development for eval and debugging

## Elasticsearch
- Elasticsearch with two nodes runs on a container
### TO Run
 The docker-compose.yml is available in the database reopo which also takes care of creating index and mapping on startup 
 ```
docker-compose build
docker-compose up 
```
#### Before starting the remaining modules lets start the docker landscape provided in the docker repo

### TO Run
 The docker-compose.yml is available in the docker repo

 ```
docker-compose build
docker-compose up 
```
Now lets attach the the running containers to execute each module individually only for development and evaluation

## Kafka Cluster
-  A zookeeper and three brokers run on separate contaniers.
(refer kafka repo for more information for adding additional brokers)


## Data Extraction
- The .tmx file is extracted and multiple worker nodes run in parallel to unwrap the src and target data and publish the data on to the kafka topic
- For configuration please refer the config file in the repo please set the path for the file

### Requirements
- Requires that the kafka zookeeper is running

### To Run
- Set up the parameters including the tmx file path, kafka topic to be published on in config.py and message_publisher/config.py

- To attach to the running container

```
docker exec -it kafka-producer-parser bash
```

- To run the data extraction module execute the below comand
```
python data_extraction.py
```

## Data Cleaning
- This module uses python Faust for stream processing data from kafka topic to preprocess the data and persist on to the elasticsearch

### Requirements
- Requires that the kafka zookeeper is running
- Requires that the Elasticsearch cluster is up

### To Run
- Set up the parameters kafka topic, elasticsearch broker on in config.py

- To attach to the running container
```
docker exec -it kafka-consumer-filter bash
```
- To run the data extraction module execute the below comand
c
- Multiple nodes can be intintiated on different instance for parallel processing

# Translation Web Server
#### Now that our data has been persisted the translation web server provides a simple web interface to retrive the persisted data

- This module uses FastAPI to create endpoints to access the data and swagger for interface
- In short it mimics NLP translation model on the backend

### To Run
To run the module the docker-compose file is available in the the translation_web_server submodule

```
docker-compose build
docker-compose up
```

To access the the swagger doc
 http://127.0.0.1:8000/docs


