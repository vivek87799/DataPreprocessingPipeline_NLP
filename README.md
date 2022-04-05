# Data Pipeline: 
A simple project that takes the given tmx file parses the data and persists the data on to an elasticsearch

## Running the Project
#### Before giving an walkthrough on the project let us set up the landscape
- docker-compose is available in kafka repo that holds the containers for kafka brokers, data_extraction and data_cleaning. For elasticsearch the docker-compose is at database repo
- all the containers runs on lengoo_mtx network
- attach to individual containers in development for eval and debugging


## Kafka Cluster
-  This repo consists of three brokers running (refer kafka repo for more information for adding additional brokers)

## Elasticsearch
- There are two elasticsearch nodes running for distributed data persisting (refer database repo)




## Data Extraction
- A single process runs to extract the tmx file,this also takes care of handling large file on limited local memory
- multiple worker nodes run in parallel to filter and put the messages into the kafka topic

### Requirements
- Requires that the kafka zookeeper is running

### To Run
- Set up the parameters including the tmx file path, kafka topic to be published on in config.py and message_publisher/config.py
- Use the docker file provided to set up the container and to install the required packages
- To attach to the running container
```
docker exec -it kafka-producer-parser bash
```

- To run the data extraction module execute the below comand
```
python data_extraction.py
```

## Data Cleaning
- This module uses python Faust for stream processing data from kafka topic and persist the data on to the elastic search
- The applicaion is simple and scalable

### Requirements
- Requires that the kafka zookeeper is running
- Requires that the Elasticsearch cluster is up

### To Run
- Set up the parameters kafka topic, elasticsearch broker on in config.py
- Use the docker file provided to set up the container and to install the required packages
- To attach to the running container
```
docker exec -it kafka-consumer-filter bash
```
- To run the data extraction module execute the below comand
```
faust -A data_cleaning worker -l info --web-port 6067
```
- Multiple nodes can be intintiated on the same system on different ports
