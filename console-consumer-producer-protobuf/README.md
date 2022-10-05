 Tutorial 2 - Console Producer and Consumer with (de)serializers **Protbuf**
 
[source](https://docs.confluent.io/platform/current/schema-registry/serdes-develop/serdes-protobuf.html#schema-references-in-protobuf)
## 1. Get Confluent Platform
Launch Confluent Platform by running
```
docker-compose up -d
```
## 2. Create the Kafka topic
In the same terminal run the following to create new Kafka topic
```
docker-compose exec broker kafka-topics --create --topic orders-proto --bootstrap-server broker:9092
```
## 3. Start a console consumer
From the same terminal you used to create the topic, run the following command to open a terminal on the broker container.
```
docker-compose exec schema-registry bash
```
From within the terminal on the broker container, run this
command to start a console consumer
```
kafka-protobuf-console-consumer \
  --topic orders-proto \
  --bootstrap-server broker:9092 \
  --property schema.registry.url=http://localhost:8081
  ```
The consumer will start up and block waiting for records.
## 4. Produce your first records
Open another terminal window and run the following command to open a second shell on the broker container.
```
docker-compose exec schema-registry bash
```
From inside the second terminal on the broker container, run the following command to start a console producer. **Property value.schema refers to the data schema definition passed as an command argument**
```
kafka-protobuf-console-producer \
  --topic orders-proto \
  --bootstrap-server broker:9092 \
  --property schema.registry.url=http://localhost:8081 \
  --property value.schema='syntax = "proto3"; message Order {
    int32 order_id = 1;
    string order_date = 2;
    int32 order_amount = 3;
}'
```
The producer starts waiting for the imput. Type the following data and go back to the console consumer window and look for the output.
```
{"order_id": 1223, "order_date": "2019/07/08", "order_amount":3}
{"order_id": 2345, "order_date": "2020/10/18", "order_amount":1}
```
Close the consumer and producer by entering```CTRL+C```
## 6. Clean Up
Stop any console producers and consumers with a ```CTRL+C``` then close the container shells with a ```CTRL+D``` command.
Then you can shut down the stack by running:
```
docker-compose down
```