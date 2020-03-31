
## Send and consume Avro data from Kafka using schema registry

- Create a fresh Kafka Topic ```agkafkaschemareg```
```
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agkafkaschemareg --zookeeper $KAFKAZKHOSTS
```

- Use the **Kafka Avro Console Producer** to create a schema , assign the schema to the Topic and start sending data to the topic in Avro format. 

``` 
/usr/bin/kafka-avro-console-producer     --broker-list $KAFKABROKERS     --topic agkafkaschemareg     --property parse.key=true --property key.schema='{"type" : "int", "name" : "id"}'     --property value.schema='{ "type" : "record", "name" : "example_schema", "namespace" : "com.example", "fields" : [ { "name" : "cust_id", "type" : "int", "doc" : "Id of the customer account" }, { "name" : "year", "type" : "int", "doc" : "year of expense" }, { "name" : "expenses", "type" : {"type": "array", "items": "float"}, "doc" : "Expenses for the year" } ], "doc:" : "A basic schema for storing messages" }'
```

- Note that schema
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjQ3MzkzMDMsNjA3NTY1OTMwXX0=
-->