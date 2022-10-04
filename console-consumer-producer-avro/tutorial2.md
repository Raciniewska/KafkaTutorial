# Tutorial 2 - Console Producer and Consumer with (de)serializers

## 1. Get Confluent Platform
Launch Confluent Platform by running
```
docker-compose up -d
```
## 2. Create the Kafka topic
In the same terminal run the following to create new Kafka topic
```
docker-compose exec broker kafka-topics --create --topic orders-avro --bootstrap-server broker:9092
```
## 3. Start a console consumer
From the same terminal you used to create the topic, run the following command to open a terminal on the broker container.
```
docker-compose exec schema-registry bash
```
From within the terminal on the broker container, run this command to start a console consumer
```
kafka-avro-console-consumer \
  --topic orders-avro \
  --bootstrap-server broker:9092 \
  --property schema.registry.url=http://localhost:8081
  ```
  The consumer will start up and block waiting for records.
## 4. Produce your first records
Open another terminal window and run the following command to open a second shell on the broker container.
```
docker-compose exec schema-registry bash
```
From inside the second terminal on the broker container, run the following command to start a console producer. **Property value.schema refers to the data schema definition file - *orders-avro-schema.json***
```
kafka-avro-console-producer \
  --topic orders-avro \
  --bootstrap-server broker:9092 \
  --property schema.registry.url=http://localhost:8081 \
  --property value.schema="$(< /etc/tutorial/orders-avro-schema.json)"
```
The producer starts waiting for the imput. Type the following data and go back to the console consumer window and look for the output.
```
{"number":1,"shipping_address":"ABC Sesame Street,Wichita, KS. 12345","subtotal":110.00,"tax":10.00,"grand_total":120.00,"shipping_cost":0.00}
{"number":2,"shipping_address":"123 Cross Street,Irving, CA. 12345","subtotal":5.00,"tax":0.53,"grand_total":6.53,"shipping_cost":1.00}
{"number":3,"shipping_address":"5014  Pinnickinick Street, Portland, WA. 97205","subtotal":93.45,"tax":9.34,"grand_total":102.79,"shipping_cost":0.00}
```
Close the consumer and producer by entering```CTRL+C```
## 5. Produce records with full key-value pairs
Kafka works with key-value pairs, but so far you’ve only sent records with values only. Well to be fair you’ve sent key-value pairs, but the keys are null.      

To enable sending full key-value pairs from the command line you add two properties to your console producer, *parse.key* and *key.separator*. Since we want the key to use String and not a schema, also set the configuration parameter for *key.serializer*.

In the new console producer terminal run:
```
kafka-avro-console-producer \
  --topic orders-avro \
  --bootstrap-server broker:9092 \
  --property schema.registry.url=http://localhost:8081 \
  --property value.schema="$(< /etc/tutorial/orders-avro-schema.json)" \
  --property key.serializer=org.apache.kafka.common.serialization.StringSerializer \
  --property parse.key=true \
  --property key.separator=":"
```
Enter sample data
```
6:{"number":6,"shipping_address":"9182 Shipyard Drive, Raleigh, NC. 27609","subtotal":72.00,"tax":3.00,"grand_total":75.00,"shipping_cost":0.00}
7:{"number":7,"shipping_address":"644 Lagon Street, Chicago, IL. 07712","subtotal":11.00,"tax":1.00,"grand_total":14.00,"shipping_cost":2.00}
```
## 5. Start a consumer to show full key-value pairs and read from the topic
Since the key was serialized as just a String and not a schema, also set the configuration parameter for *key.deserializer*. start the consumer by running the following command.
```
kafka-avro-console-consumer \
  --topic orders-avro \
  --property schema.registry.url=http://localhost:8081 \
  --bootstrap-server broker:9092 \
  --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
  --property print.key=true \
  --property key.separator="-" \
  --from-beginning
```
After the consumer starts you should see the output in a few seconds.
## 6. Clean Up
Stop any console producers and consumers with a ```CTRL+C``` then close the container shells with a ```CTRL+D``` command.
Then you can shut down the stack by running:
```
docker-compose down
```



 