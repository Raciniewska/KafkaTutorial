# Console Producer and Consumer Basics
[source](https://developer.confluent.io/tutorials/kafka-console-consumer-producer-basics/kafka.html)
## 1. Launch Confluent Platform
Run the following command in the terminal.
```docker-compose up -d```
## 2. Create the Kafka topic
Run in the same terminal.
```docker-compose exec broker kafka-topics --create --topic orders --bootstrap-server broker:9092```
## 3. Start a console consumer
Run in the same (consumer) terminal, to open up a console consumer to read records sent to the topic you created in the previous step.
```docker-compose exec broker bash``` 
Start console consumer
```
kafka-console-consumer  --topic orders  --bootstrap-server broker:9092 
  ```
The consumer will start up and block waiting for records, you won’t see any output until after the next step.
 ## 4. Produce your first records
 in new (producer) terminal execute o open a second shell on the broker container 
 ```
 docker-compose exec broker bash 
 ```
From inside the second terminal on the broker container, run the following command to start a console producer
 ```
 kafka-console-producer \
  --topic orders \
  --bootstrap-server broker:9092
  ```
  Type your input and view it in consumer app.
```
hi
The 
weather 
is
nice
```
## 5. Read all records
To read all previously sent records add one property --from-beginning when starting the consumer
```
kafka-console-consumer \
--topic orders \
--bootstrap-server broker:9092 \
--from-beginning
``` 
## 6. Produce and Read records with full key-value pairs 
Run the producer with parse.key and key.separator properties
```
kafka-console-producer \
  --topic orders \
  --bootstrap-server broker:9092 \
  --property parse.key=true \
  --property key.separator=":"
  ```
Enter new records to producer terminal
```
key1: nice day
key1: fresh air
foo:bar
fun:programming
```
Run the consumer with parse.key and key.separator properties
```
kafka-console-consumer \
  --topic orders \
  --bootstrap-server broker:9092 \
  --from-beginning \
  --property print.key=true \
  --property key.separator="-"
```
After the consumer starts you should see the output in a few seconds

## 7. Clean up 
Shut down the stack by running following command in consumer and producer terminal.
```
docker-compose down
```
