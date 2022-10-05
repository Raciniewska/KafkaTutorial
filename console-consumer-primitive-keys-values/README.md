# Tutorial 3 - How to use the console consumer to read non-string primitive keys and values

[source](https://developer.confluent.io/tutorials/kafka-console-consumer-primitive-keys-values/kafka.html)

## 1. Get Confluent Platform
Currently, the console producer only writes strings into Kafka, but we want to work with non-string primitives and the console consumer. So in this tutorial, docker-compose.yml file will also create a source connector embedded in *ksqldb-server* to populate a topic - *example* with keys of type long and values of type double.

Run the following command and wait 30 seconds to a 1 minute before executing the next step.
```
docker-compose up -d
```
**Note:** Dockerfile was added in order to resolve internal Kainos certificate problem related to ZScaler security in ksql-server container
## 2. Start an initial console consumer
Open a new terminal window and start a shell in the broker container.
```
docker-compose exec broker bash
```
start up a console consumer to read some records. Run this command in the container shell:
```
kafka-console-consumer --topic example --bootstrap-server broker:9092 \
 --from-beginning \
 --property print.key=true \
 --property key.separator=" : "
```
Similar unreadble output should be visible.
```
-@MA : ?V??oA
? : ?Hʌ??Ue
? : ?5?ͻ?
?? : @?p???
?6_ : ?8???~k@
M : ?P?t?GV
^?: @F>P
 : @ ????
 : @Pc6Ŏ??
```
Close the consumer with a ```Ctrl+C``` command, but keep the container shell open.
## 3. Specify key and value deserializers
To specify the deserializer for keys and values we need to update command to the console consumer by adding parameters **```--key-deserializer``` and ```--value-deserializer```**.
```
kafka-console-consumer --topic example --bootstrap-server broker:9092 \
 --from-beginning \
 --property print.key=true \
 --property key.separator=" : " \
 --key-deserializer "org.apache.kafka.common.serialization.LongDeserializer" \
 --value-deserializer "org.apache.kafka.common.serialization.DoubleDeserializer"
```
After the consumer starts you should see readable numbers similar to this:
```
759188801 : -6.0847354
438 : -49.582416
3803 : -21.936733
45160434 : 2.9518748
15939167 : -24.615814
589 : -66.163364
35 : 44.485178
0 : 8.3665621
4 : 65.550218
12 : 51.10404
```
## 4. Clean up

lose the consumer with a ```Ctrl+C``` command and container with ```Ctrl+D``` Then you can shut down the docker container by running:
```
docker-compose down
```