# KafkaTutorial
source: https://developer.confluent.io/tutorials/creating-first-apache-kafka-producer-application/kafka.html
Basic Kafka tutorial
## step 1 - Get Confluent Platform:
```cd kafka-producer-application ```\
```docker-compose up -d```
## step 2 - Create a topic:
Open a new terminal window and then run this command to open a 
shell on the broker docker container\
```docker-compose exec broker bash ```\
```kafka-topics --create --topic output-topic --bootstrap-server broker:9092 --replication-factor 1 --partitions 1```
## step 3 - Configure the project:
```gradle wrapper```\
